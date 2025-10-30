# IO

![1761782305814](image/IO/1761782305814.png)

## 核心抽象类

**InputStream 类**

* `int read()`：读取数据
* `int read(byte b[], int off, int len)`：从第 off 位置开始读，读取 len 长度的字节，然后放入数组 b 中
* `long skip(long n)`：跳过指定个数的字节
* `int available()`：返回可读的字节数
* `void close()`：关闭流，释放资源

**OutputStream 类**

* `void write(int b)`： 写入一个字节，虽然参数是一个 int 类型，但只有低 8 位才会写入，高 24 位会舍弃（这块后面再讲）
* `void write(byte b[], int off, int len)`： 将数组 b 中的从 off 位置开始，长度为 len 的字节写入
* `void flush()`： 强制刷新，将缓冲区的数据写入
* `void close()`：关闭流

**Reader 类**

* `int read()`：读取单个字符
* `int read(char cbuf[], int off, int len)`：从第 off 位置开始读，读取 len 长度的字符，然后放入数组 b 中
* `long skip(long n)`：跳过指定个数的字符
* `int ready()`：是否可以读了
* `void close()`：关闭流，释放资源

**Writer 类**

* `void write(int c)`： 写入一个字符

* `void write( char cbuf[], int off, int len)`： 将数组 cbuf 中的从 off 位置开始，长度为 len 的字符写入
* `void flush()`： 强制刷新，将缓冲区的数据写入
* `void close()`：关闭流

![1761782263886](image/IO/1761782263886.png)
