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

note: Show implementation in Java 7. Reference to _this_ and _toString()_ are way more complicated.

---

### Scope
* Methods of the class are available in the _body_ of the lamdba expression
* Possible to refer to final or _effectively_ final variables

---

### Method references

* Syntax sugar for lambda expressions
* Works for:
  * static method
  * instance method of an object of a particular type
  * instance method of an existing object
  * constructor

---

Example

```java
Arrays.sort(rosterAsArray,
    (Person a, Person b) -> {
        return a.getBirthday().compareTo(b.getBirthday());
    }
);
Arrays.sort(rosterAsArray,
    (a, b) -> Person.compareByAge(a, b)
);
Arrays.sort(rosterAsArray, Person::compareByAge);
```

---

### Kinds of Method References

| Kind                                 | Example                              |
| ------------------------------------ | ------------------------------------ |
| static method                        | ContainingClass::staticMethodName    |
| instance method                      | containingObject::instanceMethodName |
| instance method of a particular type | ContainingType::methodName           |
| constructor                          | ClassName::new                       |

---

### Functional interfaces

* Interface with exactly one **abstract** method
* Applies to interfaces that were created with previous versions of Java
* Any interface can be functional interface
* `@FunctionalInterface` annotation causes a compilation error if interface doesn't satisfy requirements

---

* Build-in new functional interfaces:
  * Function<T,R> - takes an object of type T and returns R

  * Supplier<T> - just returns an object of type T

  * Predicate<T> - returns a boolean value based on input of type T

  * Consumer<T> - performs an action with given object of type T

  * BiFunction - like Function but with two parameters

  * BiConsumer - like Consumer but with two parameters

---

### A few examples

```java
Function<String, String> atr = (name) -> {return "@" + name;};
Function<String, Integer> leng = (name) -> name.length();
Function<String, Integer> leng2 = String::length;
```
---

### Printing out a list of String

```java
// Java 7
for (String s : list) {
    System.out.println(s);
}
//Java 8
list.forEach(System.out::println);
```

---

### Sorting a list of Strings
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