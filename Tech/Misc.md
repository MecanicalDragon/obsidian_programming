**Симплексная связь** — связь, при которой информация передаётся только в одном направлении (теле- и радиовещание)
**Дуплексная связь** — способ связи, при котором передача возможна одновременно в обоих направлениях канала связи (телефон).
**Полудуплексная связь** (Semi-duplex, half-duplex) — передача в обе стороны, но поочереди (рация).

---
**Netty uses nio eventloops**. On Windows Netty uses java implementation for *NIO* and Netty threads have `nio` prefix in their names; on Linux, Netty uses native platform implementation and its threads have `epoll` prefix in their names. ^epoll