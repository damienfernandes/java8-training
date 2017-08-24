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

```

---


 