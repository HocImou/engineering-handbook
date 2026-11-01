# Domain-Driven Design (DDD) – Fiche mémo

> **Objectif :** retrouver rapidement les grands principes du Domain-Driven Design (DDD), comprendre le rôle de chaque concept et savoir quand l'utiliser.

---

## 1. Qu'est-ce que le DDD ?

Le **Domain-Driven Design** est une approche de conception logicielle qui place le métier au centre du développement.

Son objectif est de construire un modèle logiciel qui reflète fidèlement les concepts, les règles et les processus métier.

### Les deux dimensions du DDD

| DDD stratégique | DDD tactique |
|---|---|
| Comprendre et découper le domaine métier | Construire un modèle métier cohérent |
| Subdomains | Entity |
| Bounded Context | Value Object |
| Ubiquitous Language | Aggregate |
| Context Mapping | Repository |
| | Domain Service |
| | Domain Event |
| | Factory |

**À retenir :** le DDD commence par la compréhension du métier, pas par le choix des technologies ou la création des tables SQL.

---

## 2. DDD stratégique : structurer le domaine métier

### Domain

Le domaine représente le secteur d'activité ou le problème métier auquel l'application répond.

Exemple : gestion de commandes, gestion bancaire, gestion de stocks.

### Subdomains

Un domaine peut être découpé en plusieurs sous-domaines :

- **Core Domain :** cœur de métier, ce qui apporte le plus de valeur et différencie l'entreprise.
- **Supporting Subdomain :** fonctionnalité spécifique qui soutient le cœur métier.
- **Generic Subdomain :** fonctionnalité générique que l'on peut généralement réutiliser ou externaliser.

**À retenir :** un Subdomain est un découpage métier, pas un découpage technique ni nécessairement un microservice.

### Bounded Context

Un Bounded Context définit une frontière à l'intérieur de laquelle un modèle métier et son vocabulaire ont une signification précise.

Un même terme peut avoir des significations différentes selon le contexte.

Exemple : le terme « Client » peut représenter des informations différentes dans le contexte commercial et dans le contexte facturation.

**À retenir :**
- Chaque Bounded Context possède son propre modèle métier.
- Le modèle d'un contexte ne doit pas être imposé aux autres.
- Un Bounded Context n'est pas automatiquement un microservice.

### Ubiquitous Language

C'est le langage commun partagé entre les développeurs et les experts métier.

Il doit se retrouver dans :
- Les noms des classes et des méthodes.
- Les attributs et les exceptions.
- Les cas d'utilisation.
- Les tests.
- La documentation métier.

**À retenir :** utiliser le vocabulaire métier plutôt que des noms techniques génériques.

---

## 3. DDD tactique : construire le modèle métier

### Entity

Une Entity est un objet métier possédant une identité propre et un cycle de vie.

Ses attributs peuvent évoluer sans que son identité change.

Exemple : une commande identifiée par un UUID.

**Quand l'utiliser ?**
Lorsqu'un objet doit être suivi individuellement au cours de son existence.

### Value Object

Un Value Object est défini par ses valeurs et non par une identité.

Caractéristiques :
- Généralement immuable.
- Égalité basée sur ses valeurs.
- Peut encapsuler des validations et des règles métier.
- Représente un concept métier cohérent.

Exemples : `AdresseEmail`, `Montant`, `AdressePostale`, `Periode`.

**Quand l'utiliser ?**
Lorsqu'une donnée représente un concept métier avec une signification et éventuellement des règles propres.

### Aggregate

Un Aggregate est un ensemble d'objets métier regroupés autour d'une unité de cohérence.

Il protège les invariants métier qui doivent toujours être respectés.

Il possède une **Aggregate Root**, qui constitue le seul point d'entrée pour modifier l'Aggregate.

Exemple :

- `Commande` : Aggregate Root.
- `LigneCommande` : objet interne.
- `ProduitReference` : Value Object éventuel.

