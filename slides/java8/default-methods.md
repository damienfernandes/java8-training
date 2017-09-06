### Default methods

* Put code in interfaces
* Add methods in existing interfaces without breaking existing implementations

```java
default public Stream stream() {
	return StreamSupport.stream(spliterator());
}
```

---

* An interface can have **one or more** default methods and still be functional

```java
@FunctionalInterface
public interface Iterable {
	Iterator iterator();
	default void forEach(Consumer<? super T> action) {
		Objects.requireNonNull(action);
		for (T t : this) {
			action.accept(t);
		}
	}
}
```

---

What if a class implements two interfaces and both interfaces define a default method with the same signature ?

---

```java
interface Foo {
	default void talk() {
		out.println("Foo!");
	}
}
interface Bar {
	default void talk() {
		out.println("Bar!");
	}
}
class FooBar implements Foo, Bar {
	@Override
	void talk() { Foo.super.talk(); }
}
```

---

* **Static** methods in interfaces are allowed too in Java 8

```java
// Creates a new stream containing given values
public static<T> Stream<T> of(T... values) {
    return Arrays.stream(values);
}
```
