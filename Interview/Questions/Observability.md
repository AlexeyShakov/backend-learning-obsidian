
## Fundamentals

- Что такое observability и чем она отличается от monitoring?
- Какие основные составляющие observability существуют и как logs, metrics и traces дополняют друг друга?


## Logging

### Fundamentals

- Зачем нужны логи и какие уровни логирования существуют?
- Что такое structured logging и зачем оно нужно?
- Что стоит и что нельзя писать в логи?
- Что такое correlation/request ID и как с его помощью отслеживать запрос между несколькими сервисами?

### Centralized Logging

- Где и как должны храниться логи приложения?
- Куда приложение должно писать логи в контейнерной среде?
- Кто и как собирает логи контейнеров в Kubernetes?
- Как выглядит полный путь лога от приложения до централизованного хранилища и системы визуализации?
- Какие инструменты используются для сбора, хранения и просмотра логов?


## Metrics

### Fundamentals

- Что такое metrics и чем они отличаются от logs?
- Какие типы метрик существуют: Counter, Gauge, Histogram, Summary?
- Какие метрики стоит собирать с backend-приложения?
- Что такое latency, throughput и error rate?
- Что такое percentiles (`p50`, `p95`, `p99`) и почему average latency может быть недостаточно?

### Prometheus and Grafana

- Что такое Prometheus и как он собирает метрики?
- Что такое labels в Prometheus и почему high cardinality является проблемой?
- Что такое Grafana и чем она отличается от Prometheus?


## Distributed Tracing

- Что такое distributed tracing и зачем он нужен?
- Что такое trace и span?
- Как trace context передаётся между сервисами?
- Как tracing работает при HTTP, gRPC и взаимодействии через message broker?
- Как связать traces с logs?
- Что такое OpenTelemetry и какие задачи он решает?


## Alerting

- Что такое alerting и на какие события/метрики стоит создавать alerts?
- Что такое alert fatigue и почему нельзя создавать alert на каждую ошибку?
- Как определить по метрикам, что сервис деградировал, даже если продолжает отвечать?


## Health Checks

- Что такое health check и что должен проверять health endpoint?
- Чем liveness и readiness отличаются друг от друга?
- Должен ли health check зависеть от состояния БД и внешних сервисов?


## SLI, SLO and SLA

- Что такое SLI, SLO и SLA и чем они отличаются?
- Что такое availability и как её измерять?
- Что такое error budget?


## Incident Investigation

- API endpoint стал отвечать медленно. Как искать причину?
- В production выросло количество `5xx`. Как искать причину?
- Как понять, где находится проблема: в приложении, БД, кэше или внешнем сервисе?
- Как расследовать проблему в запросе, проходящем через несколько микросервисов?
- Как понять, что проблема связана с connection pool, Kafka consumer lag или конкретной replica приложения?
## Observability Stack

- Какие инструменты обычно используются для logs, metrics, traces и visualization?
- Что такое ELK Stack и какую роль выполняют Elasticsearch, Logstash и Kibana?
- Чем ELK отличается от EFK и зачем вместо Logstash может использоваться Fluent Bit / Fluentd?
- Что такое Loki и чем подход Loki к хранению логов отличается от Elasticsearch?
- Как связаны Prometheus и Grafana и какую задачу решает каждый из них?
- Что такое OpenTelemetry и как он связан с logs, metrics и traces?
- Какие системы используются для distributed tracing: Jaeger, Tempo и другие?
- Как может выглядеть типичный observability stack в Kubernetes?
- Для чего нужен Sentry?