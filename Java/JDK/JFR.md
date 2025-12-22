**Typical launch arguments:**
`-XX:StartFlightRecording=disk=true,dumponexit=true,maxage=30m,maxsize=50m,
`settings=profile.jfc, filename=/dumps/jfrdump.jfr`
`-XX:FlightRecorderOptions=repository=/dumps, maxchunksize=50m,stackdepth=1024

`jcmd -l`, `jps` - commands to know PID of application
`jcmd <PID> JFR.XXX` - commands to manage JFR on flight
`jstack -l <PID> > td.txt` - thread dump to file
`jmap -dump:[live],format=b,file=<file-path> <PID>` - do the heap dump
`jcmd <pid> GC.heap_dump <file-path>` - do the heap dump
`jcmd <pid> JFR.dump <file-path>` - JFR dump
`kill -3 <PID>` - Linux command that kills process and outputs thread dump to process output

[Habr JFR guide](https://habr.com/ru/company/krista/blog/532632/)
