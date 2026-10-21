# Contributing to the Engineering Handbook

**Language / Langue:** French and English  
**Status:** Initial contribution guidelines

## 🇫🇷 Français

### 1. Objectif

Ce document définit les règles permettant de créer, modifier, relire et maintenir le contenu du dépôt `engineering-handbook`.

L'objectif est de maintenir un référentiel de pratiques d'ingénierie logicielle fiable, cohérent, compréhensible et évolutif.

Ces règles s'appliquent à toutes les contributions : nouvelles recommandations, corrections, exemples de code, décisions d'architecture et documentation technique.

### 2. Principes de contribution

Toute contribution doit respecter les principes suivants :

- **Clarté :** expliquer le problème traité et la recommandation proposée.
- **Justification :** expliquer pourquoi une pratique est recommandée, plutôt que de présenter une règle sans contexte.
- **Pragmatisme :** tenir compte du contexte technique, des contraintes et des compromis.
- **Fiabilité :** vérifier l'exactitude des informations et des exemples techniques.
- **Maintenabilité :** privilégier un contenu facile à comprendre, à appliquer et à mettre à jour.
- **Sécurité :** prendre en compte les risques de sécurité et les bonnes pratiques applicables.
- **Traçabilité :** documenter les décisions importantes et leurs justifications.

### 3. Périmètre des contributions

Les contributions peuvent notamment concerner :

- l'architecture logicielle et les principes de conception ;
- les pratiques Java et Spring Boot ;
- le développement frontend et Angular ;
- les tests et la qualité du code ;
- la sécurité applicative ;
- les bases de données et le traitement des données ;
- les pratiques DevOps, CI/CD et observabilité ;
- la documentation technique et les décisions d'architecture.

Chaque contribution doit rester cohérente avec l'objectif du référentiel.

### 4. Structure recommandée d'une recommandation

Lorsqu'une contribution introduit une nouvelle pratique technique, elle devrait présenter les éléments suivants, lorsque cela est pertinent :

1. **Context / Contexte** — Décrire le problème et les situations concernées.
2. **Recommendation / Recommandation** — Énoncer clairement la pratique proposée.
3. **Rationale / Justification** — Expliquer les raisons techniques et les bénéfices attendus.
4. **Example / Exemple** — Fournir un exemple concret si cela facilite la compréhension.
5. **Limitations / Limites** — Préciser les contraintes, les exceptions et les compromis.
6. **References / Références** — Ajouter les sources utiles pour vérifier ou approfondir le sujet.

Cette structure est indicative : elle doit être adaptée à la complexité du sujet.

### 5. Qualité et vérification technique

Avant de proposer une contribution :

- vérifier les informations dans la documentation officielle lorsque cela est possible ;
- préciser les versions des technologies lorsque le comportement dépend de la version ;
- vérifier les exemples de code et les commandes lorsqu'ils sont présentés comme exécutables ;
- distinguer les faits vérifiables, les recommandations et les choix propres à un contexte ;
- signaler les hypothèses, les prérequis et les limites connus ;
- vérifier les liens et supprimer les références obsolètes ;
- ne pas présenter une pratique comme universelle lorsqu'elle dépend du contexte.

Les exemples ne doivent pas exposer de secrets, de données personnelles ou d'informations confidentielles.

### 6. Conventions de documentation

Les documents doivent :

- utiliser Markdown lorsque cela convient ;
- adopter des titres explicites et une structure cohérente ;
- privilégier un langage précis, accessible et professionnel ;
- fournir des exemples utiles plutôt que des explications inutilement longues ;
- utiliser des noms de fichiers et des termes cohérents avec l'organisation du dépôt ;
- éviter les duplications en privilégiant les références vers les documents existants.

Le français et l'anglais peuvent être utilisés. Lorsque cela apporte une réelle valeur, les recommandations importantes peuvent être documentées dans les deux langues. Une traduction ne doit pas modifier le sens technique de la recommandation.

### 7. Processus de contribution

Pour toute modification significative :

1. Identifier le problème, le besoin ou l'amélioration attendue.
2. Vérifier si un document existant couvre déjà le sujet.
3. Créer ou modifier le document concerné.
4. Vérifier la cohérence, les références et les exemples techniques.
5. Relire les changements avant de les proposer.
6. Expliquer clairement l'objectif de la modification dans le commit ou la pull request.

Les corrections simples de typographie ou de liens peuvent suivre un processus allégé.

Lorsqu'une pull request est utilisée, elle doit présenter le contexte, les changements apportés et les éventuels compromis ou impacts.

### 8. Évolution des recommandations

Les recommandations doivent pouvoir évoluer avec les technologies et les besoins des projets.

Lorsqu'une pratique devient obsolète ou qu'une nouvelle information remet en cause une recommandation :

