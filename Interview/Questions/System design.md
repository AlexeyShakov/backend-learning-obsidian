
Чек-лист для решения задач по System Design.

Это не список теоретических вопросов, на которые нужно отдельно написать ответы.
Это последовательность шагов и вопросов, которые нужно пройти при проектировании системы.

Связанные материалы:

- [[System Design Workload Patterns]]
- [[Capacity Planning]]
- [[Databases]]
- [[Caching]]
- [[Message Brokers]]
- [[Web]]
- [[Observability]]
- [[System Architecture]]

---

# 1. Functional Requirements

Сначала определить, **что должна уметь система(набор функций)**.

Вопросы:

- Кто пользуется системой?
- Какие основные сценарии использования?
- Какие операции выполняет пользователь?
- Какие функции являются обязательными, а какие можно оставить за scope задачи?

Дополнительно проверить специфические требования:

- нужен ли real-time?
- нужна ли cross-device synchronization?
- нужен ли поиск?
- есть ли загрузка файлов/media?
- нужны ли notifications?
- нужна ли история изменений?
- нужны ли read/delivery statuses?

Например, для ютуб:
* просмотр видео
* поиск
* сохранение видео
* скачивание
* загрузка видео

---

# 2. Non-functional Requirements

Определить требования и ограничения системы.

## Scale

- DAU(уникальных пользователей в день) / MAU(уникальных пользователей в месяц)?
- Сколько операций выполняет пользователь?
- Какова пиковая нагрузка?
- Есть ли сезонность или резкие всплески?
- есть ли гео-распределенность?

## Performance

- Какая допустимая latency?
- Какие операции наиболее чувствительны к latency?

## Availability

- Какая требуется availability?
- Что должно происходить при недоступности части системы?

## Consistency

- Где нужна strong consistency?
- Где допустима eventual consistency?

## Durability

- Насколько критична потеря данных?
- Можно ли потерять несколько секунд данных при аварии?

## Data

- Сколько времени храним данные?
- Есть ли ограничения на размер сущностей?
- Как быстро растёт объём данных?

## Geography

- Где находятся пользователи?
- Нужна ли работа из нескольких регионов / DC?
## Лимиты
* есть ли лимиты? Например, для социальной сети - это кол-во друзей. Для дискового хранилища - кол-во файлов. Ограничение по размерам файлов
# 3. Capacity Estimation

Не считать всё подряд.

Оценивать только параметры, которые действительно влияют на архитектуру конкретной системы.

Основные:

- read RPS;
- write RPS;
- peak RPS;
- ingress traffic(входящий трафик);
- egress traffic(исходящий трафик);
- storage growth;
- concurrent connections.
- IOPS;
- disk throughput;
- RAM;
- CPU;
- количество hosts.

Подробнее: [[Capacity Planning]]

## RPS

```text
average RPS =
DAU × operations_per_user_per_day / 86 400
```

Отдельно считать:

```text
read RPS
write RPS
peak RPS
```

Средний RPS не показывает пиковую нагрузку.

---
# 4. Workload Classification

Перед проектированием определить характер системы.

Спросить:

> Что здесь является основной нагрузкой и потенциальным bottleneck?

Система может одновременно относиться к нескольким категориям:

- read-heavy;
- write-heavy;
- CPU-heavy;
- memory-heavy;
- storage-heavy;
- network-heavy;
- connection-heavy(много соединений, например WS);
- latency-sensitive;
- event-driven;
- precomputation-heavy;
- search-heavy;
- geo-heavy;
- consistency-heavy;
- fan-out-heavy(это например проблема celebruty, одна публикация может затронуть очень большое кол-во людей);
- burst-heavy(нагрузка приходит **резкими всплесками**, например, сайт по продаже билетов).

Подробнее: [[System Design Workload Patterns]]

Например:

```text
Social Feed(ленты чего-то, например ВК, инстаграм)

read-heavy
fan-out-heavy
precomputation-heavy
media/network-heavy
```

Классификация должна помочь определить, **какие части архитектуры требуют особого внимания**.

# 5. Data Model

Определить основные сущности системы.

Для каждой сущности подумать:

- какие поля нужны?
- какой примерно размер записи?
- какие связи существуют между сущностями?
- какие основные access patterns?
- по каким полям ищем?
- по каким полям сортируем?
- какие данные изменяются часто?

На этом этапе уже можно приблизительно оценить объём хранения.

---

# 6. API

Определить основные операции системы.

Например:

```text
POST /posts
GET /feed
POST /messages
GET /messages
```

Не нужно проектировать десятки endpoint'ов.

Нужно показать основные flows.

Для каждого взаимодействия подумать:

- REST / RPC?
- WebSocket / SSE?
- sync или async?
- нужна ли pagination?
- нужен ли idempotency key?
- какие операции могут быть долгими?

