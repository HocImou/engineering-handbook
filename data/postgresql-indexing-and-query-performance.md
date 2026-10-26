# PostgreSQL — Indexation et performances des requêtes

## 1. Objectif

Ce document présente les principes et les bonnes pratiques permettant de concevoir, d'utiliser et de valider les index dans PostgreSQL.

L'objectif est d'aider les développeurs et les Tech Leads à prendre des décisions d'indexation fondées sur les requêtes réellement exécutées, la distribution des données et les mesures de performance.

Un index ne doit pas être ajouté systématiquement. Chaque index doit répondre à un besoin identifié et justifier son coût de maintenance.

## 2. Qu'est-ce qu'un index ?

Un index est une structure de données qui permet à PostgreSQL de retrouver des lignes sans nécessairement parcourir l'intégralité d'une table.

Par défaut, PostgreSQL utilise généralement des index de type B-tree pour les opérations relationnelles courantes : comparaisons d'égalité, recherches par intervalle, tris et contraintes d'unicité.

Un index peut améliorer les performances en lecture, mais il entraîne également des coûts :

- Consommation d'espace disque supplémentaire.
- Maintenance lors des insertions, mises à jour et suppressions.
- Augmentation potentielle du coût des écritures.
- Structures supplémentaires à surveiller et à maintenir.

PostgreSQL peut préférer un parcours séquentiel lorsque lire une grande partie d'une table est plus efficace que passer par un index.

**Principe essentiel :** un index représente une possibilité d'optimisation, et non une garantie d'amélioration des performances.

## 3. Les principaux types d'index

### 3.1. B-tree

Le B-tree est le type d'index utilisé par défaut. Il convient à la majorité des requêtes relationnelles classiques.

Exemples d'utilisation :

- Égalité : `WHERE client_id = 42`
- Intervalle : `WHERE created_at >= ...`
- Tri : `ORDER BY created_at DESC`
- Recherche de valeurs uniques.

### 3.2. Les autres types d'index

D'autres types peuvent être utiles selon la nature des données et les opérateurs utilisés.

- **GIN** : adapté à certaines recherches dans les tableaux, les colonnes JSONB et les données de recherche en texte intégral.
- **GiST** : adapté à des structures spécialisées, notamment les données géométriques et les intervalles.
- **BRIN** : potentiellement efficace sur de très grandes tables lorsque l'ordre physique des lignes est corrélé à la valeur indexée, par exemple dans certaines tables de séries temporelles.
- **Hash** : adapté aux comparaisons d'égalité, mais ne remplace pas le B-tree pour les besoins généraux d'indexation.

Ces types ne sont pas interchangeables. Leur pertinence dépend des opérateurs de recherche, de la distribution des données et de la version de PostgreSQL utilisée.

Il est préférable de commencer par un B-tree pour les besoins relationnels classiques, puis d'envisager d'autres types lorsqu'un besoin précis le justifie.

## 4. Les index composites

Un index composite porte sur plusieurs colonnes.

Exemple :

```sql
CREATE INDEX idx_orders_client_created_at
    ON orders (client_id, created_at DESC);
```

Cet index peut être utilisé pour une requête qui récupère les commandes d'un client, de la plus récente à la plus ancienne :

```sql
SELECT id, status, created_at
FROM orders
WHERE client_id = 42
ORDER BY created_at DESC
LIMIT 20;
```

### 4.1. Pourquoi l'ordre des colonnes est-il important ?

Pour un index B-tree composite, les colonnes placées au début de l'index jouent un rôle déterminant dans les possibilités de recherche.

Un index sur `(client_id, created_at)` est particulièrement adapté aux requêtes filtrant sur `client_id`. Il n'est généralement pas équivalent à un index sur `created_at` seul.

Avant de créer un index composite, il faut examiner :

- Les conditions d'égalité et les recherches par intervalle.
- Les colonnes utilisées pour le tri.
- La sélectivité des colonnes, c'est-à-dire leur capacité à réduire le nombre de lignes candidates.
- La fréquence d'exécution de la requête.
- Les index existants susceptibles de couvrir le même besoin.

L'ordre des colonnes ne doit pas être choisi uniquement en fonction de l'ordre des colonnes dans la table.

## 5. Index et contraintes d'intégrité

Les index et les contraintes sont liés, mais ne remplissent pas exactement le même rôle.

### 5.1. Clé primaire

Une clé primaire identifie chaque ligne de manière unique.

PostgreSQL crée automatiquement un index unique pour prendre en charge cette contrainte.

### 5.2. Contrainte UNIQUE

Une contrainte `UNIQUE` garantit l'unicité des valeurs concernées. PostgreSQL crée automatiquement un index B-tree unique pour une contrainte `UNIQUE` classique.

Il faut donc éviter de créer un second index redondant portant sur les mêmes colonnes avec une définition équivalente.

Exemple :

```sql
reference VARCHAR(100) NOT NULL UNIQUE
```

Cette définition suffit à garantir l'unicité de la référence et à fournir un index pour les recherches correspondantes.

### 5.3. Clé étrangère

Une clé étrangère garantit l'intégrité référentielle entre deux tables.

Exemple :

```sql
FOREIGN KEY (order_id) REFERENCES orders(id)
```

