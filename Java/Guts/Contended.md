`@Contended` - annotation introduced in JDK8 that is a hint for the JVM to isolate annotated fields to avoid [[false sharing]]. This annotation forces the compiler to add paddings around the annotated fields to guarantee they won't be fetched in the same [[Cacheline]] with other fields.

It is possible to group fields in the same cache line by passing the *groupName* into the annotation.

Padding size is controlled by JVM flag `-XX:ContendedPaddingWidth=128`
To make it work the other flag `-XX:-RestrictContended` is required.
