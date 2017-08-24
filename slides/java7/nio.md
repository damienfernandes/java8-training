### File I/O with NIO.2

---

* A _New I/O API_ (NIO) was introduced in Java 1.4 under package `java.nio`
* Java 7 enhances this API with _NIO.2_ (New I/O version 2?)

---

### java.nio.file.Files

Utility methods for file related operations

* `copy()` : copy a file
* `move()` : move or rename a file
* `readAllBytes()` : similar to the Apache `IOUtils.readFileToByteArray()`
* `createSymbolicLink()`: creates a symbolic link

---

### WatchService API

* API to watch file system events
* Events like entry _created_ / _deleted_ / _modified_

---

```
WatchService watchService
  = FileSystems.getDefault().newWatchService();

Path path = Paths.get(System.getProperty("user.home"));

path.register(
  watchService, 
    StandardWatchEventKinds.ENTRY_CREATE, 
      StandardWatchEventKinds.ENTRY_DELETE, 
        StandardWatchEventKinds.ENTRY_MODIFY);

WatchKey key;
while ((key = watchService.take()) != null) {
    for (WatchEvent<?> event : key.pollEvents()) {
        System.out.println("Event kind:" + event.kind() 
            + ". File affected: " + event.context() + ".");
    }
    // return the key to the ready state
    key.reset();
}
```

---

```bash
> Event kind:ENTRY_CREATE. File affected: New Text Document.txt.  
> Event kind:ENTRY_DELETE. File affected: New Text Document.txt.  
> Event kind:ENTRY_CREATE. File affected: testFile.txt.  
> Event kind:ENTRY_MODIFY. File affected: testFile.txt.  
```
