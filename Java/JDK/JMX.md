JMX allows to attach the running application, monitor process properties, and control *MBeans*. Typical tool for that - *VisualVM* or default JDK utility *jconsole*. To enable JMX use the following JVM options:
```
-Dcom.sun.management.jmxremote  
-Dcom.sun.management.jmxremote.port={{ .Values.jmx.port }}  
-Dcom.sun.management.jmxremote.rmi.port={{ .Values.jmx.port }}  
-Dcom.sun.management.jmxremote.ssl=false  
-Dcom.sun.management.jmxremote.authenticate=false
```
Another important property: `-Djava.rmi.server.hostname`. It makes jmx server tell the client during a connection establishment a hostname where data can be fetch from. For local docker run it must be set to `localhost`

JMX guides: [one](https://iceburn.medium.com/how-to-expose-jmx-in-kubernetes-b1faad450451), [two](https://www.geeksforgeeks.org/how-to-enable-jmx-for-java-application-running-in-the-kubernetes-cluster/).
