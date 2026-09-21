
## Architecture Fundamentals
- Какие характеристики системы влияют на архитектурные решения: scalability, availability, reliability, maintainability?
- Чем vertical scaling отличается от horizontal scaling?
## Architecture Styles

- Какие основные архитектурные стили существуют и в каких случаях они применяются?
- Чем Monolith, Modular Monolith и Microservices отличаются друг от друга?
- Какие преимущества и недостатки есть у микросервисной архитектуры?
- Когда стоит выбрать монолит, а когда микросервисы?
- Что такое Event-Driven Architecture и для каких задач она подходит?

## Microservices

- По каким принципам следует определять границы микросервисов?
- Должны ли разные микросервисы использовать одну базу данных?
- Почему обычно используется подход Database per Service?
- Какие еще есть антипаттерны?
- Как организовать взаимодействие между микросервисами?
- Когда использовать synchronous, а когда asynchronous communication?
- Какие проблемы появляются при переходе от монолита к микросервисам?

## Communication

- Какие способы взаимодействия сервисов существуют: HTTP/REST, RPC/gRPC, message broker?
- В каких случаях использовать синхронное, а в каких асинхронное взаимодействие?
- Какие trade-offs возникают при использовании message broker вместо прямого HTTP/RPC?
- Что такое service discovery и зачем он нужен?

## Distributed Systems

### CAP and Consistency

- Что такое CAP theorem?
- Что означают Consistency, Availability и Partition Tolerance в CAP?
- Что такое strong consistency и eventual consistency?
- В каких случаях eventual consistency является приемлемой?
- Что такое read-after-write consistency?

### Replication and Partitioning

- Зачем в распределённых системах нужны replication и partitioning?
- Какие проблемы возникают при репликации данных?
- Как replication lag влияет на consistency?
- Как выбор partition/shard key влияет на масштабируемость системы?


## Reliability and Fault Tolerance

- Что такое fault tolerance и graceful degradation?
- Как правильно работать с timeout и retry при взаимодействии сервисов?
- Почему retries могут усугубить проблему и зачем нужен exponential backoff с jitter?
- Что такое Circuit Breaker и какую проблему он решает?
- Что такое rate limiting и зачем он нужен?
- Что такое backpressure и как система может с ним работать?

## Idempotency

- Что такое idempotency и зачем она нужна в распределённых системах?
- Как сделать обработку HTTP request или сообщения идемпотентной?
- Что такое idempotency key и как он используется?
- Почему retries и `at-least-once` delivery требуют идемпотентности?

## Distributed Locks

- Что такое distributed lock и в каких случаях он нужен?
- Какие проблемы возникают при реализации distributed lock?
- Как можно реализовать distributed lock с помощью Redis или базы данных?
- Когда distributed lock лучше вообще не использовать?

## Distributed Transactions

- Почему обычная database transaction не решает проблему транзакции между несколькими сервисами?
- Какие подходы существуют для обеспечения согласованности данных между несколькими сервисами?
- Что такое Two-Phase Commit (2PC) и какие у него недостатки?
- Чем distributed transaction отличается от eventual consistency?

## Saga Pattern

- Что такое Saga и какую проблему она решает?
- Что такое compensating transaction?
- Чем choreography отличается от orchestration в Saga?
- Какие преимущества и недостатки есть у choreography и orchestration?
- Какие проблемы возникают при реализации Saga?


## Transactional Outbox

- Какую проблему решает Transactional Outbox?
- Почему небезопасно отдельно записывать данные в БД и отправлять сообщение в broker?
- Как работает Transactional Outbox?
- Каким образом сообщения могут доставляться из Outbox в message broker?
- Может ли Outbox доставить сообщение несколько раз и как с этим работать?

## CQRS

- Что такое CQRS и какую проблему он решает?
- Что такое Command и Query в контексте CQRS?
- Обязательно ли CQRS означает использование разных баз данных для чтения и записи?
- Какие преимущества и недостатки даёт CQRS?
- В каких системах CQRS оправдан, а где он создаёт лишнюю сложность?


## Event Sourcing

- Что такое Event Sourcing?
- Чем хранение событий отличается от хранения текущего состояния объекта?
- Как восстановить состояние объекта при использовании Event Sourcing?
- Какие преимущества и проблемы есть у Event Sourcing?
- Как связаны Event Sourcing и CQRS?
- Обязательно ли использовать CQRS вместе с Event Sourcing?


## Data Consistency Between Services

- Как поддерживать согласованность данных между несколькими микросервисами без общей транзакции?
- Что делать, если один сервис успешно выполнил операцию, а вызов другого сервиса завершился ошибкой?
- Как проектировать систему, если одно и то же бизнес-действие может быть выполнено повторно?
- Как бороться с duplicate messages и duplicate requests?


## Architecture Patterns

- Что такое API Gateway и какие задачи он решает?
- Что такое Backend for Frontend (BFF)?
- Что такое Sidecar Pattern?
- Что такое Strangler Fig Pattern и как он используется при миграции с монолита на микросервисы?