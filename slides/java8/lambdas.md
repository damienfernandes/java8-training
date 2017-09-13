### Lambdas

---

* _Like_ syntactic sugar for an anonymous class
* Type of a lambda expression is a _functional interface_
* Syntax = "parameters -> body"

---

* Declaring the types of the parameters is optional
* Using parentheses around the parameter is optional if you have only one parameter

```java
() -> System.out.println(this)
str -> System.out.println(str)
(String str) -> System.out.println(str)
```

---

* Using curly braces is optional (unless you need multiple statements)
* The _return_ keyword is optional if you have a single expression that returns a value

```java
(s1, s2) -> s2.length() - s1.length()
(String s1, String s2) -> { return s2.length() - s1.length(); }
```

---

### Example with the Runnable interface
```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```

---

```java
import static java.lang.System.out;

public class Hello {
  Runnable r1 = () -> out.println(this);
  Runnable r2 = () -> out.println(toString());

  public String toString() {
    return "Hello, world!";
  }

  public static void main(String... args) {
    new Hello().r1.run(); //Hello, world!
    new Hello().r2.run(); //Hello, world!
  }
}
```

note: Have a look at the same implementation in Java 7. Reference to _this_ and _toString()_ are way more complicated.

---

### Scope
* Methods of the class are available in the _body_ of the lamdba expression
* Possible to refer to final or _effectively_ final variables

---

### Method references

* Syntax sugar for lambda expressions
* Refer to existing methods instead of using a lambda expression

---

### Kinds of Method References

| Kind            | Example                              |
| --------------- | ------------------------------------ |
| Static method   | ContainingClass::staticMethodName    |
| Instance method | containingObject::instanceMethodName |
| Class method    | ContainingType::methodName           |
| Constructor     | ClassName::new                       |

---

Example

```java
Arrays.sort(populationArray,
    (Person a, Person b) -> {
        return a.getBirthday().compareTo(b.getBirthday());
    }
);
Arrays.sort(populationArray,
    (a, b) -> Person.compareByAge(a, b)
);
Arrays.sort(populationArray, Person::compareByAge);
```

---

### Functional interfaces

* Interface with exactly one **abstract** method
* Applies to interfaces that were created with previous versions of Java
* Any interface can be functional interface
* `@FunctionalInterface` annotation causes a compilation error if interface doesn't satisfy requirements

---

* Build-in new functional interfaces:
  * `Function<T,R>` - takes an object of type T and returns R

  * `Supplier<T>` - just returns an object of type T

  * `Predicate<T>` - returns a boolean value based on input of type T

  * `Consumer<T>` - performs an action with given object of type T

  * `BiFunction`, `BiConsumer`


---

### A few examples

---

```java
// Function that prefixes a name with @
Function<String, String> atr = (name) -> {return "@" + name;};

// Function that returns length of a String
Function<String, Integer> leng = (name) -> name.length();
Function<String, Integer> leng2 = String::length;
```
---

Printing out a list of `String`:

```java
// Java 7
for (String s : list) {
    System.out.println(s);
}
//Java 8
list.forEach(System.out::println);
```

---

Sorting a list of `String`:

```java
// Java 7
Collections.sort(list, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
});
//Java 8
Collections.sort(list, (s1, s2) -> s1.length() - s2.length());
// or
list.sort(Comparator.comparingInt(String::length));
```