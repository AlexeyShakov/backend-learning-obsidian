# Operating Systems

## Processes
- Что такое process?
- Что находится внутри process?
- Какую память имеет process?
- Изолирована ли память разных processes?
- Что такое PID?
- Какие состояния может иметь process?
- Что происходит при создании process?
- Что происходит при завершении process?
- Что такое fork?
## Threads
- Что такое thread?
- Чем thread отличается от process?
- Что является общим для threads одного process?
- Что у каждого thread своё?
- Могут ли threads одного process обращаться к общей памяти?
- Какие проблемы возникают при работе нескольких threads с общей памятью?
- Что дешевле создать: process или thread? Почему?
## Process vs Thread

- В чём разница между process и thread?
- В каких случаях используются несколько processes, а в каких несколько threads?
- Почему переключение между threads обычно дешевле переключения между processes?
- Может ли один process использовать несколько CPU cores?
## Context Switching
- Что такое context switch?
- Когда происходит context switch?
- Что должна сохранить ОС при переключении между threads/processes?
- Почему context switching имеет стоимость?
- Чем переключение goroutines отличается от переключения OS threads?
## CPU Scheduling

- Что такое CPU scheduler?
- Зачем операционной системе scheduler?
- Что происходит, когда runnable processes/threads больше, чем CPU cores?
- Что такое time slice?
- Чем concurrency отличается от parallelism?
- Может ли concurrent программа выполняться на одном CPU core?
## Memory

### Virtual Memory

- Что такое virtual memory?
- Зачем нужна virtual memory?
- Чем virtual memory отличается от physical memory?
- Что такое virtual address?
- Что такое page?
- Что такое page table?
- Что такое page fault?
### Stack and Heap

- Что такое stack?
- Что такое heap?
- Чем stack отличается от heap?
- Что обычно хранится на stack?
- Что обычно хранится в heap?
- Почему выделение памяти на stack обычно дешевле?
- Что происходит со stack при вызове функции?
## Load Average

- Что такое Load Average в Linux?
- Что означают три числа в `load average`?
- Почему Load Average нужно интерпретировать с учётом количества CPU cores?
- Означает ли высокий Load Average обязательно высокую загрузку CPU?
- Может ли Load Average быть высоким при низком CPU usage?
- Как I/O влияет на Load Average?
## I/O

- Что такое I/O?
- Чем CPU-bound задача отличается от I/O-bound задачи?
- Что такое blocking I/O?
- Что такое non-blocking I/O?
- Что такое synchronous и asynchronous I/O?
- В чём разница между blocking и synchronous I/O?
- Как процесс ожидает завершения I/O операции?
- Что такое `select`? 
- Что такое `poll`?
- Что такое `epoll`?
### I/O Multiplexing 
- Что такое I/O multiplexing? 
- Как обслуживать большое количество network connections без отдельного thread на каждое соединение? 
- Что такое `select`? 
- Что такое `poll`? 
- Что такое `epoll`?
- В чём разница между `select`, `poll` и `epoll`? 
- Почему `epoll` эффективнее при работе с большим количеством connections? 
- Что такое `epoll_create`, `epoll_ctl` и `epoll_wait`? 
- Что происходит, когда thread вызывает `epoll_wait()`? 
- Чем level-triggered режим отличается от edge-triggered?
## File Descriptors

- Что такое file descriptor?
- Для чего используются file descriptors?
- Является ли network socket file descriptor?
- Что такое `stdin`, `stdout` и `stderr`?
- Что произойдёт, если приложение исчерпает доступные file descriptors?
## Signals

- Что такое signal в Linux?
- Для чего используются signals?
- В чём разница между `SIGTERM` и `SIGKILL`?
- Может ли приложение обработать `SIGTERM`?
- Может ли приложение обработать `SIGKILL`?
- Как signals связаны с graceful shutdown приложения?
## Другое
* Что такое syscalls?