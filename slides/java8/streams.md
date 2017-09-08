### Streams

* Stream = sequence of objects
* Somewhat like `Iterator` but supports parallel execution
* Goal = efficiently process amounts of data
* Streams are heavily used in _reactive programming_

---

* Supports the filter / map / reduce pattern
* Elements in a stream are **lazily** evaluated

```java
List<String> list = persons.stream()
        .filter(person -> person.getAge() < 40)
        .map(Person::getName)
        .collect(toList());
```

Stream + lambdas = basis of functional-style programming in Java 8

---
### Operations

* Intermediate operation: return a new stream, lazy
* Terminal operation: produce a result or a side-effect

|     Type     |        Operations        |
| :----------: | :----------------------: |
| Intermediate |    map, filter, peek     |
|   Terminal   | forEach, reduce, collect |

---

### ForEach

* Replaces the _for loop_
* More concise and more object-oriented

```java
Files.list(Paths.get("."))
     .forEach(System.out::println);
```

---

### Map

Apply a function to the elements of the stream

```java
List<String> nameList = persons.stream()
                .map(p -> p.getFirstname() + " " + p.getLastname())
                .collect(Collectors.toList());
```

---

### Filter

Return a stream consisting of the elements that match a given predicate

```java
List<Widget> redWidgets = widgets.stream()
                .filter(w -> w.getColor() == RED)
                .collect(Collectors.toList());
```

---

### FlatMap

```java
List<Developer> team = new ArrayList<>();
Developer polyglot = new Developer("polyglot").knows("scala").knows("groovy");
Developer pragmatic = new Developer("pragmatic").knows("java").knows("javascript");

team.add(polyglot);
team.add(busy);

List<String> teamLanguages = team.stream()
                                 .map(d -> d.getLanguages())
                                 .flatMap(l -> l.stream())
                                 .collect(Collectors.toList());
teamLanguages.forEach(System.out::println);
```



---

### Reductions

* Combine elements into a single summary result by repeated application of a combining operation
* Class `Stream` provides methods for the most common reduction operations:
  * max(), min(), count()
  * allMatch(), noneMatch(), anyMatch()

---

Calculate the sum of the members' ages in a collection :

```java
Integer totalAgeReduce = population.stream()
   .map(Person::getAge)
   .reduce(0, (a, b) -> a + b);
```

```java
Integer totalAge = population.stream()
   .mapToInt(Person::getAge)
   .sum();
```

---

### Collectors

* Accumulate elements into a mutable result container (Collection, String, ...)
* Do not aggregate elements
* Consists of three things:
  - A **supplier** of an initial value
  - An **accumulator** which adds to the initial value
  - A **combiner** which combines two results into one
* Use `collect(supplier,accumulator,combiner)`, or `collect(Collector)`

---

Simple Collectors

Joining

Statistics

Grouping and Partitioning

---

### Create streams

* Collections
* Files
* Infinite streams
* Ranges
* Anything

---

### A few examples


