
Этот документ помогает определить **характер нагрузки системы** и понять, на какие части архитектуры стоит обратить особое внимание.

Система может одновременно относиться к нескольким категориям.

Например:

```text
WhatsApp

connection-heavy
write-heavy
latency-sensitive
fan-out-heavy
```

```text
Google Drive

storage-heavy
network-heavy
durability-heavy
```

Классификация — не готовое архитектурное решение.

Она помогает перейти от:

> Что это за система?

к:

> Где здесь потенциальные bottleneck и какие архитектурные решения стоит рассмотреть?

---

# 1. Read-heavy

## Что это

Система, в которой операций чтения значительно больше, чем операций записи.

```text
read RPS >> write RPS
```

## Примеры

- социальная лента(лента ВК, Твиттер);
- новостной сайт;
- каталог товаров маркетплейса;
- просмотр видео;
- Wikipedia;
- поиск отелей(букинг).

## Основная проблема

Большое количество запросов на чтение создаёт нагрузку на хранилище.

## На что обратить внимание

- read RPS;
- latency чтения;
- размер response;
- сложность запросов;
- повторяемость запросов;
- допустимость stale(устаревших) data.

## Возможные решения

- caching;
- read replicas;
- CDN;
- индексы;
- denormalization;
- precomputation;
- отдельная read model;
- pagination.
- data cooling (охлаждение редко используемых исторических данных) — если объём истории большой, а большинство запросов работает с актуальными данными;

Главный вопрос:

> Можно ли сделать запись немного дороже, чтобы существенно удешевить чтение?

---

# 2. Write-heavy

## Что это

Система получает очень большое количество операций записи.

```text
write RPS >> read RPS
```

## Примеры

- сбор логов;
- metrics;
- IoT telemetry;
- clickstream;
- tracking координат автомобилей;
- ingestion событий.

## Основная проблема

Хранилище должно успевать принимать большой поток данных.

## На что обратить внимание

- write RPS;
- размер записи;
- IOPS;
- disk throughput;
- write amplification;
- durability;
- partitioning;
- скорость consumers.

## Возможные решения

- batching (пакетная обработка, уменьшаем кол-во запросов);
- asynchronous writes (асинхронная запись);
- buffering (буферизация, полезно при кратковременных всплесках);
- backpressure (обратное давление);
- partitioning (партиционирование);
- sharding (шардирование);
- append-oriented storage (хранение с упором на добавление данных);
- sequential writes (последовательная запись).

Важно не делать write path без необходимости слишком дорогим.

---

# 3. CPU-heavy

## Что это

Основным ограниченным ресурсом является CPU.

## Примеры

- LeetCode / Codeforces;
- video transcoding;
- image processing;
- compression;
- encryption;
- ML inference;
- rendering.

## Основная проблема

Одна операция требует значительного количества вычислений.

## На что обратить внимание

- CPU time одной задачи;
- количество CPU cores;
- concurrency;
- время выполнения;
- возможность параллельной обработки;
- ограничения ресурсов.

## Возможные решения

- worker pool;
- task queue;
- asynchronous processing;
- horizontal scaling workers;
- autoscaling;
- batching;
- resource limits;
- priority queues.

Типичная схема:

```text
API
 ↓
Queue
 ↓
Worker Pool
```

API и computational workers часто имеет смысл масштабировать независимо.

---

# 4. Memory-heavy

## Что это

Для эффективной работы системе необходимо держать большой объём данных в RAM.

## Примеры

- distributed cache;
- session storage;
- Redis;
- leaderboard;
- некоторые real-time aggregation systems.

## Основная проблема

RAM ограничена и значительно дороже дискового хранения.

## На что обратить внимание

- размер working set;
- количество объектов;
- размер объекта;
- replication;
- memory overhead;
- TTL;
- persistence.

Приблизительная оценка:

```text
Memory ≈ number_of_objects × average_object_size
```

## Возможные решения

- eviction policies (политики вытеснения данных из памяти);
- TTL (время жизни данных);
- partitioning (разделение данных на части);
- compression (сжатие данных);
- sharding (распределение данных между несколькими узлами);
- ограничение working set (активно используемого набора данных).

