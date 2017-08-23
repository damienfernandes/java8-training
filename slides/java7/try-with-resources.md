### try-with-resources

---

### before Java 7
```java
String readFirstLineFromFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        if (br != null) br.close();
    }
}
```

---

### with Java 7
```java
String readFirstLineFromFile(String path) throws IOException {
    try (BufferedReader br =
                   new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```

---

* Try statement that declares *one or more* resource(s)
* Ensures that each resource is closed at the end of the statement
* `catch` or `finally` blocks are run *after* the resources have been closed

---

* Any object that implements `java.lang.AutoCloseable` can be used as a resource
* Interface `java.io.Closeable` extends `java.lang.AutoCloseable`