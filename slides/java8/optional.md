### Optional

---

"_Our intention was to provide a **limited mechanism for library method return types** where there needed to be a clear way to represent “no result”, and using null for such was overwhelmingly likely to cause errors._"

Brian Goetz in an [answer on stackoverflow](https://stackoverflow.com/questions/26327957/should-java-8-getters-return-optional-type/26328555#26328555)

---

* Functional way to handle *null*
* Avoid `NullPointerException`

---

* It's a **monad** because it is a parameterized type (`Optional<T>`) that:
  * Implements a _bind_ operation `flatMap()`
  * Implements a _unit_ function `of()`
  * Follows three laws: _left identity_, _right identity_ and _associativity_

---

An Optional wraps a value using:

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

- `ifPresent(Consumer<? super T> consumer)` : Executes the given Consumer only if there is a value present (no return value)
- `filter(Predicate<? super T> predicate)` : If the wrapped value matches the predicate then return an Optional with the value otherwise return an empty Optional

---

- `map(Function<? super T,? extends U> mapper)` : Applies the given mapping Function and returns a new Optional
- `flatMap(Function<? super T,Optional<U>> mapper)` : Applies the given mapping Function, unwrap the resulting value and returns a new Optional

---

#### Remember one thing

If you're using `Optional ` and you realize that you systematically write `isPresent()` or `get()` then you are wrong and can do it better.

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

* Use Optional as return type when value can be null

```java
public Optional<Address> getAddress() {
    AddressDto address = PersonDao.findAddress(this.id);
    return Optional.ofNullable(address);
}
```

---

### When NOT use Optional ?

---

#### Do not wrap a Collection

* Remember: Optional is a way to represent _no result_
* Optional does not provide any additional value than an **empty Collection**
* Wrap a Collection in an Optional only complicates code

---

#### Do not use in POJO fields and getters

* Optional deliberately [doesn’t implement the Serializable interface](http://mail.openjdk.java.net/pipermail/jdk8-dev/2013-September/003274.html)
* If your POJO accessors are using Optional it will be more complicated for a library or framework to use reflection to manipulate them
* However some people would like to use Optional this way (like [Stephen Colebourne](http://blog.joda.org/2015/08/java-se-8-optional-pragmatic-approach.html), contributor of Joda-Time)

---

#### Do not use in constructor or method parameters

* Optional was not created for this purpose
* When a method can accept optional parameters, it’s preferable to use method overloading and declare separate constructors

---

### Practice

Rewrite following method using `Optional`:

```java
public String getNickname(Person person) {
    if (person != null) {
        return person.getNickname();
    }
    return Person.defaultNickname();
}
```

---

Imagine `getCurrentServiceContext(...)` returns an `Optional<ServiceContext>`.  

Rewrite following method using `Optional`:

```java
public String getSessionId(String subSettingPrefix) {
    String result;
    ServiceContext current = getCurrentServiceContext(subSettingPrefix);
    if (current != null) {
        result = current.getSessionId();
    } else {
        // Else take the current session id
        BusinessSessionManagementDelegate delegate = new BusinessSessionManagementDelegate();
        result = delegate.getSessionId();
    }
    return result;
}
```

