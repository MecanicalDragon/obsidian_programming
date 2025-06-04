First of all, they are not explicitly bad, but we must be very careful to use them.

1.      It causes memory leaks:
Some Executors (and thus app servers based on executors) reuse threads instead of creating a new one for each HTTP Request, so ThreadLocals start to stash garbage. If an application for some reason doesn't remove the data stored in ThreadLocal, it will remain with the Thread (from the application server thread pool). Since the lifetime of the pooled Thread surpasses that of the application, it will prevent the object and its Classloader from being garbage collected, that leads to *Memory Leak*. Also, this may lead to a skewed logic if a thread suddenly sees a state of the previous request.

2.      It can be considered as a global state of your thread:
ThreadLocal gives an opportunity to use the variables without explicitly passing them down through the method invocation chain. Which could be useful on certain occasions. But when we have created a n-layer architecture to abstract away different communication interfaces, we still have an opportunity to grab HttpServletRequest in DAO.

3.      It signals about poor architecture and provokes to make ugly design decisions.

[https://smartbear.com/blog/how-and-when-to-use-javas-threadlocal-object/](https://smartbear.com/blog/how-and-when-to-use-javas-threadlocal-object/)
[https://plumbr.io/blog/locked-threads/how-to-shoot-yourself-in-foot-with-threadlocals](https://plumbr.io/blog/locked-threads/how-to-shoot-yourself-in-foot-with-threadlocals)
