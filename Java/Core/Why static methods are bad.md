Object-orientation is about three things:
- messaging
- local retention and protection, and hiding of the process state
- extreme late binding of all things.

Static methods violate at least messaging and late binding.

The idea of messaging in OOP means that computation is performed by interaction of self-contained objects. Sending a message is the only way of communication/computation. Static methods don’t do that. They aren’t associated with any object. They really aren’t methods at all, but just procedures or subroutines.

As for late binding: a more modern name for that is dynamic dispatch. Static methods cannot be overridden, so they violate it too. You can also execute every object in parallel in its own process, since they only communicate via messaging, thus providing some trivial concurrency. Static methods, and especially static state break that nice property.

[This question is about OOP](https://ru.stackoverflow.com/questions/748372/%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D1%8C%D0%BD%D0%BE-%D0%B4%D0%B5%D0%BB%D0%B0%D1%82%D1%8C-%D0%BF%D1%80%D0%B8%D0%B2%D0%B0%D1%82%D0%BD%D1%8B%D0%B5-%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D1%8B-java-%D1%81%D1%82%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%BC%D0%B8-%D0%B8%D0%BB%D0%B8-%D0%BD%D0%B5%D1%82-%D0%9F%D0%BB%D1%8E%D1%81%D1%8B-%D0%B8-%D0%BC%D0%B8%D0%BD%D1%83%D1%81%D1%8B-%D0%BA%D0%B0%D0%B6%D0%B4)