---

# 5. Storage-heavy

## Что это

Система должна хранить очень большой объём данных.

Есть как минимум два разных случая.

## Large objects

### Примеры

- Google Drive;
- Dropbox;
- Google Photos;
- YouTube;
- backup storage.

### На что обратить внимание


- storage capacity (необходимый объём хранилища);
- network throughput (пропускная способность сети);
- durability (гарантия сохранности данных);
- replication (создание копий данных на нескольких узлах);
- размер файлов;
- multi-DC (хранение и работа системы в нескольких дата-центрах).
### Возможные решения

- object storage (объектное хранилище, например S3);
- chunking (разбиение больших файлов на части);
- replication (хранение нескольких копий данных);
- erasure coding (избыточное кодирование для восстановления данных при потере части блоков);
- compression (сжатие данных);
- deduplication (устранение дубликатов данных);
- Content Addressable Storage / CAS (хранение с адресацией данных по хэшу их содержимого);
- cold storage (дешёвое хранилище для редко используемых данных).

---

## Большое количество небольших объектов

### Примеры

- messages;
- logs;
- events;
- metrics.

### На что обратить внимание

- IOPS (количество операций ввода-вывода в секунду);
- write throughput (пропускная способность системы на запись);
- indexes (индексы и их влияние на стоимость записи);
- retention (срок хранения данных);
- partitioning (разделение данных на части).
### Возможные решения

- batching (объединение множества операций записи в пакеты);
- partitioning (разделение данных на части);
- compression (сжатие данных);
- retention policies (правила удаления или переноса устаревших данных).

Важно:

> Хранение 10 TB больших файлов и 10 TB маленьких событий — разные архитектурные задачи.

---

# 6. Network-heavy

## Что это

Основным ограничением становится объём передаваемых по сети данных.

## Примеры

- YouTube;
- Netflix;
- Google Drive;
- video conferencing;
- photo hosting;
- backup systems.

## Основная проблема

Большой ingress/egress traffic.

```text
Ingress — входящий трафик
Egress  — исходящий трафик
```

## На что обратить внимание

- network (пропускная способность сети);
- размер payload (объём полезных данных, передаваемых в запросе или ответе);
- concurrent downloads/streams (количество одновременных загрузок или потоков);
- geography (географическое расположение пользователей и серверов);
- latency (задержка передачи данных);
- ingress/egress ratio (соотношение входящего и исходящего сетевого трафика).

## Возможные решения

- CDN (сеть серверов для доставки контента пользователю из ближайшей точки);
- compression (сжатие передаваемых данных);
- caching (кэширование часто запрашиваемых данных);
- edge locations (серверы, расположенные географически ближе к пользователям);
- chunking (разбиение больших данных или файлов на части);
- regional storage (хранение данных ближе к пользователям в соответствующих регионах);
- parallel download (параллельное скачивание нескольких частей данных);
- adaptive bitrate (динамическое изменение качества потока в зависимости от скорости соединения).

---

# 7. Connection-heavy

## Что это

Система должна поддерживать очень большое количество одновременно открытых долгоживущих соединений.

Количество connections здесь может быть важнее RPS.

## Примеры

- WhatsApp;
- Telegram;
- Slack;
- Discord;
- multiplayer games;
- trading feeds.

## Основная проблема

Например:

```text
5 000 000 concurrent WebSocket connections
```

Даже если каждый пользователь отправляет сообщения редко, соединение необходимо постоянно поддерживать.

## На что обратить внимание

- concurrent connections;
- memory per connection;
- file descriptors;
- connection distribution;
- reconnect;
- heartbeat;
- messages/sec.

## Возможные решения

- WebSocket/SSE gateways;
- horizontal scaling;
- connection routing;
- pub/sub;
- heartbeat;
- efficient event loop;
- backpressure.

---

# 8. Latency-sensitive

## Что это

Система должна отвечать с очень маленькой задержкой.

## Примеры

- search autocomplete;
- online games;
- ad serving;
- recommendation serving;
- trading systems;
- real-time matching.

