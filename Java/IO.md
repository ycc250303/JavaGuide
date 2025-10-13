# IO

Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

* `InputStream`/`Reader`: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
* `OutputStream`/`Writer`: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

## 字节流

### InputStream（字节输入流）

* 用于从源头（通常是文件）读取数据（字节信息）到内存中，`java.io.InputStream`抽象类是所有字节输入流的父类。
* 常用方法

  * `read()`：返回输入流中下一个字节的数据。返回的值介于 0 到 255 之间。如果未读取任何字节，则代码返回 `-1` ，表示文件结束。
  * `read(byte b[ ])` : 从输入流中读取一些字节存储到数组 `b` 中。如果数组 `b` 的长度为零，则不读取。如果没有可用字节读取，返回 `-1`。如果有可用字节读取，则最多读取的字节数最多等于 `b.length` ， 返回读取的字节数。这个方法等价于 `read(b, 0, b.length)`。
  * `read(byte b[], int off, int len)`：在 `read(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
  * `skip(long n)`：忽略输入流中的 n 个字节 ,返回实际忽略的字节数。
  * `available()`：返回输入流中可以读取的字节数。
  * `close()`：关闭输入流释放相关的系统资源。
  * `readAllBytes()`：读取输入流中的所有字节，返回字节数组。
  * `readNBytes(byte[] b, int off, int len)`：阻塞直到读取 `len` 个字节。
  * `transferTo(OutputStream out)`：将所有字节从一个输入流传递到一个输出流。
* 一般不会直接单独使用 `FileInputStream` ，通常会配合字节缓冲输入流 `BufferedInputStream`来使用
* `DataInputStream` 用于读取指定类型数据，不能单独使用，必须结合其它流，比如 `FileInputStream`
* ```java
  FileInputStream fileInputStream = new FileInputStream("input.txt");
  //必须将fileInputStream作为构造参数才能使用
  DataInputStream dataInputStream = new DataInputStream(fileInputStream);
  //可以读取任意具体的类型数据
  dataInputStream.readBoolean();
  dataInputStream.readInt();
  dataInputStream.readUTF();
  ```
* `ObjectInputStream` 用于从输入流中读取 Java 对象（反序列化），`ObjectOutputStream` 用于将对象写入到输出流(序列化)

### OutputStream（字节输入流）

用于将数据（字节信息）写入到目的地（通常是文件），`java.io.OutputStream`抽象类是所有字节输出流的父类

常用方法

* `write(int b)`：将特定字节写入输出流。
* `write(byte b[ ])` : 将数组 `b` 写入到输出流，等价于 `write(b, 0, b.length)` 。
* `write(byte[] b, int off, int len)` : 在 `write(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
* `flush()`：刷新此输出流并强制写出所有缓冲的输出字节。
* `close()`：关闭输出流释放相关的系统资源
* 向 output.txt 写入 "JavaGuide"

```java
try (FileOutputStream output = new FileOutputStream("output.txt")) {
    byte[] array = "JavaGuide".getBytes();
    output.write(array);
} catch (IOException e) {
    e.printStackTrace();
}
```

* `FileOutputStream` 通常也会配合 字节缓冲输出流 `BufferedOutputStream` 来使用
* **`DataOutputStream`** 用于写入指定类型数据，不能单独使用，必须结合其它流，比如 `FileOutputStream` 。

## 字符流

* I/O操作为什么要分为字符流和字节流？
  * 字符流是由 Java 虚拟机将字节转换得到的，这个过程还算是比较耗时。
  * 如果我们不知道编码类型就很容易出现乱码问题。
* I/O 流提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。
* 音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。
* 字符流默认采用的是 `Unicode` 编码，我们可以通过构造方法自定义编码。
* Unicode 本身只是一种字符集，它为每个字符分配一个唯一的数字编号，并没有规定具体的存储方式。UTF-8、UTF-16、UTF-32 都是 Unicode 的编码方式，它们使用不同的字节数来表示 Unicode 字符。例如，UTF-8 :英文占 1 字节，中文占 3 字节。

### Reader（字符输入流）

* `Reader`用于从源头（通常是文件）读取数据（字符信息）到内存中，`java.io.Reader`抽象类是所有字符输入流的父类。`Reader` 用于读取文本， `InputStream` 用于读取原始字节。
* 常用方法：
  * `read()` : 从输入流读取一个字符。
  * `read(char[] cbuf)` : 从输入流中读取一些字符，并将它们存储到字符数组 `cbuf`中，等价于 `read(cbuf, 0, cbuf.length)` 。
  * `read(char[] cbuf, int off, int len)`：在 `read(char[] cbuf)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字符数）。
  * `skip(long n)`：忽略输入流中的 n 个字符 ,返回实际忽略的字符数。
  * `close()` : 关闭输入流并释放相关的系统资源。
* `InputStreamReader` 是字节流转换为字符流的桥梁，其子类 `FileReader` 是基于该基础上的封装，可以直接操作字符文件。

```java
try (FileReader fileReader = new FileReader("input.txt");) {
    int content;
    long skip = fileReader.skip(3);
    System.out.println("The actual number of bytes skipped:" + skip);
    System.out.print("The content read from file:");
    while ((content = fileReader.read()) != -1) {
        System.out.print((char) content);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

### Writer（字符输出流）

* `Writer`用于将数据（字符信息）写入到目的地（通常是文件），`java.io.Writer`抽象类是所有字符输出流的父类。
* 常用方法：

  * `write(int c)` : 写入单个字符。
  * `write(char[] cbuf)`：写入字符数组 `cbuf`，等价于 `write(cbuf, 0, cbuf.length)`。
  * `write(char[] cbuf, int off, int len)`：在 `write(char[] cbuf)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字符数）。
  * `write(String str)`：写入字符串，等价于 `write(str, 0, str.length())` 。
  * `write(String str, int off, int len)`：在 `write(String str)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字符数）。
  * `append(CharSequence csq)`：将指定的字符序列附加到指定的 `Writer` 对象并返回该 `Writer` 对象。
  * `append(char c)`：将指定的字符附加到指定的 `Writer` 对象并返回该 `Writer` 对象。
  * `flush()`：刷新此输出流并强制写出所有缓冲的输出字符。
  * `close()`:关闭输出流释放相关的系统资源
* `OutputStreamWriter` 是字符流转换为字节流的桥梁，其子类 `FileWriter` 是基于该基础上的封装，可以直接将字符写入到文件。

```java
try (Writer output = new FileWriter("output.txt")) {
    output.write("你好，我是Guide。");
} catch (IOException e) {
    e.printStackTrace();
}
```

## 字节缓冲流

* IO 操作很消耗性能，缓冲流将数据加载至缓冲区，一次性读取/写入多个字节，从而避免频繁的 IO 操作，提高流的传输效率。
* 字节缓冲流这里采用了装饰器模式来增强 `InputStream` 和 `OutputStream`子类对象的功能。
* 字节流和字节缓冲流的性能差别主要体现在我们使用两者的时候都是调用 `write(int b)` 和 `read()` 这两个一次只读取一个字节的方法的时候。由于字节缓冲流内部有缓冲区（字节数组），因此，字节缓冲流会先将读取到的字节存放在缓存区，大幅减少 IO 次数，提高读取效率。

### BufferedInputStream（字节缓冲输入流）

* `BufferedInputStream` 从源头（通常是文件）读取数据（字节信息）到内存的过程中不会一个字节一个字节的读取，而是会先将读取到的字节存放在缓存区，并从内部缓冲区中单独读取字节。这样大幅减少了 IO 次数，提高了读取效率。
* `BufferedInputStream` 内部维护了一个缓冲区，这个缓冲区实际就是一个字节数组，默认大小 8192

### BufferedOutputStream（字节缓冲输出流）

* `BufferedOutputStream` 将数据（字节信息）写入到目的地（通常是文件）的过程中不会一个字节一个字节的写入，而是会先将要写入的字节存放在缓存区，并从内部缓冲区中单独写入字节。这样大幅减少了 IO 次数，提高了效率

### 字符缓冲流

`BufferedReader` （字符缓冲输入流）和 `BufferedWriter`（字符缓冲输出流）类似于 `BufferedInputStream`（字节缓冲输入流）和 `BufferedOutputStream`（字节缓冲输入流），内部都维护了一个字节数组作为缓冲区。不过，前者主要是用来操作字符信息。

## 随机访问流

* 这里的随机访问流指的是支持随意跳转到文件的任意位置进行读写的 `RandomAccessFile`
* 读写模式主要有下面四种：

  * `r` : 只读模式。
  * `rw`: 读写模式
  * `rws`: 相对于 `rw`，`rws` 同步更新对“文件的内容”或“元数据”的修改到外部存储设备。
  * `rwd` : 相对于 `rw`，`rwd` 同步更新对“文件的内容”的修改到外部存储设备。
* 文件内容指的是文件中实际保存的数据，元数据则是用来描述文件属性比如文件的大小信息、创建和修改时间。
* `RandomAccessFile` 中有一个文件指针用来表示下一个将要被写入或者读取的字节所处的位置。我们可以通过 `RandomAccessFile` 的 `seek(long pos)` 方法来设置文件指针的偏移量（距文件开头 `pos` 个字节处）。如果想要获取文件指针当前的位置的话，可以使用 `getFilePointer()` 方法。
* `RandomAccessFile` 的 `write` 方法在写入对象的时候如果对应的位置已经有数据的话，会将其覆盖掉。需要提前设置指针偏移量
* `RandomAccessFile` 比较常见的一个应用就是实现大文件的 **断点续传，它可以合并文件分片**

## IO设计模式

### 装饰器模式

* **装饰器（Decorator）模式** 可以在不改变原有对象的情况下拓展其功能。通过组合替代继承来扩展原始类的功能，在一些继承关系比较复杂的场景（IO 这一场景各种类的继承关系就比较复杂）更加实用。
* 对于字节流来说， `FilterInputStream` （对应输入流）和 `FilterOutputStream`（对应输出流）是装饰器模式的核心，分别用于增强 `InputStream` 和 `OutputStream`子类对象的功能。
* 我们常见的 `BufferedInputStream`(字节缓冲输入流)、`DataInputStream` 等等都是 `FilterInputStream` 的子类，`BufferedOutputStream`（字节缓冲输出流）、`DataOutputStream`等等都是 `FilterOutputStream`的子类。
* 举例：通过 `BufferedInputStream`（字节缓冲输入流）来增强 `FileInputStream` 的功能， `BufferedInputStream` 的构造函数其中的一个参数就是 `InputStream`。

```java
public BufferedInputStream(InputStream in) {
    this(in, DEFAULT_BUFFER_SIZE);
}

public BufferedInputStream(InputStream in, int size) {
    super(in);
    if (size <= 0) {
        throw new IllegalArgumentException("Buffer size <= 0");
    }
    buf = new byte[size];
}
```

### 适配器模式

* 主要用于接口互不兼容的类的协调工作，你可以将其联想到我们日常经常使用的电源适配器。
* IO 流中的字符流和字节流的接口不同，它们之间可以协调工作就是基于适配器模式来做的，更准确点来说是对象适配器。通过适配器，我们可以将字节流对象适配成一个字符流对象，这样我们可以直接通过字节流对象来读取或者写入字符数据。
* `InputStreamReader` 和 `OutputStreamWriter` 就是两个适配器(Adapter)， 同时，它们两个也是字节流和字符流之间的桥梁。`InputStreamReader` 使用 `StreamDecoder` （流解码器）对字节进行解码，**实现字节流到字符流的转换，**`OutputStreamWriter` 使用 `StreamEncoder`（流编码器）对字符进行编码，实现字符流到字节流的转换。

### 二者区别

**装饰器模式** 更侧重于动态地增强原始类的功能，装饰器类需要跟原始类继承相同的抽象类或者实现相同的接口。并且，装饰器模式支持对原始类嵌套使用多个装饰器。

**适配器模式** 更侧重于让接口不兼容而不能交互的类可以一起工作，当我们调用适配器对应的方法时，适配器内部会调用适配者类或者和适配类相关的类的方法，这个过程是透明的。就比如说 `StreamDecoder` （流解码器）和 `StreamEncoder`（流编码器）就是分别基于 `InputStream` 和 `OutputStream` 来获取 `FileChannel`对象并调用对应的 `read` 方法和 `write` 方法进行字节数据的读取和写入。

### 观察者模式

没看懂

## IO模型
