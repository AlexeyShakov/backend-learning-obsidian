
## Fundamentals

- Что такое message broker и зачем он нужен?
- Какие задачи решают брокеры сообщений?
- Какие преимущества и недостатки асинхронного взаимодействия через брокер по сравнению с синхронным HTTP?
- Основные параметры для настройки(из конфига)?


# Kafka

## Architecture

- Как устроена Kafka?
- Что такое broker в Kafka?
- Что такое topic?
- Какие основные параметры topic нужно знать?


## Partitions

- Что такое partition?
- Зачем topic разбивается на partitions?
- Как Kafka определяет, в какую partition отправить сообщение?
- Как связаны message key и partition?
- Как количество partitions влияет на производительность и параллелизм обработки?


## Replication

- Что такое replica в Kafka?
- Чем partition отличается от replica?
- Зачем Kafka реплицирует partitions?
- Что такое leader и follower replica?
- Что произойдёт, если broker с leader partition станет недоступен?


## Producers

- Как producer отправляет сообщения в Kafka?
- Какие основные настройки producer влияют на надёжность доставки?
- Что такое `acks`?
- Какие значения может принимать `acks` и на что они влияют?
- Что такое idempotent producer и зачем он нужен?


## Consumers and Consumer Groups

- Что такое consumer?
- Что такое consumer group?
- Как partitions распределяются между consumers одной consumer group?
- Что произойдёт, если partitions больше, чем consumers?
- Что произойдёт, если consumers больше, чем partitions?
- Может ли одна partition одновременно читаться несколькими consumers одной consumer group?
- Что такое consumer group rebalance?
- В каких случаях происходит rebalance?


## Offsets

- Что такое offset?
- Как Kafka хранит прогресс чтения consumer?
- Что значит commit offset?
- В какой момент consumer должен commit offset?
- Что такое auto commit и manual commit?
- Что произойдёт, если consumer обработал сообщение, но не успел commit offset?
- Что произойдёт, если offset был committed до того, как сообщение было успешно обработано?


## Consumer Lag and Monitoring

- Что такое consumer lag?
- Как по offset определить lag consumer?
- Почему большой consumer lag является проблемой?
- Как мониторить Kafka?
- Какие основные метрики Kafka стоит мониторить?


## Message Ordering

- Какие гарантии порядка сообщений предоставляет Kafka?
- Гарантирует ли Kafka порядок сообщений между разными partitions?
- Как обеспечить порядок сообщений?
- Как обеспечить порядок обработки сообщений, если consumer читает несколько partitions?
- Как message key связан с гарантией порядка сообщений?


## Delivery Semantics

- Какие гарантии доставки сообщений существуют?
- Что такое `at-most-once`?
- Что такое `at-least-once`?
- Что такое `exactly-once`?
- Как момент commit offset влияет на гарантии доставки?
- Почему при `at-least-once` consumer должен уметь обрабатывать дубликаты?
- Как обеспечить идемпотентную обработку сообщения?


## Message

- Какие метаданные есть у сообщения Kafka?
- Что такое key и value?
- Какие ещё данные кроме payload доступны consumer?
- Для чего нужны headers?


## Outbox Pattern

- Какую проблему решает Transactional Outbox?
- Почему нельзя просто сохранить данные в БД, а затем отправить сообщение в Kafka?
- Что произойдёт, если запись в БД прошла успешно, а отправка сообщения в Kafka завершилась ошибкой?
- Как работает Transactional Outbox?
- Какие существуют способы доставки сообщений из outbox в Kafka?

# RabbitMQ

## Architecture

- Как устроен RabbitMQ?
- Какие основные компоненты есть в RabbitMQ?
- Что такое producer, consumer и queue?
- Что такое exchange и зачем он нужен?
- Как сообщение попадает от producer к нужной queue?
- Что такое binding?
- Какие типы exchange существуют и чем они отличаются?


## Message Delivery

- Как consumer получает сообщения из RabbitMQ?
- Что такое acknowledgement (`ack`)?
- Что такое `nack`?
- Что произойдёт, если consumer получил сообщение, но умер до отправки `ack`?
- Когда сообщение удаляется из queue?
- Какие гарантии доставки можно реализовать с RabbitMQ?
- Как реализуется `at-most-once`?
- Как реализуется `at-least-once`?
- Может ли consumer получить одно сообщение несколько раз?
- Почему consumer должен быть идемпотентным?


## Dead Letter Queue

- Что такое Dead Letter Queue (DLQ)?
- Зачем нужна DLQ?
- В каких случаях сообщение попадает в DLQ?
- Что такое Dead Letter Exchange (DLX)?
- Как организовать retry сообщений с помощью RabbitMQ?
- Как избежать бесконечного retry проблемного сообщения?


## Scalability

- Как масштабировать consumers в RabbitMQ?
- Что произойдёт, если несколько consumers читают одну queue?
- Как RabbitMQ распределяет сообщения между consumers?
- Как масштабируется сам RabbitMQ?
- Какие ограничения возникают при масштабировании одной queue?
- Как обеспечить high availability RabbitMQ?


# Kafka vs RabbitMQ

## Architecture

- В чём основные архитектурные различия Kafka и RabbitMQ?
- Чем Kafka topic/partition отличается от RabbitMQ queue?
- Чем routing через RabbitMQ exchange отличается от partitioning в Kafka?
- Что происходит с сообщением после его обработки consumer в Kafka и RabbitMQ?


## Pull vs Push

- Что такое pull-модель получения сообщений?
- Что такое push-модель?
- Какую модель использует Kafka?
- Какую модель использует RabbitMQ?
- Какие преимущества и недостатки есть у push и pull моделей?
- Как модель получения сообщений влияет на backpressure?


## Delivery and Storage

- Как отличаются гарантии доставки Kafka и RabbitMQ?
- Как Kafka и RabbitMQ работают с повторной доставкой сообщений?
- Как отличаются механизмы acknowledgement в Kafka и RabbitMQ?
- Как Kafka и RabbitMQ хранят сообщения?
- Можно ли повторно прочитать уже обработанное сообщение?
- Как реализуются retry и DLQ в Kafka и RabbitMQ?


## Scalability

- Как масштабируется Kafka?
- Как масштабируется RabbitMQ?
- Как partitions позволяют Kafka масштабировать обработку?
- Как queues и consumers масштабируются в RabbitMQ?
- Какие ограничения на параллелизм существуют у Kafka?
- Какие ограничения на масштабирование существуют у RabbitMQ?


## Use Cases

- В каких задачах лучше подходит Kafka?
- В каких задачах лучше подходит RabbitMQ?
- Когда нужен event log / event streaming?
- Когда нужна классическая task queue?
- Какой брокер лучше подходит для большого потока событий?
- Какой брокер удобнее для сложной маршрутизации сообщений?
- Как выбрать между Kafka и RabbitMQ для конкретной задачи?