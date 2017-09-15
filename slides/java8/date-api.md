### New Date and Time API

---

* More comprehensive than the previous API
* Immutable types
* Thread-safe
* Very similar to Joda-Time

---

### New classes

- `LocalDate` – Day, month, year
- `LocalTime` – Time of day only
- `LocalDateTime` – Both date and time

---

```java
// Java 7
Calendar cal = Calendar.getInstance();
cal.add(Calendar.HOUR, 8);
Date eightHoursAfter = cal.getTime(); // actually returns a Date
```

```java
// Java 8
LocalTime now = LocalTime.now();
LocalTime eightHoursAfter = now.plus(8, HOURS);
```

---

Also well-named methods:

```java
LocalDate today = LocalDate.now();

// Each method returns a different instance of LocalDate, today remains unchanged
LocalDate thirtyDaysFromNow = today.plusDays(30);
LocalDate nextMonth = today.plusMonths(1);
LocalDate aMonthAgo = today.minusMonths(1);
```

---

### Create date and time

---

```java
// Today
LocalDate today = LocalDate.now();

// Now
LocalDateTime now = LocalTime.now();
```

---

```java
// new LocalDate for March 15, 2014 is as simple as:
LocalDate date = LocalDate.of(2014, 3, 15);
System.out.println(date.getMonth());
// or
date = LocalDate.of(2014, Month.MARCH, 15);
System.out.println(date.getMonth());

// add time to date
LocalTime time = LocalTime.of(12, 15, 0);
LocalDateTime datetime = date.atTime(time);
```

---

#### New Enum's

* `java.time.temporal.ChronoUnit`
* `java.time.DayOfWeek`
* `java.time.Month`

```java
LocalDate today = LocalDate.now();
LocalDate nextWeek = today.plus(1, ChronoUnit.WEEKS);
LocalDate nextMonth = today.plus(1, ChronoUnit.MONTHS);
LocalDate nextYear = today.plus(1, ChronoUnit.YEARS);
```