**Règles essentielles :**
- Les modifications passent par l'Aggregate Root.
- Les invariants sont protégés par la Root.
- Les autres Aggregates sont généralement référencés par leur identifiant.
- Garder les Aggregates aussi petits que possible.
- Un Repository est généralement associé à chaque Aggregate Root.

### Repository

Le Repository fournit une abstraction pour accéder aux Aggregates et les sauvegarder.

Exemple :

```java
public interface CommandeRepository {

    Optional<Commande> findById(UUID id);

    void save(Commande commande);
}
```

L'interface exprime le besoin métier ou applicatif ; son implémentation technique peut utiliser JPA, JDBC ou une autre technologie.

**À retenir :**
- Le domaine ne dépend pas de la base de données.
- Le Repository ne doit pas exposer les détails techniques de persistance au domaine.
- Éviter de créer un Repository pour chaque classe.

### Domain Service

Un Domain Service porte une règle métier qui ne trouve pas naturellement sa place dans une Entity ou un Aggregate.

Il est utile notamment lorsqu'une règle implique plusieurs Aggregates indépendants.

**Attention :** une règle qui protège l'invariant d'un Aggregate doit rester dans son Aggregate Root.

### Application Service

L'Application Service orchestre un cas d'utilisation.

Il :
- Récupère les objets nécessaires.
- Appelle les méthodes métier.
- Coordonne les services et les dépendances.
- Déclenche la persistance.
- Gère les transactions au niveau approprié.

**Différence essentielle :**

- Domain Service : contient une règle métier.
- Application Service : orchestre un scénario métier.

### Domain Event

Un Domain Event représente un fait métier qui s'est produit.

Exemples :
- `CommandeCreee`
- `CommandeValidee`
- `PaiementEffectue`

**À retenir :**
- Un événement est exprimé au passé.
- Il permet de découpler les réactions à un événement métier.
- Il ne doit pas être confondu avec une commande, qui exprime une intention.
- Pour publier des événements de manière fiable vers Kafka, le pattern Transactional Outbox est une solution à étudier.

### Factory

Une Factory centralise la création d'objets métier lorsque cette création est complexe.

Elle garantit que l'objet est construit dans un état initial cohérent.

Trois possibilités :
- Constructeur classique.
- Méthode factory statique.
- Factory dédiée.

**À retenir :** ne pas créer une Factory systématiquement pour chaque classe.

---

## 4. Comment choisir le bon concept ?

| Question à se poser | Concept à envisager |
|---|---|
| L'objet possède-t-il une identité et un cycle de vie ? | Entity |
| Représente-t-il une valeur sans identité propre ? | Value Object |
| Plusieurs objets doivent-ils respecter des invariants communs ? | Aggregate |
| Une règle métier ne trouve-t-elle sa place dans aucun objet ? | Domain Service |
| Faut-il orchestrer un cas d'utilisation ? | Application Service |
| Faut-il accéder aux Aggregates sans exposer la persistance ? | Repository |
| Faut-il représenter un fait métier passé ? | Domain Event |
| La création d'un objet est-elle complexe ? | Factory |

---

## 5. Les règles d'or du DDD

1. **Le métier avant la technique :** comprendre les règles métier avant de concevoir les classes et les tables.
2. **Un langage commun :** utiliser les termes métier dans le code.
3. **Des modèles cohérents :** chaque Bounded Context possède son propre modèle.
4. **Des objets responsables :** placer les règles métier au bon endroit.
5. **Des invariants protégés :** un Aggregate doit empêcher les états métier invalides.
6. **Des Aggregates petits :** éviter les transactions et dépendances trop larges.
7. **Une persistance découplée :** le domaine ne doit pas dépendre de JPA, SQL ou Spring.
8. **Une orchestration séparée :** ne pas mélanger les règles métier et les scénarios applicatifs.
9. **Des événements métier explicites :** représenter les faits importants du domaine.
10. **Un modèle évolutif :** faire évoluer le modèle à partir de la compréhension réelle du métier.