- mettre à jour le document concerné ;
- expliquer les changements importants ;
- conserver les références utiles ;
- préciser les éventuelles conséquences sur les projets qui appliquent déjà cette pratique.

Les décisions d'architecture ayant un impact significatif peuvent être documentées séparément sous forme d'Architecture Decision Record (ADR).

### 9. Utilisation de l'intelligence artificielle

Les outils d'intelligence artificielle peuvent aider à rédiger, analyser ou relire les contributions.

Cependant :

- le contributeur reste responsable du contenu proposé ;
- les informations techniques doivent être vérifiées ;
- les exemples de code doivent être examinés et testés lorsque cela est applicable ;
- les références doivent être vérifiées indépendamment ;
- aucun secret ni contenu confidentiel ne doit être transmis à un outil d'IA non autorisé.

Une contribution générée ou assistée par IA doit respecter les mêmes exigences de qualité qu'une contribution rédigée manuellement.

---

## 🇬🇧 English

### 1. Purpose

This document defines the guidelines for creating, modifying, reviewing, and maintaining the content of the `engineering-handbook` repository.

The objective is to maintain a reliable, consistent, understandable, and evolving software engineering knowledge base.

These guidelines apply to all contributions, including recommendations, corrections, code examples, architecture decisions, and technical documentation.

### 2. Contribution principles

Every contribution should follow these principles:

- **Clarity:** explain the problem being addressed and the proposed recommendation.
- **Rationale:** explain why a practice is recommended instead of presenting rules without context.
- **Pragmatism:** consider technical context, constraints, and trade-offs.
- **Reliability:** verify the accuracy of technical information and examples.
- **Maintainability:** favor content that is easy to understand, apply, and update.
- **Security:** consider relevant security risks and practices.
- **Traceability:** document important decisions and their rationale.

### 3. Scope of contributions

Contributions may cover:

- software architecture and design principles;
- Java and Spring Boot practices;
- frontend development and Angular;
- testing and code quality;
- application security;
- databases and data processing;
- DevOps, CI/CD, and observability;
- technical documentation and architecture decisions.

Every contribution must remain consistent with the purpose of the handbook.

### 4. Recommended structure for a technical recommendation

When introducing a technical practice, include the following elements where relevant:

1. **Context** — Describe the problem and applicable situations.
2. **Recommendation** — Clearly state the proposed practice.
3. **Rationale** — Explain the technical reasoning and expected benefits.
4. **Example** — Provide a practical example when useful.
5. **Limitations** — Describe constraints, exceptions, and trade-offs.
6. **References** — Provide sources for verification and further reading.

This structure is a guideline and should be adapted to the complexity of the subject.

### 5. Technical quality and verification

Before submitting a contribution:

- verify information against official documentation whenever possible;
- specify technology versions when behavior depends on them;
- verify code examples and commands presented as executable;
- distinguish verifiable facts, recommendations, and context-specific choices;
- identify assumptions, prerequisites, and known limitations;
- check links and remove outdated references;
- avoid presenting context-dependent practices as universal rules.

Examples must not expose secrets, personal data, or confidential information.

### 6. Documentation conventions

Documents should:

- use Markdown where appropriate;
- follow a clear and consistent heading structure;
- use precise, accessible, and professional language;
- provide useful examples instead of unnecessarily lengthy explanations;
- use consistent file names and terminology;
- avoid duplication by referencing existing documents where appropriate.

French and English are both accepted. Important recommendations may be documented in both languages when this provides meaningful value. Translations must preserve the technical meaning.

### 7. Contribution workflow

For significant changes:

1. Identify the problem, need, or expected improvement.
2. Check whether an existing document already covers the topic.
3. Create or update the relevant document.
4. Verify consistency, references, and technical examples.
5. Review the changes before proposing them.
6. Clearly explain the purpose of the change in the commit or pull request.

Simple typo or link corrections may follow a lightweight process.

When a pull request is used, it should describe the context, proposed changes, and relevant trade-offs or impacts.

### 8. Evolving recommendations

Recommendations should evolve with technologies and project needs.

When a practice becomes outdated or new information challenges an existing recommendation:

- update the relevant document;
- explain significant changes;
- retain useful references;
- identify potential impacts on projects already following the practice.

Architecture decisions with significant impact may be documented separately as Architecture Decision Records (ADRs).

### 9. Use of artificial intelligence

Artificial intelligence tools may assist with drafting, analysis, and review.

However:

- contributors remain responsible for submitted content;
- technical claims must be verified;
- code examples must be reviewed and tested where applicable;
- references must be independently checked;
- secrets and confidential information must not be submitted to unauthorized AI tools.

AI-generated or AI-assisted contributions must meet the same quality requirements as manually written contributions.
