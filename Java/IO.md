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

* 用于将数据（字节信息）写入到目的地（通常是文件），`java.io.OutputStream`抽象类是所有字节输出流的父类
* 常用方法

  * `write(int b)`：将特定字节写入输出流。
  * `write(byte b[ ])` : 将数组 `b` 写入到输出流，等价于 `write(b, 0, b.length)` 。
  * `write(byte[] b, int off, int len)` : 在 `write(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
  * `flush()`：刷新此输出流并强制写出所有缓冲的输出字节。
  * `close()`：关闭输出流释放相关的系统资源。

## 字符流
