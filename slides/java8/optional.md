### Optional

---

* Goal = avoid return `null ` values (and thus `NullPointerException`)
* Use `Optional.of(x)` to wrap a non-null value
* Use `Optional.empty()` to represent a missing value
* Use `Optional.ofNullable(x)` for a reference that may or may not be null

---

Deal with missing values:

- `orElse(T)` : Returns the given default value if the Optional is empty
- `orElseGet(Supplier<T>)` : Calls on the given Supplier to provide a value if the Optional is empty
- `orElseThrow(Supplier<X extends Throwable>)` : Calls on the given Supplier for an exception to throw if the Optional is empty

---

Functional style methods:

- `filter(Predicate<? super T> predicate)` : Filters the value and returns a new Optional
- `flatMap(Function<? super T,Optional<U>> mapper)` : Performs a mapping operation which returns an Optional
- `ifPresent(Consumer<? super T> consumer)` : Executes the given Consumer only if there is a value present (no return value)
- `map(Function<? super T,? extends U> mapper)` : Uses the given mapping Function and returns a new Optional

---

Several methods on the `Stream` interface return Optional (in case there are no values in the Stream):

- `reduce(BinaryOperator<T> accumulator)` : Reduces the stream to a single value
- `max(Comparator<? super T> comparator)` : Finds the maximum value
- `min(Comparator<? super T> comparator)` : Finds the minimum value