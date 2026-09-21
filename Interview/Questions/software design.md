
## Design Principles

- Что такое SOLID и какие проблемы решают эти принципы?
- Что означают принципы SOLID и как они проявляются на практике?
- Что такое DRY, KISS и YAGNI? Когда следование этим принципам может навредить?
- Что такое separation of concerns?
- Почему composition обычно предпочитают inheritance?


## Coupling and Cohesion

- Что такое coupling и cohesion?
- Почему обычно стремятся к low coupling и high cohesion?
- По каким признакам можно понять, что компоненты системы слишком сильно связаны?


## Dependency Management

- Что такое Dependency Injection и зачем он нужен?
- Что такое Dependency Inversion и чем он отличается от Dependency Injection?
- Какие преимущества DI даёт для тестирования и замены реализаций?
- Что такое Dependency Injection Container и всегда ли он нужен?


## Application Architecture

### Layered Architecture

- Что такое Layered Architecture и какие слои обычно выделяют в backend-приложении?
- За что должны отвечать Controller/Handler, Service и Repository?
- Как должны зависеть друг от друга слои приложения?
- Где должна находиться business logic?


### Clean Architecture

- Что такое Clean Architecture и какую проблему она решает?
- Какие слои обычно выделяют в Clean Architecture?
- Что такое Dependency Rule и в какую сторону должны направляться зависимости?
- Как Clean Architecture позволяет отделить business logic от БД, HTTP framework и других внешних систем?
- Какие недостатки и trade-offs есть у Clean Architecture?


### Hexagonal Architecture

- Что такое Hexagonal Architecture (Ports and Adapters)?
- Что такое ports и adapters?
- Чем Hexagonal Architecture отличается от Clean Architecture?


## Service Layer

- Что такое Service Layer и за что он должен отвечать?
- Какая логика должна находиться в Service, а какая — в Handler/Controller или Repository?
- Чем application service отличается от domain service?
- Когда Service превращается в слишком большой God Object и как этого избежать?


## Repository

- Что такое Repository Pattern и какую проблему он решает?
- Должен ли Repository содержать business logic?
- Должен ли Repository скрывать конкретную БД/ORM от остального приложения?
- В каких случаях Repository является полезной абстракцией, а когда становится лишним слоем?


## Unit of Work

- Что такое Unit of Work и какую проблему он решает?
- Как Unit of Work связан с Repository и транзакциями?
- Где должна определяться граница транзакции в приложении?


## Design Patterns

- Что такое design pattern и зачем нужны паттерны проектирования?
- Какие существуют группы паттернов: creational, structural и behavioral?
- Какие паттерны ты использовал на практике и какие проблемы они решали?
- Как работают Strategy, Factory, Adapter, Decorator и Observer?
- Когда использование design patterns приводит к unnecessary complexity?


## Domain-Driven Design

### Fundamentals

- Что такое Domain-Driven Design и для каких систем он полезен?
- Что такое domain, subdomain и bounded context?
- Что такое ubiquitous language и зачем он нужен?
- Чем Entity отличается от Value Object?
- Что такое Aggregate и Aggregate Root?
- Что такое Domain Service и Domain Event?


### DDD and Architecture

- Как DDD сочетается с Clean/Hexagonal Architecture?
- Где должна находиться domain logic?
- Должна ли domain model зависеть от базы данных, ORM или web framework?
- Что такое Anemic Domain Model и всегда ли она является проблемой?


## Code Quality and Refactoring

- Что такое code smell?
- Какие признаки говорят о том, что код или класс имеет слишком много ответственности?
- Что такое technical debt?
- Когда стоит делать refactoring, а когда изменение архитектуры не оправдано?