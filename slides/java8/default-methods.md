### Default methods

---

* Put code in interfaces
* Add methods in existing interfaces without breaking existing implementations

```java
public interface Stream {
  ...

  default public Stream stream() {
    return StreamSupport.stream(spliterator());
  }
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

#### When use default methods ?

* Useful to enhance existing interfaces in an API without breaking anything
* Was essential in order to add lambda support to existing Java `Collection`'s
* But not a feature appropriate to use every day

---

#### Not only default methods

* **Static** method is allowed in interface too in Java 8

```java
public interface Stream {
  ...

  /**
   * Returns a sequential ordered stream whose elements are the specified values.
   *
   * @param <T> the type of stream elements
   * @param values the elements of the new stream
   * @return the new stream
   */
  public static<T> Stream<T> of(T... values) {
    return Arrays.stream(values);
  }
}
```