---

## 6. DDD et Clean Architecture

DDD et Clean Architecture sont complémentaires :

- **DDD** aide à construire un modèle métier pertinent.
- **Clean Architecture** organise le code pour protéger ce modèle des dépendances techniques.

Exemple d'organisation :

```text
my-context/
├── domain/
│   ├── model/
│   ├── service/
│   ├── event/
│   └── repository/
│
├── application/
│   ├── usecase/
│   └── port/
│       ├── in/
│       └── out/
│
├── infrastructure/
│   ├── persistence/
│   ├── messaging/
│   └── client/
│
└── presentation/
    └── rest/
```

### Sens des dépendances

```text
Presentation
     |
     v
Application
     |
     v
Domain

Infrastructure ---> implémente les ports
```

Le domaine reste indépendant des frameworks et des technologies.

---

## 7. Démarche pratique pour appliquer le DDD

Lorsqu'on démarre une nouvelle fonctionnalité :

1. Comprendre le besoin métier et les acteurs.
2. Identifier les concepts et le vocabulaire métier.
3. Identifier les règles métier et les invariants.
4. Déterminer les Bounded Contexts concernés.
5. Identifier les Entities et Value Objects.
6. Définir les Aggregates et leurs Aggregate Roots.
7. Déterminer les responsabilités des Domain Services.
8. Définir les cas d'utilisation et les Application Services.
9. Identifier les besoins en Repository et les événements métier.
10. Implémenter progressivement et tester les règles métier.

**Ne pas commencer par créer les packages, les entités JPA ou les tables SQL.**

---

## 8. Les erreurs à éviter

| Mauvaise pratique | Pourquoi l'éviter ? |
|---|---|
| Créer un microservice par Entity | Mélange découpage métier et découpage technique |
| Créer un Aggregate trop volumineux | Augmente le couplage et les conflits de concurrence |
| Mettre toute la logique dans les Services | Produit un modèle métier anémique |
| Utiliser JPA directement dans le domaine | Couple le métier à la persistance |
| Créer un Value Object pour chaque attribut | Complexifie inutilement le modèle |
| Créer un Domain Service pour toute règle | Déplace inutilement les responsabilités |
| Publier Kafka directement depuis l'Aggregate | Couple le domaine à la messagerie |
| Créer des interfaces et des abstractions partout | Ajoute de la complexité sans valeur métier |

---

## 9. Checklist avant de valider un modèle DDD

- [ ] Le domaine métier est-il clairement compris ?
- [ ] Le vocabulaire utilisé correspond-il à celui du métier ?
- [ ] Les Bounded Contexts sont-ils clairement délimités ?
- [ ] Les Entities et Value Objects sont-ils justifiés ?
- [ ] Les Aggregates protègent-ils leurs invariants ?
- [ ] Les responsabilités métier sont-elles correctement réparties ?
- [ ] Les Application Services se limitent-ils à l'orchestration ?
- [ ] Le domaine est-il indépendant des technologies ?
- [ ] Les règles métier importantes sont-elles testées ?
- [ ] Le modèle reste-t-il suffisamment simple ?

---

## 10. Références pour approfondir

- Eric Evans — *Domain-Driven Design: Tackling Complexity in the Heart of Software*.
- Vaughn Vernon — *Implementing Domain-Driven Design*.
- Martin Fowler — [Domain-Driven Design](https://martinfowler.com/tags/domain%20driven%20design.html).
- Microsoft — [Domain analysis for microservices](https://learn.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis).

---

> **Principe fondamental à retenir :** le DDD ne consiste pas à multiplier les classes, les interfaces ou les couches. Il consiste à construire un modèle logiciel qui exprime clairement le métier, protège ses règles et facilite son évolution.