**Симплексная связь** — связь, при которой информация передаётся только в одном направлении.
**Дуплексная связь** — способ связи, при котором передача возможна в обоих направлениях канала связи.
**Полудуплексная связь** (Semi-duplex) — способ симплексной связи на одном конце линии и дуплексной связи на другом.

**Netty uses nio eventloops**. On Windows Netty uses java implementation for nio and Netty threads have `nio` prefix in their names, on Linux Netty uses native platform implementation and its threads have `epoll` prefix in their names.