## Основная проблема

Даже если средняя latency хорошая, часть запросов может выполняться слишком долго.

Поэтому важны:

```text
p50
p95
p99
```

## На что обратить внимание

- network hops;
- database latency;
- external calls;
- tail latency;
- geography;
- synchronous dependencies.

## Возможные решения

- caching;
- precomputation;
- locality;
- parallel requests;
- timeout budgets;
- CDN/edge;
- уменьшение synchronous request path.

Главный вопрос:

> Что можно вычислить до того, как придёт пользовательский запрос?

---

# 9. Event-driven

## Что это

Изменения в системе естественно представляются как события, на которые могут реагировать несколько независимых компонентов.

## Примеры

- Taxi;
- Delivery;
- order processing;
- payment workflows;
- notifications;
- logistics.

Например:

```text
OrderCreated
      ↓
 ┌────┼─────────┐
 ↓    ↓         ↓
Payment
     Delivery
              Notification
```

## На что обратить внимание

- ordering;
- delivery guarantees;
- duplicates;
- retries;
- idempotency;
- event schema;
- consumer lag.

## Возможные решения

- message broker;
- consumer groups;
- idempotent consumers;
- DLQ;
- Transactional Outbox.

---

# 10. Precomputation-heavy

## Что это

Часть данных выгоднее вычислить заранее, чем строить их заново при каждом пользовательском запросе.

## Примеры

- social feed;
- recommendations;
- leaderboard;
- analytics dashboards;
- ranking;
- search suggestions.

## Основная проблема

Вычисление результата непосредственно во время запроса слишком дорогое.

Вместо:

```text
Request
   ↓
Expensive calculation
   ↓
Response
```

можно:

```text
Events
   ↓
Background processing
   ↓
Prepared data
   ↓
Request → fast read
```

## Возможные решения

- background workers;
- materialized views;
- denormalized read models;
- cache;
- CQRS;
- event-driven processing.

Главный trade-off:

```text
cheaper reads
     ↕
more expensive writes
```

---

# 11. Search-heavy

## Что это

Основной сценарий — поиск и фильтрация данных по множеству параметров, а не получение сущности по известному ID.

## Примеры

- Booking;
- marketplace;
- поиск товаров;
- поиск документов;
- поиск пользователей.

Пример:

```text
Отели:
город = Москва
цена < 10 000
rating > 8
есть парковка
```

## На что обратить внимание

- количество searchable fields;
- full-text search;
- filtering;
- sorting;
- ranking;
- freshness search index.

## Возможные решения

- indexes;
- inverted indexes;
- Elasticsearch/OpenSearch;
- отдельная search read model;
- denormalization;
- CDC.

Частый вариант:

```text
Primary Database
       ↓
      CDC
       ↓
Search Index
```

---

# 12. Geo-heavy

## Что это

Система активно работает с координатами и пространственными запросами.

## Примеры

- Taxi;
- Delivery;
- Maps;
- поиск ближайших магазинов;
- поиск курьеров.

## Основная проблема

Например:

> Найти свободных водителей в радиусе 2 км.

Обычный поиск по всей таблице становится слишком дорогим.

## На что обратить внимание

- частота обновления координат;
- количество объектов;
- geographic distribution;
- radius queries;
- latency.

## Возможные решения

- spatial indexes;
- географическое partitioning;
- Geohash/H3-подобное разбиение пространства;
- caching;
- regional processing.

---

# 13. Consistency-heavy

## Что это

Ошибочное или конфликтующее состояние данных недопустимо или очень дорого для бизнеса.

## Примеры

- banking;
- payments;
- Booking;
- ticket sales;
- inventory;
- wallets.

## Типичные проблемы

- double booking;
- double payment;
- lost update;
- duplicate request;
- race condition.

## На что обратить внимание

- transactions;
- isolation levels;
- concurrent updates;
- idempotency;
- consistency;
- auditability.

## Возможные решения

- database transactions;
- constraints;
- optimistic locking;
- pessimistic locking;
- atomic updates;
- idempotency keys;
- Saga;
- Transactional Outbox.

