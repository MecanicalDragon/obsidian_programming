JVM determines when it can invoke *ﬁnalize()* method of an object based upon the state of the object. The state of the object can be described as the cross product of **reachability** and **ﬁnalization**. The state describing the reachability of an object falls into one of three categories, these are:<br>
- **reachable** - an object is reachable if it can potentially be accessed from any live thread.
- **f-reachable** - an object is f-reachable if it can be reached by a chain of references from some ﬁnalizable object (possibly including itself if it is ﬁnalizable), and it is not reachable.
- **unreachable** - an object is unreachable if it is not reachable, and it is not f-reachable.
<br>The automatic invocation of the *ﬁnalize()* method and the readiness of the object to have its ﬁnalize method invoked is characterized by the ﬁnalization state of the object. There are three possible states:<br>
- **unﬁnalized** - an object is unﬁnalized if it has not had its ﬁnalize method automatically invoked.
- **ﬁnalizable** - an object that has not had its ﬁnalize method automatically invoked, but the JVM may eventually automatically invoke its ﬁnalizer.
- **ﬁnalized** - an object which has had its ﬁnalize method automatically invoked.
<br>[Article](https://www.researchgate.net/publication/2889098_Java_Finalize_Method_Orthogonal_Persistence_and_Transactions#pf3)
