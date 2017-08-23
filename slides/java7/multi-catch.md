### Multi-catch

---

### Before Java 7

```java
try {
  doSomething();
} catch (Exception1 e) {
  handleException();
} catch (Exception2 e) {
  handleException();
}
```

---

### With Java 7

```java
try {
  doSomething();
} catch (Exception1 | Exception2 e) {
  handleException();
}
```