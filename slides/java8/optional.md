### Optional

---

"_Our intention was to provide a **limited mechanism for library method return types** where there needed to be a clear way to represent “no result”, and using null for such was overwhelmingly likely to cause errors._"

Brian Goetz in an [answer on stackoverflow](https://stackoverflow.com/questions/26327957/should-java-8-getters-return-optional-type/26328555#26328555)

---
* Functional way to handle *null* (and avoid `NullPointerException`)
* It's a monad because it is a parameterized type (`Optional<T>`) that:
  * Implements a _bind_ operation `Optional.flatMap()` and a _unit_ function `Optional.of()`
  * Follows three laws: _left identity_, _right identity_ and _associativity_

---

Create an Optional that wraps a value using:

* `of(x)` for a non-null value
* `ofNullable(x)` for a reference that may or may not be null
* `empty()` to represent a missing value

---

Deal with missing values using:

- `orElse(T)` to return a default value if the Optional is empty
- `orElseGet(Supplier<T>)` to call a given Supplier to provide a value if the Optional is empty
- `orElseThrow(Supplier<X extends Throwable>)` to call a given Supplier for an exception to throw if the Optional is empty

---

Work with Optional's using functional style methods:

- `filter(Predicate<? super T> predicate)` : If the wrapped value matches the predicate then return an Optional with the value otherwise return an empty Optional
- `map(Function<? super T,? extends U> mapper)` : Applies the given mapping Function and returns a new Optional
- `flatMap(Function<? super T,Optional<U>> mapper)` : Applies the given mapping Function, unwrap the resulting value and returns a new Optional
- `ifPresent(Consumer<? super T> consumer)` : Executes the given Consumer only if there is a value present (no return value)

---

Example

```java
public boolean isLocatedNearSopra(Person person) {
     return Optional.ofNullable(person)
       .flatMap(Person::getAddress)
       .map(Address::getStreet)
       .filter(street -> street.contains("Pré Faucon"))
       .isPresent();
 }
```

---

Several methods on the `Stream` interface return Optional (in case there are no values in the Stream):

- `reduce(BinaryOperator<T> accumulator)` : Reduces the stream to a single value
- `max(Comparator<? super T> comparator)` : Finds the maximum value
- `min(Comparator<? super T> comparator)` : Finds the minimum value
- ...

---

### So when should one use Optional ?

---

#### Method result

```java
public Optional<Address> getAddress() {
    AddressDto address = PersonDao.getAddress(this.id);
    return Optional.ofNullable(address);
}
```

Usage:

```java
getFilePath("application.properties").ifPresent(System.out::println);
```

---

### When NOT use Optional ?

---

#### Do not wrap a Collection

* Remember: Optional is a way to represent _no result_
* Optional doesn’t provide any additional value than an empty Collection and only complicates code

#### Do not use in POJO fields and getters

* Optional deliberately doesn’t implement the Serializable interface](http://mail.openjdk.java.net/pipermail/jdk8-dev/2013-September/003274.html)

#### Do not use in constructor or method parameters