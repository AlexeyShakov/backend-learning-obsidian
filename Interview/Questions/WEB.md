
## HTTP

### Fundamentals

- Что такое HTTP?
- Как устроен HTTP request?
- Как устроен HTTP response?
- Какие основные HTTP methods существуют?
- В чём семантическая разница между `GET`, `POST`, `PUT`, `PATCH` и `DELETE`?
- Какие основные группы HTTP status codes существуют?
- Какие HTTP headers часто используются?
- Что такое request body?
- Что такое query parameters?
- Чем query parameters отличаются от path parameters?
- Что такое Content-Type?
- Что такое User-Agent?


### HTTP Properties

- Что означает, что HTTP stateless?
- Что такое идемпотентность HTTP method?
- Какие HTTP methods являются идемпотентными?
- Что такое safe HTTP method?
- Чем safe method отличается от idempotent method?

### HTTP Connections

- Как HTTP использует TCP?
- Создаётся ли новое TCP-соединение для каждого HTTP request?
- Что такое HTTP Keep-Alive?
- Какие преимущества даёт переиспользование соединений?
- Что такое connection timeout?
- Какие виды timeout обычно используются HTTP-клиентом?

## HTTPS and TLS

- Чем HTTP отличается от HTTPS?
- Что такое TLS?
- Как устанавливается защищённое соединение?

## REST

- Что такое REST?
- Какие основные принципы REST?
- Что такое resource в REST?
- Как resources обычно отображаются на HTTP endpoints?
- Что означает statelessness в REST?
- Что такое RESTful API?
- Чем REST отличается от просто HTTP API?
- Как версионировать REST API?

## WebSocket

- Что такое WebSocket? Какой принципе работы?
- Для каких задач используется WebSocket?
- Какие проблемы возникают при масштабировании WebSocket-соединений?
- Что происходит с WebSocket connection при перезапуске экземпляра приложения?
- Как работать с WebSocket при нескольких replicas приложения?
- Плюсы и минусы

## Server-Sent Events

- Что такое Server-Sent Events (SSE)? Какой принцип работы
- Как клиент восстанавливает SSE connection после разрыва?
- Какие проблемы возникают при использовании большого количества долгоживущих SSE connections?
- Плюсы и минусы

## Long Polling

- Что такое Long Polling? Принципе работы? И отличие от pollig
- Как работает Long Polling?
- Какие минусы  и плюсы по сравнению с SSE и WS?

## RPC

### Fundamentals

- Что такое RPC?
- Как работает Remote Procedure Call?
- Чем RPC отличается от REST?
- Какие преимущества и недостатки RPC?
- Для каких задач подходит RPC?
### gRPC

- Что такое gRPC? 
- Какой принципе работы?
- Какие основные компоненты?
- Какие преимущества gRPC имеет для взаимодействия между микросервисами?
- Какие недостатки есть у gRPC по сравнению с REST?


## Web Security
* Что такое CORS?
* Что такое CSRF?
* Что такое SQL injection?

## Authentication and Authorization

### Fundamentals

- Что такое authentication?
- Что такое authorization?
- В чём разница между authentication и authorization?
- В каком порядке обычно происходят authentication и authorization?
- Что такое Session-Based Authentication?
- Что такое Token-Based Authentication?
- Что такое JWT-токены?
* Что такое OAuth 2.0?
* Что такое Keycloak?
* Что такое OpenIDConnect?