Подробнее: [[Web]]

---

# 7. High-Level Design

Сначала нарисовать систему максимально просто. Также перед рисованием схемы стоит задать себе вопрос про характер нагрузки. Какой у нас перекос между чтением и записью? Нужно ли нам много делать(какое-то агригирование) для чтение или чтение очень простое. Тоже самое с записью - это просто положить в БД или нужно что-то парсить, ходить в другие ручки и тд

Например:

```text
Client
  ↓
Load Balancer / API Gateway
  ↓
Application
  ↓
Database
```

После этого постепенно добавлять необходимые компоненты.

Двигаться:

```text
high-level
    ↓
components
    ↓
critical flows
    ↓
bottlenecks
    ↓
scaling
```

Не начинать проектирование сразу с Kafka, Redis, sharding и десятка микросервисов.

## Architecture Diagrams (архитектурные схемы)

- Есть ли общепринятые правила изображения компонентов?
- Что обозначают прямоугольники, цилиндры, стрелки и другие фигуры?
- Как обозначать синхронные и асинхронные взаимодействия?
- Что такое UML (Unified Modeling Language)?
- Чем UML Component Diagram отличается от Sequence Diagram?
- Что такое C4 Model (модель четырёх уровней архитектуры)?
- Какие уровни C4 полезны на System Design интервью?
- Когда достаточно обычной архитектурной блок-схемы?

---

# 8. Analyze Each Component

Для каждого важного компонента пройти небольшой checklist.

## Responsibility

- Что делает компонент?
- Почему он вообще нужен?

## Load

Какой характер нагрузки?

- read;
- write;
- CPU;
- memory;
- network;
- storage;
- connections.


## Resource Utilization (утилизация ресурсов)

- Что означает утилизация CPU, RAM, диска и сети?
-  Почему высокая утилизация не всегда означает эффективность?
- Как учитывать запас ресурсов для пиков и отказов?

## State

- Stateless или stateful?
- Какие данные хранит?

## Storage

- SQL или NoSQL?
- Нужен ли object storage?
- Какие access patterns?
- Нужны ли indexes?
- Нужны ли replication / partitioning / sharding?

Подробнее: [[Databases]]

## Cache

- Нужен ли cache?
- Что именно кэшируем?
- Как инвалидируем?
- Что произойдёт при cache miss?
- Что произойдёт при потере cache?

## Redis

- Для каких задач Redis используют помимо кэширования?
- Какие структуры данных поддерживает Redis?
- Что такое Sorted Set (упорядоченное множество)?
- Как использовать Sorted Sets для рейтингов и очередей?
- Как Redis применяют для rate limiting?
- Как реализовать распределённую блокировку через SET NX PX?
- Почему важно безопасно освобождать блокировку?
- Какие гарантии даёт Redis при отказе primary?
- Когда Redis не подходит как источник истины?

Подробнее: [[Caching]]

## Precomputation

- Можно ли подготовить данные заранее?
- Что дешевле: compute-on-read или compute-on-write?

## Communication

- sync или async?
- нужен ли message broker?
- нужны ли события?

Подробнее: [[Message Brokers]]

## Consistency

- Какая consistency нужна этому компоненту?
- Что произойдёт при concurrent updates?

---

# 9. Critical Flows

После high-level схемы пройти несколько основных сценариев от начала до конца.

Например для messenger:

```text
Send Message

Client
 → Gateway
 → Message Service
 → Database
 → Notification / Connection Service
 → Recipient
```

Отдельно:

```text
Recipient Offline
```

или:

```text
Service Failure
```

Для каждого flow спросить:

- где происходит synchronous operation?
- где может возникнуть ошибка?
- что произойдёт при retry?
- можно ли получить duplicate?
- можно ли потерять данные?
- где нужна idempotency?

---

# 10. Bottlenecks

Для каждого важного компонента спросить:

> Что сломается первым при росте нагрузки?

Проверить:

- CPU;
- RAM;
- network;
- disk throughput;
- IOPS;
- DB connections;
- locks/contention;
- external API;
- broker;
- hot partition / hot key.

И главный вопрос:

> Что произойдёт, если нагрузка увеличится в 10 раз?

---

# 11. Scaling

После обнаружения bottleneck решить, как его масштабировать.

Проверить:

- vertical scaling;
- horizontal scaling;
- load balancing;
- caching;
- read replicas;
- partitioning;
- sharding;
- asynchronous processing;
- batching;
- precomputation;
- CDN.

Не добавлять технологию просто потому, что она существует.

Каждое решение должно отвечать на конкретную проблему.

---

# 12. Reliability

Пройтись по основным failure scenarios.

## Single Point of Failure

- Есть ли компонент в единственном экземпляре?
- Что произойдёт при его падении?

## Database

