**JDWP** (Java Debug Wire Protocol) — это встроенная в JVM библиотека, позволяющая пользоваться удаленным дебагом. Чтобы ее активировать, нужна JVM опция:
- `-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:{{ .Values.debug.jdwp.port }}`

После этого подключиться для удаленного дебага можно прямо из IDEA (*Run Configurations* -> *Remote JVM Debug*) по ip хоста и порту пода.
