### Streams

---

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
        .collect(Collectors::toList);
```

Stream + lambdas = basis of functional-style programming in Java 8

---

### Generate streams

---

#### Collections

The Collection interface has two default methods on it for creating streams:

- stream(): Returns a sequential Stream with the collection as its source
- parallelStream(): Returns a possibly parallel Stream with the collection as its source

---

#### Files

Read a file as a Stream using `Files.lines(Path filePath)`:

```java
try (Stream st = Files.lines(Paths.get("/path/to/file"))) {
    st.forEach(System.out::println);
}
```

note: Any `IOException` that is thrown while processing the file (after the file is opened) will get wrapped in an `UncheckedIOException` and thrown

---

Navigate file trees using a Stream using static methods on the `Files` class:

- `list(Path dir)` : Stream of files in the given directory
- `walk(Path dir)` : Stream that traverses the file tree depth-first starting at the given directory
- `walk(Path dir, int maxDepth)` : Same as `walk(dir)` but with a maximum depth

---

#### Infinite streams

Use the `generate` or `iterate` static methods on Stream:

````java
Stream.generate(() -> Math.random())
      .forEach(System.out::print);
````

```java
Stream.iterate(1, i -> i+1)
      .forEach(System.out::print);
```

---

#### Ranges

Each primitive Stream (`IntStream`, `DoubleStream`, and `LongStream`) has a corresponding `range` method:

```java
IntStream.range(1, 11)
         .forEach(System.out::println);
```

---

#### Anything

You can create a Stream from any number of elements or an array using the two following methods:

```java
Stream<Integer> s1 = Stream.of(1, 2, 3);
Stream<Object> s2 = Arrays.stream(array);
```

`Stream.of` can take any number of parameters of any type.

---

### Operations

* Intermediate operation: return a new stream, lazy
* Terminal operation: produce a result or a side-effect

|     Type     |        Operations        |
| :----------: | :----------------------: |
| Intermediate |    map, filter, peek     |
|   Terminal   | forEach, reduce, collect |

---

#### ForEach

* Replaces the _for loop_
* More concise and more object-oriented

```java
Files.list(Paths.get("."))
     .forEach(System.out::println);
```

---

#### Map

Apply a function to the elements of the stream

```java
List<String> nameList = persons.stream()
                .map(p -> p.getFirstname() + " " + p.getLastname())
                .collect(Collectors::toList);
```

---

#### Filter

Return a stream consisting of the elements that match a given predicate

```java
List<Widget> redWidgets = widgets.stream()
                .filter(w -> w.getColor() == RED)
                .collect(Collectors::toList);
```

---

#### FlatMap

```java
List<Developer> team = new ArrayList<>();
Developer polyglot = new Developer("polyglot").knows("scala").knows("groovy");
Developer pragmatic = new Developer("pragmatic").knows("java").knows("javascript");

team.add(polyglot);
team.add(busy);

List<String> teamLanguages = team.stream()
                                 .map(d -> d.getLanguages())
                                 .flatMap(l -> l.stream())
                                 .collect(Collectors::toList);
teamLanguages.forEach(System.out::println);
```



---

### Reductions

* Combine elements into a single summary result by repeated application of a combining operation
* Class `Stream` provides methods for the most common reduction operations:
  * max(), min(), count()
  * allMatch(), noneMatch(), anyMatch()

---

Calculate the sum of the members age in a collection :

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

Check that at least one developer knows `Java` language :

````java
boolean atLeastOneKnowsJava = team.stream()
                .map(d -> d.getLanguages())
                .flatMap(l -> l.stream())
                .anyMatch(l -> "java".equals(l));
````

---

### Collectors

* Accumulate elements into a _mutable_ result container (Collection, String, ...)
* Consists of three things:
  - A **supplier** of an initial value
  - An **accumulator** which adds to the initial value
  - A **combiner** which combines two results into one
* Use `collect(supplier,accumulator,combiner)` or `collect(Collector)`

---

Simple Collectors

```java
// Accumulate names into a List
List<String> list = persons.stream()
        .map(Person::getName)
        .collect(Collectors::toList);

// Accumulate names into a TreeSet
Set<String> set = persons.stream()
        .map(Person::getName)
        .collect(Collectors.toCollection(TreeSet::new));
```

---

Joining (similar to Apache Commons’ `StringUtil.join`)

```java
String names = persons.stream()
        .map(Person::getName)
        .collect(Collectors.joining(", "));
```

---

Collect multiple statistics about a collection

```java
// Compute the count of people, as well as the minimum, maximum, sum, and average of their number of dependents
IntSummaryStatistics stats = people.stream()
                                   .collect(Collectors.summarizingInt(Person::getDependents));
System.out.println(stats.getAverage());
System.out.println("count = " + stats.getCount());
System.out.println("max = " + stats.getMax());
System.out.println("min = " + stats.getMin());
```

---

Grouping

```java
// Group by first letter of name
Map<Character,List<Developer>> map = developers.stream()
        .collect(Collectors.groupingBy(dev -> dev.getName().charAt(0)));
map.forEach((k,v) ->
            System.out.println(k + " " + v.stream()
                               .map(Developer::getName)
                               .collect(Collectors.joining(", "))));
```

---

Partitioning

```java
// Group by whether or not the developer knows Java
Map<Boolean,List<Developer>> map = developers.stream()
        .collect(Collectors.partitioningBy(Developer::knowsJava));
```

---

### Practice

---

* Read a list of `Person` (firstName, lastName, email, age) from file *MOCK_DATA.csv*
* Compute the average age of the mens older than 35
* Collect the email of the people older than 35
* Get oldest person
* Get people age statistics (average, count, max, min)