PostgreSQL ne crée pas automatiquement d'index sur les colonnes qui portent la clé étrangère dans la table référençante.

Un index sur `order_items(order_id)` peut donc être utile pour retrouver les lignes d'une commande et accélérer certaines opérations de jointure.

Il peut également aider PostgreSQL à localiser les lignes référençant une ligne parente lors de certaines opérations de mise à jour ou de suppression.

L'intérêt de cet index doit être évalué en fonction des requêtes et du volume de données.

## 6. Analyser les performances des requêtes

### 6.1. Utiliser EXPLAIN

La commande `EXPLAIN` permet de consulter le plan d'exécution choisi par PostgreSQL.

Exemple :

```sql
EXPLAIN
SELECT id, status
FROM orders
WHERE client_id = 42
ORDER BY created_at DESC
LIMIT 20;
```

Parmi les opérations que l'on peut rencontrer :

- `Seq Scan` : parcours séquentiel d'une table.
- `Index Scan` : accès par index, suivi éventuellement de la lecture des lignes dans la table.
- `Index Only Scan` : accès pouvant être effectué uniquement à partir de l'index, sous réserve des données nécessaires et des informations de visibilité disponibles.
- `Bitmap Index Scan` et `Bitmap Heap Scan` : stratégie combinant un index et un accès aux lignes par bitmap.

Un `Seq Scan` n'est pas nécessairement un problème de performance. Il peut constituer le choix le plus efficace lorsque la requête doit récupérer une grande partie des lignes.

### 6.2. Utiliser EXPLAIN ANALYZE

`EXPLAIN ANALYZE` exécute réellement la requête et fournit des mesures d'exécution.

Exemple pour une requête de lecture :

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT id, status
FROM orders
WHERE client_id = 42
ORDER BY created_at DESC
LIMIT 20;
```

Il faut notamment examiner :

- Les écarts entre le nombre de lignes estimé et le nombre de lignes réellement traité.
- Le temps d'exécution.
- L'activité des buffers.
- Les stratégies d'accès et de jointure.
- La cohérence entre le plan observé et le besoin métier.

`EXPLAIN ANALYZE` exécute la requête. Il faut donc être prudent avec les instructions qui modifient les données et éviter de lancer sans précaution des requêtes coûteuses en production.

Les mesures doivent être réalisées sur des données représentatives et dans des conditions proches de l'utilisation réelle. Un plan observé sur une petite base de développement peut différer sensiblement de celui obtenu en production.

## 7. Bonnes pratiques d'indexation

1. Partir des requêtes existantes ou prévues.
2. Mesurer les performances avant d'optimiser.
3. Examiner les index existants avant d'en ajouter de nouveaux.
4. Choisir avec soin l'ordre des colonnes dans les index composites.
5. Prendre en compte le coût des écritures et de l'espace disque.
6. Éviter les index redondants.
7. Maintenir des statistiques adaptées pour permettre au planificateur de faire de bons choix.
8. Valider les changements à l'aide de plans d'exécution et de mesures représentatives.
9. Réévaluer les index lorsque l'application, les données ou les requêtes évoluent.

Chaque index devrait avoir un objectif identifiable : accélérer une recherche fréquente, une jointure, un tri ou contribuer à faire respecter une contrainte.

## 8. Exemple pratique : gestion des commandes

Considérons une application composée des tables suivantes :

- `orders` : une ligne par commande.
- `order_items` : une ligne par article d'une commande.
- `products` : une ligne par produit.

Les index suivants peuvent répondre à des besoins identifiés :

```sql
CREATE INDEX idx_orders_client_created_at
    ON orders (client_id, created_at DESC);

CREATE INDEX idx_order_items_order_id
    ON order_items (order_id);

CREATE INDEX idx_order_items_product_id
    ON order_items (product_id);
```

Ces index correspondent à des cas d'utilisation distincts :

- Récupérer les commandes les plus récentes d'un client.
- Récupérer les lignes associées à une commande.
- Rechercher les commandes contenant un produit donné.

Ces propositions constituent des hypothèses initiales de conception, et non des obligations universelles. Leur intérêt doit être vérifié à partir des requêtes réelles, de la distribution des données et des plans d'exécution.

## 9. Liste de vérification avant de créer un index

Avant d'ajouter un index, vérifier les points suivants :

- [ ] Quelle requête ou opération cet index doit-il améliorer ?
- [ ] Un index existant couvre-t-il déjà ce besoin ?
- [ ] L'ordre des colonnes est-il adapté aux filtres et aux tris ?
- [ ] La requête est-elle suffisamment fréquente ou coûteuse pour justifier l'index ?
- [ ] Quel sera l'impact sur les écritures et l'espace disque ?
- [ ] Le plan d'exécution a-t-il été examiné ?
- [ ] Les performances ont-elles été mesurées sur des données représentatives ?

## 10. Références officielles

- [PostgreSQL — Les index](https://www.postgresql.org/docs/current/indexes.html)
- [PostgreSQL — Les types d'index](https://www.postgresql.org/docs/current/indexes-types.html)
- [PostgreSQL — Utilisation de EXPLAIN](https://www.postgresql.org/docs/current/using-explain.html)
- [PostgreSQL — Les contraintes](https://www.postgresql.org/docs/current/ddl-constraints.html)
