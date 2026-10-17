
## Core
### Strings 
- Как строка устроена под капотом? 
- Почему строки в Go immutable?
- Что такое руна?
- Как связаны строки и байты?
### Arrays and Slices
- Чем массив отличается от slice? 
- Как slice устроен под капотом? 
- Что такое `len` и `cap`? 
- Что происходит при создании среза из существующего slice? Создаётся ли новый slice?
### Maps 
- Как `map` устроена в памяти? 
- Какие типы могут быть ключами `map`? 
- Почему `slice` не может быть ключом `map`? 
- Что такое эвакуация данных в `map`?
### Interfaces 
- Что такое интерфейс в Go?
- Как работают интерфейсы?

### Errors
* IsError
* AsError
### Embedding 
- Как в Go реализуется композиция вместо классического наследования? 
- Что такое embedded struct?
### Initialization 
- Что делает функция `init()`? 
- Когда вызывается `init()`? 
- В каком порядке выполняются `init()` разных пакетов?
### Другие вопросы
- рефлексия
- Какие способы выполнения background jobs существуют в Go?
- Какие решения используются в Go для durable background jobs и чем они отличаются от Celery?
- Когда использовать task queue, message broker или workflow engine вроде Temporal?

## Concurrency 
### Goroutines and Scheduler 
- Чем goroutine отличается от OS thread? 
- Какие состояния могут быть у goroutine? 
- Как работает Go scheduler и GMP-модель? 
- Есть ли для Go scheduler разница между `http.Get()` и чтением файла? В чём она?
### Channels 
- Что такое channel в Go? 
- В чём особенности работы channels?
### Synchronization 
- Какие примитивы синхронизации есть в Go? 
- Как работает `Mutex`? 
- Как работает `RWMutex`? 
- Как `RWMutex` устроен под капотом? 
- Зачем `RWMutex` нужны `Lock/RLock` и `Unlock/RUnlock`? 
- Почему операции с `RWMutex` могут быть дороже операций с обычным `Mutex`? 
- Что такое atomic operations и для чего они нужны? ЧТо такое атомики? Нужны примеры использования
- WaitGroup и errgroup.Group
### Race Conditions and Deadlocks 
- Что такое race condition? 
- Какие есть способы борьбы с race conditions? 
- Что такое deadlock? 
- Какие есть способы предотвращения и борьбы с deadlock?
## Memory Management 
### Stack and Heap 
- что вообще такое stack и heap? Зачем они нужны?
- Что делает функция `new()`?
- Какой тип имеет переменная `var i = new(int)`? 
- Где будет аллоцирована переменная — на stack или heap?
- Как работает выделение памяти в golang, когда вызываются новые аллокации памяти?
### Garbage Collector 
- Как работает Garbage Collector в Go? В какие моменты начинает работать?
- Что такое Stop-the-World? 
- Работает ли Garbage Collector со stack? 
- Может ли в Go возникнуть memory leak?
- Какие данные Garbage Collector не сможет освободить?
- Что такое write barier?
## Остальное
## Context 
- Что такое `context.Context` и для чего он нужен? 
- Как работает отмена контекста? 
- Можно ли хранить данные в `context`?
- Когда данные стоит и не стоит передавать через `context`?
## Patterns 
- Что такое fan-in и fan-out? 
- Как реализовать fan-in/fan-out в Go? 
## Tooling and Ecosystem
- Какие инструменты и библиотеки для логирования используются в Go?
- какие инструменты используются для профилирования и обнаружения deadlocks?


!!!!!!!!! НУЖНО РАЗОБРАТЬ НА ПРАКТИКЕ
* контексты. В каких случаях могут применяться
* fan-in и fan-out