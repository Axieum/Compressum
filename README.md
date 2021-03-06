# Compressum
> A Java library for easily handling archive creation

## Getting Started

The source and binaries are available for download on the [releases page](https://github.com/Axieum/Compressum/releases).

Alternatively, you can pull from Maven Central repositories:

```xml
<dependency>
  <groupId>me.axiuem.java.compressum</groupId>
  <artifactId>Compressum</artifactId>
  <version>1.0.2</version>
</dependency>
```

### Examples

Calling `Compressum#compress()` expects an output file and an archive format to be set - prematurely invoking will cause an exception to be thrown in the future.

For the convenience of the consumer, archiving can be quite a time consuming task and thus in order not "block" your code
a `CompletableFuture<File>` instance is immediately returned (learn more at [Java Docs](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)) -
accessible through `CompletableArchive#getTask()`.
Here, you can add callbacks for when the archive is ready, handle exceptions, etc. - though most future functions are transitively available through `CompletableArchive`.

```java
File output = new File("New Folder/Compressum.zip");
Compressum comp = new Compressum(output, Format.ZIP);
// Add files or directories (recursive)...
comp.addEntry(new File("README.md"));
comp.addEntry(new File("New Text Document.txt"), "LICENSE.txt");
comp.addEntry(new File("Folder/SubFolder"), "Folder");

comp.compress().then((archiveFile) -> {
    // The compression was successful, do stuff
    System.out.println('\'' + archiveFile.getName() + "' compressed!");
}).exceptionally((e) -> {
    // The compression failed, do stuff
    e.printStackTrace();
    System.out.println("Compression failed.");
    return null; // Must return a default value
}).after(() -> {
    // The compression is finished, do stuff
    System.out.println("Compression finished!");
});
```

By calling `CompletableArchive#get()`, you can block your code to wait for the archive (think synchronously).

```java
try {
    File archive = comp.compress().get();
} catch (Exception e) {
    e.printStackTrace();
}
```

## Contributing

Pull requests are encouraged here on GitHub! Learn more in the [contribution guidelines](CONTRIBUTING.md).

## License

This **Compressum** code is protected under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). View the full license in the [LICENSE.txt](LICENSE.txt).

## Additional Resources

* [Apache Commons Compress](https://commons.apache.org/proper/commons-compress/)
