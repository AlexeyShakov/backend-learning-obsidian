# Infrastructure

## Virtualization

- Что такое виртуализация?
- Что такое Virtual Machine?
- Как устроена виртуальная машина?
- Что такое hypervisor?
- Какие ресурсы виртуализируются?
- Какие преимущества и недостатки есть у виртуальных машин?


## Containers

### Fundamentals

- Что такое контейнер?
- Чем контейнер отличается от виртуальной машины?
- Что такое containerization?
- Почему контейнеры обычно легче виртуальных машин?
- Используют ли контейнеры ядро host OS?
- Что обеспечивает изоляцию контейнеров?
- Что такое Linux namespaces?
- Что такое cgroups?
- Чем namespaces отличаются от cgroups?


### Containerization vs Virtualization

- В чём разница между containerization и virtualization?
- Когда лучше использовать контейнер, а когда Virtual Machine?
- Почему контейнер запускается быстрее Virtual Machine?
- Насколько контейнеры изолированы друг от друга?
- Может ли контейнер использовать другое ядро ОС?


## Docker

### Fundamentals

- Что такое Docker?
- Как Docker связан с Linux containers?
- Чем Docker image отличается от container?
- Что такое Dockerfile?
- Что такое Docker registry?
- Что происходит при выполнении `docker run`?


### Images

- Как устроен Docker image?
- Что такое layers в Docker image?
- Почему Docker images состоят из layers?
- Что такое Copy-on-Write в Docker?
- Чем `COPY` отличается от `ADD`?
- Что такое multi-stage build?
- Зачем уменьшать размер Docker image?


### Networking

- Как работает сеть в Docker?
- Как контейнер получает IP-адрес?
- Как контейнеры взаимодействуют друг с другом?
- Что такое port mapping?
- В чём разница между `EXPOSE` и публикацией порта?


### Storage

- Что происходит с данными внутри контейнера после его удаления?
- Что такое Docker volume?
- Зачем нужны volumes?
- Чем volume отличается от bind mount?


## Kubernetes

### Fundamentals

- Что такое Kubernetes и какие задачи он решает?
- Что такое cluster?
- Что такое node?
- Что такое Pod?
- Почему Pod, а не container, является минимальной deployable unit в Kubernetes?
- Может ли Pod содержать несколько containers?
- В каких случаях несколько containers размещают в одном Pod?


### Deployment

- Что такое Deployment?
- Зачем нужен Deployment?
- Что такое ReplicaSet?
- Как связаны Deployment, ReplicaSet и Pod?
- Что произойдёт, если Pod упадёт?
- Что произойдёт, если Node с Pod станет недоступна?
- Как Kubernetes поддерживает необходимое количество replicas?
- Что происходит при `kubectl rollout restart deployment`?

### Service

- Что такое Kubernetes Service?
- Зачем нужен Service, если у Pod уже есть IP?
- Почему нельзя обращаться к Pod напрямую по его IP?
- Какие типы Service существуют?
- Чем `ClusterIP`, `NodePort` и `LoadBalancer` отличаются друг от друга?
- Как Service находит нужные Pods?
- Что такое labels и selectors?


### Configuration

- Что такое ConfigMap?
- Что такое Secret?
- Чем ConfigMap отличается от Secret?
- Как передать ConfigMap или Secret в Pod?
- Что происходит с приложением после изменения Secret?
- Всегда ли Pod автоматически получает новое значение Secret?


### Health Checks

- Что такое liveness probe?
- Что такое readiness probe?
- Что такое startup probe?
- Чем liveness probe отличается от readiness probe?
- Что произойдёт, если liveness probe не проходит?
- Что произойдёт, если readiness probe не проходит?
- Почему неправильно использовать одну и ту же логику для liveness и readiness?


### Resources

- Что такое CPU/memory requests?
- Что такое CPU/memory limits?
- Чем requests отличаются от limits?
- Как requests используются Kubernetes scheduler?
- Что произойдёт, если container превысит memory limit?
- Что такое `OOMKilled`?
- Что происходит при превышении CPU limit?


### Scaling

- Как масштабировать приложение в Kubernetes?
- Что такое horizontal scaling?
- Что такое vertical scaling?
- Что такое Horizontal Pod Autoscaler (HPA)?
- На основании каких метрик можно масштабировать приложение?
- Какие проблемы могут возникнуть при запуске нескольких replicas приложения?

### Graceful Shutdown

- Что происходит с Pod при его удалении?
- Что такое `SIGTERM` в контексте Kubernetes?
- Что такое `terminationGracePeriodSeconds`?
- Как правильно реализовать graceful shutdown приложения?
- Что происходит с выполняющимися HTTP requests при завершении Pod?
- Что произойдёт с выполняющейся background job при завершении Pod?
- Почему нельзя рассчитывать на локальную память Pod для хранения состояния задачи?
### Deployment Strategies

- Что такое Rolling Update?
- Как работает Rolling Update в Kubernetes?
- Как Kubernetes постепенно заменяет старые Pods новыми?
- Что такое `maxSurge` и `maxUnavailable`?
- Как реализовать deployment приложения без downtime?
- Как readiness probe помогает обеспечить zero-downtime deployment?
- Когда новый Pod начинает получать traffic?
- Когда старый Pod перестаёт получать traffic при Rolling Update?
- Что происходит с уже выполняющимися запросами при завершении старого Pod?
- Как graceful shutdown связан с zero-downtime deployment?
- Зачем нужен `terminationGracePeriodSeconds`?
- Может ли Rolling Update привести к downtime? В каких случаях?
- Что произойдёт, если новая версия приложения не проходит readiness probe?
- Как откатить неудачный deployment?
- Что такое Rolling Update, Blue-Green Deployment и Canary Deployment?
- Чем Rolling Update отличается от Blue-Green Deployment?
- Чем Rolling Update отличается от Canary Deployment?
## Load Balancing

- Что такое Load Balancer?
- Зачем нужен Load Balancer?
- Какие существуют алгоритмы балансировки нагрузки?
- Что такое Round Robin?
- Что такое Least Connections?
- Что такое sticky sessions?
- Зачем могут понадобиться sticky sessions?
- Какие проблемы возникают при использовании sticky sessions?
- Что такое L4 и L7 Load Balancing?
- В чём разница между L4 и L7 Load Balancer?


## Reverse Proxy

- Что такое reverse proxy?
- Чем reverse proxy отличается от forward proxy?
- Для чего используется Nginx перед backend-приложением?
- Чем reverse proxy отличается от Load Balancer?
- Может ли один компонент одновременно быть reverse proxy и Load Balancer?