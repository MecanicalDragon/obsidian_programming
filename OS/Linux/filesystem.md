`/bin`, `/usr/bin` - directories for programs. Files located here are able to be executed from the terminal by their name if their chmod is  `u+x`

|   |   |
|---|---|
|`bin`|Essential command binaries|
|`boot`|Static files of the boot loader|
|`dev`|Device files|
|`etc`|Host-specific system configuration|
|`lib`|Essential shared libraries and kernel modules|
|`media`|Mount point for removable media|
|`mnt`|Mount point for mounting a filesystem temporarily|
|`opt`|Add-on application software packages|
|`run`|Data relevant to running processes|
|`sbin`|Essential system binaries|
|`srv`|Data for services provided by this system|
|`tmp`|Temporary files|
|`usr`|Secondary hierarchy|
|`var`|Variable data|
[filesystem specification](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03.html)

|         |                                                 |
| ------- | ----------------------------------------------- |
| `bin`   | Most user commands                              |
| `lib`   | Libraries                                       |
| `local` | Local hierarchy (empty after main installation) |
| `sbin`  | Non-vital system binaries                       |
| `share` | Architecture-independent data                   |
[usr hierarchy](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04.html)

![](linux_directory_map.png)