`Object.wait()` uses a monitor, thus the awakened thread is aware of changes made by other threads (respects [[Happens Before]] contract).

[[Unsafe]].`park/unpark` is not synchronized and doesn't check for memory updates, therefore you need to do it manually. Another matter is calling unpark thread must somehow know that the called thread is not destroyed - that's why these methods are unsafe.

`LockSupport.park/unpark` just uses [[Unsafe]] for you under the hood.