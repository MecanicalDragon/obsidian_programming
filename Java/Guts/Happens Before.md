Modern CPUs are able to reorder instructions if they don’t depend on each other. Reorderings are possible in JVM either. Happens Before is a set of rules that determines how JVM and CPU are allowed to reorder instructions for performance gains. Several most popular scenarios are below:<br>
**Synchronization**
- In the synchronized block monitor acquiring HB all following instructions for the thread.
	- Also, there exists a guarantee that all variables visible to a thread will be refreshed from the main memory.
- Monitor releasing on the synchronized block leaving  HA all previous instructions in the thread.
	- Also, there exists a guarantee that all variables visible to a thread will be flushed to the main memory.
		- Hence, the JVM optimizer can bring instructions into the sync block but not vice versa.
- Monitor releasing HB all other threads monitor acquiring.
**Read/Write**
- Variable write and sequential variable read in the same thread always respect the HB contract.
- Volatile writes HA all previous instructions in the same thread. (1).
	- Also, there exists a guarantee that all variables visible to a thread will be flushed to the main memory (2).
- Volatile read HB all following instructions in the same thread (1).
	- Also, there exists a guarantee that all variables visible to a thread will be refreshed from the main memory (2).
		- That means everything before volatile HB everything after.
- Volatile long and double variables obtain atomic read and write properties. Non-volatile longs and doubles are not atomic on 32-bit systems (3).
- BTW, for reference types volatile guarantees apply only to reference updates but not referenced object inner fields.
Threads
- Thread start HB all the code in the thread.
- All the thread code HB thread variables nulling.
- Thread code HB join() operation. Thread code HB isAlive() == false.
- Thread.interrupt() HB thread stopping fact revealing.
Objects
- Static initialization block HB any object instance actions.
- Final field assignment in constructor HB any other actions after.
- Any actions with an object HB finalize().
<br>Happens before — отношение строгого частичного порядка (антирефлексивное, антисимметричное, транзитивное), введённое между атомарными командами, придуманное Лесли Лэмпортом и не означающее «физически прежде», но гарантирующее, что вторая команда будет «в курсе» изменений, проведённых первой.
