**Object Initialization**
1. Static fields and static blocks of ancestors. In each class they are initialized in order of appearance.
2. Static fields and static block of instantiated class.
3. Instance fields and initialization blocks of ancestors. In each class they are initialized in order of appearance.
4. Constructor of ancestor after initialization of its instance fields.
5. Instance fields and initialization blocks of the current class. Also in order of appearance.
6. Constructor of the current class.