- Что произойдёт при недоступности primary?
- Есть ли replication/failover?
- Можно ли потерять данные?

## Network

- Что произойдёт при timeout?
- Что произойдёт при network partition?

## Retry

- Можно ли безопасно повторить операцию?
- Нужна ли idempotency?
- Нужны ли backoff и jitter?

## Overload

Что произойдёт при перегрузке?

При необходимости:

- rate limiting;
- throttling;
- backpressure;
- load shedding;
- circuit breaker.

## Degradation

Может ли система продолжить частично работать?

- graceful degradation;
- fallback;
- feature flags;
- cached/stale data.

---

# 13. Distributed Data

Если система распределённая, отдельно проверить:

- replication;
- consistency;
- conflict resolution;
- multi-DC;
- partitioning;
- sharding;
- rebalancing.

Если одна бизнес-операция затрагивает несколько сервисов:

- Saga?
- Transactional Outbox?
- Transactional Inbox?
- CDC?
- distributed lock?

## Concurrent Access (конкурентный доступ)

- Что делать, если несколько сервисов изменяют одни данные?
- Когда достаточно транзакции в одной БД?
- Чем pessimistic locking отличается от optimistic locking?
- Как работает SELECT FOR UPDATE?
- Что такое atomic conditional update?
- Когда нужен distributed lock (распределённая блокировка)?
- Какие проблемы возникают при истечении TTL блокировки?
- Что такое fencing token (токен защиты от устаревшего владельца)?
- Когда вместо блокировки стоит использовать Saga?

Подробнее: [[System Architecture]]

---

# 14. Observability

Как поймём, что система работает плохо?

Подумать про:

- metrics;
- logs;
- tracing;
- alerts.

Особенно:

- RPS;
- error rate;
- latency;
- saturation;
- queue lag;
- cache hit ratio;
- DB load.


## Distributed Tracing (распределённая трассировка)
- Что такое trace, span, trace_id и span_id?
- Как передавать trace context между микросервисами?
- Как найти медленный участок распределённого запроса?
- Как связать логи с конкретным trace_id?

## Metrics (метрики)
- Какие четыре сигнала наиболее важны: latency, traffic,
  errors, saturation?
- Что такое p50, p95, p99?
- Почему средняя latency может скрывать проблемы?
- Какие метрики важны для PostgreSQL?
- Какие метрики важны для Kafka и Redis?
- Какие метрики предоставляет Go Runtime?
- Как обнаруживать утечки памяти и горутин?
- Чем metrics отличаются от logs и traces?

## Практика
- Как искать ошибки и запросы в Kibana?
- Как анализировать метрики в Grafana?
- Как построить dashboard для микросервиса?
- Как настроить alerting (оповещения)?


Подробнее: [[Observability]]

---

# 15. Final Review

Перед завершением пройти короткий список:

- Выполнены ли functional requirements?
- Выполнены ли non-functional requirements?
- Где главный bottleneck?
- Есть ли SPOF?
- Что произойдёт при падении БД?
- Что произойдёт при падении одного instance?
- Что произойдёт при network partition?
- Что произойдёт при повторном запросе?
- Что произойдёт при росте нагрузки ×10?
- Какие данные можно потерять?
- Где нужна strong consistency?
- Где допустима eventual consistency?
- Какие основные trade-offs мы сделали?

---

# Interview Flow

В итоге решение задачи выглядит примерно так:

```text
1. Functional Requirements (функциональные требования, т.е. какими функциями должна обладать система)
          ↓
2. Non-functional Requirements (нефункциональные требования, сколько DAU, какая частотность действий пользователя, есть ли лимиты, есть ли сезонность, сколько храним данные, гео распределенность)
          ↓
3. Data Model (модель данных, прикидываем структуру основных таблиц и вес данных. Это нам нужно для расчета сети, дисков и тд)
		   ↓
4. Capacity Estimation (оценка нагрузки и ресурсов, определение характера нагрузки.
   кол-во хостов лучше посчитать на этапе масштабирования. Сначала прикидываем основные компоненты, а их кол-во можно уточнить позже. А RPS мы считаем сразу, т.к. это поможет нам прикинуть архитектуру приложения - микросервисы или монолит)
          ↓
5. API (проектирование API)
          ↓
6. High-Level Design (высокоуровневая архитектура)
          ↓
7. Component Deep Dive (детальный разбор компонентов)
          ↓
8. Bottlenecks (поиск узких мест)
          ↓
9. Scaling (масштабирование)
          ↓
10. Reliability (надёжность и отказоустойчивость)
          ↓
11. Trade-offs (компромиссы принятых решений)
```

Главный принцип:

> Не начинать с технологий.

Сначала определить требования и характер нагрузки, затем найти потенциальные проблемы и только после этого выбирать архитектурные решения и технологии.