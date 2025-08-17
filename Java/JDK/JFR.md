**Typical launch arguments:**
`-XX:StartFlightRecording=disk=true,dumponexit=true,maxage=30m,maxsize=50m,
`settings=profile.jfc, filename=/dumps/jfrdump.jfr`
`-XX:FlightRecorderOptions=repository=/dumps, maxchunksize=50m,stackdepth=1024

`jcmd -l`, `jps` - commands to know PID of application
`jcmd <PID> JFR.XXX` - commands to manage JFR on flight
`jstack -l <PID> > td.txt` - thread dump to file
`jmap -dump:[live],format=b,file=<file-path> <PID>` - do the heap dump
`jcmd <pid> GC.heap_dump <file-path>` - do the heap dump
`jcmd <pid> JFR.dump <file-path>` - JFR dump
`kill -3 <PID>` - Linux command that kills process and outputs thread dump to process output

**jconsole** - utility that allows to monitor JVM process and control MBeans. To enable remote JMX monitoring follow JMX guides: [one](https://iceburn.medium.com/how-to-expose-jmx-in-kubernetes-b1faad450451), [two](https://www.geeksforgeeks.org/how-to-enable-jmx-for-java-application-running-in-the-kubernetes-cluster/).
The only difference: `-Djava.rmi.server.hostname=0.0.0.0`. To access remote jmx port specify `<pod pi>:<enabled jmx port>`.

[Habr JFR guide](https://habr.com/ru/company/krista/blog/532632/)
