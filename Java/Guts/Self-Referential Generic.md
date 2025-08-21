Self-referential generic (or **self-bound type,** or **recursive generic**) is a generic construction that looks like `EnumᐸE extends EnumᐸEᐳᐳ`. Yes, it is used in [[Enum]]; but which problems does it solve?

First, it allows to use methods and interfaces of the parent in its children and distinguish types, avoiding runtime exceptions during interactions with them. 
```java
public abstract class MyEnum implements Comparable<MyEnum>{}
class Animal extends MyEnum {}
class Converter extends MyEnum {}

public int compareTo(MyEnum o) {
   if (o instanceOf Animal other) {
      // compare and return
   }
   throw IllegalArgumentException();
}
```
For example, this code allows to compare classes that are not compatible and get an exception in runtime. Self-referential generic allows to detect such issues on a compile-time.
```java
public abstract class Enum<E extends Enum<E>> implements Comparable<E>{}
class Animal extends Enum<Animal>{}
class Converter extends Enum<Converter> {}
public int compareTo(Animal o){}
```
With this approach `animal.compareTo(converter)` will just not compile.

Second, it allows not to loose type data while using parent methods in children.
```java
class Delivery {
	final long id;
	final boolean isActive;

	public Delivery cancelled() {
	    return new Delivery(this.id, false);
	}
}

class FastDelivery extends Delivery {
	private final long courierId;
}

FastDelivery fast = new FastDelivery();
Delivery cancelled = fast.cancelled();

long courierId = fast.getCourierId();
```
 Last code line won't compile. We need to know exact type and use direct casting to get courierId.

 ```java
 public class DeliveryᐸT extends DeliveryᐸTᐳᐳ {
	protected T create(long id, boolean isActive) {
		return (T) new Delivery(id, isActive);
	}

	public T cancelled() {
		return create(this.id, false);
	}
 }
public class FastDelivery extends DeliveryᐸFastDeliveryᐳ {
	protected FastDelivery create(long id, boolean isActive) {
		return new FastDelivery(this.id, this.isActive, courierId);
	}
}

FastDelivery fast = new FastDelivery(…);
FastDelivery cancelled = fast.cancelled();

long id = cancelled.getCourierId();
```
Self-referential generic solves the problem.
