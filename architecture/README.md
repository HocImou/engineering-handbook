# Software Architecture

**Language / Langue:** French and English

## 🇫🇷 Français

### Objectif

Cette section définit les principes et les pratiques permettant de concevoir des systèmes logiciels maintenables, évolutifs, testables et sécurisés.

Elle adopte une approche pragmatique, fondée sur des principes d'ingénierie logicielle applicables à différents contextes et illustrée par des exemples issus de l'écosystème Java et Spring Boot.

L'objectif n'est pas d'imposer une architecture unique, mais de fournir un cadre de réflexion pour prendre des décisions techniques cohérentes avec les besoins et les contraintes de chaque système.

### Périmètre

Cette section couvre notamment :

- les principes fondamentaux de conception logicielle ;
- la séparation des responsabilités, la cohésion et le couplage ;
- la modularité et la gestion des dépendances ;
- Clean Architecture et l'architecture hexagonale ;
- les choix entre monolithe modulaire et microservices ;
- les qualités architecturales : sécurité, testabilité, maintenabilité et évolutivité ;
- la documentation et la traçabilité des décisions d'architecture.

### Principes directeurs

Les recommandations de cette section privilégient :

- la simplicité et la proportionnalité de la solution ;
- des responsabilités et des frontières explicites ;
- des dépendances maîtrisées ;
- la protection de la logique métier contre les détails techniques inutiles ;
- la testabilité et la maintenabilité ;
- des décisions justifiées par le contexte, les contraintes et les compromis.

### Documents

- [`architecture-principles.md`](architecture-principles.md) — Principes fondamentaux de conception et recommandations pratiques.
- [`architecture-decision-records/`](architecture-decision-records/) — Décisions d'architecture documentées au fil des besoins.

Les documents seront ajoutés et enrichis progressivement.

---

## 🇬🇧 English

### Purpose

This section defines principles and practices for designing maintainable, evolvable, testable, and secure software systems.

It takes a pragmatic approach based on software engineering principles applicable across different contexts, illustrated with examples from the Java and Spring Boot ecosystem.

The goal is not to prescribe a single architecture, but to provide a framework for making technical decisions that align with each system's needs and constraints.

### Scope

This section covers:

- fundamental software design principles;
- separation of concerns, cohesion, and coupling;
- modularity and dependency management;
- Clean Architecture and Hexagonal Architecture;
- architectural choices between modular monoliths and microservices;
- architectural qualities: security, testability, maintainability, and evolvability;
- architecture documentation and decision traceability.

### Guiding principles

Recommendations in this section favor:

- simplicity and proportionality;
- explicit responsibilities and boundaries;
- controlled dependencies;
- protection of business logic from unnecessary technical details;
- testability and maintainability;
- decisions justified by context, constraints, and trade-offs.

### Documents

- [`architecture-principles.md`](architecture-principles.md) — Fundamental design principles and practical recommendations.
- [`architecture-decision-records/`](architecture-decision-records/) — Architecture decisions documented as the need arises.

Documents will be added and expanded progressively.
