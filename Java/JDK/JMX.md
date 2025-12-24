JMX allows attaching the running application, monitor process properties, and control *MBeans*. A typical tool for that is *VisualVM* or default JDK utility *jconsole*. To enable JMX use the following JVM options:
```
-Dcom.sun.management.jmxremote.port={{ .Values.jmx.port }}  
-Dcom.sun.management.jmxremote.rmi.port={{ .Values.jmx.port }}  
-Dcom.sun.management.jmxremote.ssl=false  
-Dcom.sun.management.jmxremote.authenticate=false
```
Another important property: `-Djava.rmi.server.hostname`. It makes jmx server tell the client during a connection establishment a hostname where data can be fetch from. For local docker run it must be set to `localhost`
### Как включить JMX «на лету» без перезапуска JVM

1. Запустить приложение без всяких JMX-флагов.
2. В кубер конфиге в манифесте *service* не определять JMX порт.

Но при необходимости 
```
kubectl exec -it <pod-name> -- jcmd <PID> ManagementAgent.start \
    jmxremote.port=5190 \
    jmxremote.rmi.port=5190 \
    jmxremote.authenticate=false \
    jmxremote.ssl=false
```
Здесь `<PID>` — это ID процесса приложения (обычно `1`).

`kubectl port-forward <pod-name> 5190:5190`

Поскольку `port-forward` работает напрямую с подом через API Kubernetes, ему неважно, открыт ли этот порт в `Service` или `Ingress`. Для внешнего мира порт будет закрыт, а для локальной машины — доступен.

JMX guides: [one](https://iceburn.medium.com/how-to-expose-jmx-in-kubernetes-b1faad450451), [two](https://www.geeksforgeeks.org/how-to-enable-jmx-for-java-application-running-in-the-kubernetes-cluster/).