---

# 14. Burst-heavy

## Что это

Нагрузка распределена во времени неравномерно и периодически возникают очень большие всплески.

Средний RPS может выглядеть небольшим:

```text
Average RPS = 2 000
```

но во время события:

```text
Peak RPS = 100 000
```

## Примеры

- старт продажи билетов;
- Black Friday;
- flash sale;
- регистрация на мероприятие;
- публикация результатов экзамена.

## Основная проблема

Система должна пережить кратковременную нагрузку, значительно превышающую обычную.

## На что обратить внимание

- peak RPS;
- размер и продолжительность burst;
- queue growth;
- downstream capacity;
- autoscaling speed.

## Возможные решения

- queues;
- buffering;
- rate limiting;
- backpressure;
- load shedding;
- autoscaling;
- caching;
- admission control(контроль доступа).

---

# Быстрая карта

| Pattern              | Примеры                    | В первую очередь думаем про          |
| -------------------- | -------------------------- | ------------------------------------ |
| Read-heavy           | Feed, каталог, news        | Cache, replicas, precomputation      |
| Write-heavy          | Logs, metrics, IoT         | Batching, broker, partitioning       |
| CPU-heavy            | LeetCode, transcoding      | Queue, workers, CPU                  |
| Memory-heavy         | Cache, sessions            | RAM, TTL, eviction                   |
| Storage-heavy        | Drive, Photos              | Capacity, object storage, durability |
| Network-heavy        | YouTube, Drive             | Bandwidth, CDN, compression          |
| Connection-heavy     | WhatsApp, Discord          | Connections, WS, routing             |
| Latency-sensitive    | Autocomplete, games        | Cache, locality, p99                 |
| Event-driven         | Taxi, Delivery             | Events, broker, idempotency          |
| Precomputation-heavy | Feed, recommendations      | Background processing, read model    |
| Search-heavy         | Booking, marketplace       | Indexes, search engine               |
| Geo-heavy            | Taxi, Delivery             | Spatial index, geo partitioning      |
| Consistency-heavy    | Bank, Booking              | Transactions, locks, idempotency     |
| Burst-heavy          | Ticket sales, flash sale   | Peak RPS, queue, rate limiting       |

---

# Как использовать на System Design интервью

После сбора требований определить:

> К каким workload patterns относится система?

Например:

## Social Feed

```text
read-heavy
fan-out-heavy
precomputation-heavy
network-heavy (media)
проблема селебретис
```

Особое внимание:

```text
cache
precomputed feeds
fan-out strategy
celebrity problem
CDN
```

## Messenger

```text
connection-heavy
latency-sensitive
write-heavy
fan-out-heavy (group chats)
```

Особое внимание:

```text
WebSocket connections
message delivery
ordering
offline users
group fan-out
```

## Taxi

```text
geo-heavy
write-heavy
event-driven
latency-sensitive
```

Особое внимание:

```text
location updates
geo search
matching
event processing
```

## Google Drive

```text
storage-heavy
network-heavy
durability-heavy
```

Особое внимание:

```text
object storage
chunking
bandwidth
replication
deduplication
```

## LeetCode

```text
CPU-heavy
asynchronous workload
burst-heavy
```

Особое внимание:

```text
task queue
worker pool
resource limits
isolation
autoscaling
```

## Booking

```text
read-heavy
search-heavy
consistency-heavy
burst-heavy
```

Особое внимание:

```text
search
availability/inventory
concurrent booking
transactions
idempotency
```

---

# Главное правило

Workload Pattern не определяет конкретную технологию.

Неправильно:

```text
read-heavy → Redis
event-driven → Kafka
write-heavy → MongoDB
```

Правильно:

```text
Requirements
      ↓
Workload Pattern
      ↓
Potential Bottlenecks
      ↓
Architectural Approach
      ↓
Technology Choice
```

Например:

```text
read-heavy
    ↓
Database становится bottleneck
    ↓
Нужно уменьшить количество обращений к БД
    ↓
Рассматриваем caching / replicas / precomputation
    ↓
После этого выбираем конкретную технологию
```