## InputStream

| 方法                                 | InputStream                                                  | FileInputStream | ByteArrayInputStream | BufferedInputStream | DataInputStream |
| ------------------------------------ | ------------------------------------------------------------ | --------------- | -------------------- | ------------------- | --------------- |
| abstract int read()                  | 留给子类自定义                                               |                 |                      |                     |                 |
| int read(btye[] b)                   | 源码：调用read(byte[] b, int off, int len)                   |                 |                      |                     |                 |
| int read(byte[] b, int off, int len) | 循环读取字节<br />返回结果：读取的字节数（0、-1、len、小于len） |                 |                      |                     |                 |
| int skip(long n)                     | 利用内部buffer缓存读取字节流，相当于跳过<br />返回结果：跳过的字节数（0、n、小于n） |                 |                      |                     |                 |
| int available()                      | 直接返回0，留给子类自定义                                    |                 |                      |                     |                 |
| synchronized void mark()             | 留给子类自定义                                               |                 |                      |                     |                 |
| synchronized void reset()            | 留给子类自定义                                               |                 |                      |                     |                 |
| boolean markSupported()              | 留个子类自定义                                               |                 |                      |                     |                 |
| void closed()                        | 空                                                           |                 |                      |                     |                 |

```java
/**
     * @param      b     the buffer into which the data is read.
     * @param      off   the start offset in array <code>b</code>
     *                   at which the data is written.
     * @param      len   the maximum number of bytes to read.
     * @return     the total number of bytes read into the buffer, or
     *             <code>-1</code> if there is no more data because the end of
     *             the stream has been reached.
     * @exception  IOException If the first byte cannot be read for any reason
     * other than end of file, or if the input stream has been closed, or if
     * some other I/O error occurs.
     * @exception  NullPointerException If <code>b</code> is <code>null</code>.
     * @exception  IndexOutOfBoundsException If <code>off</code> is negative,
     * <code>len</code> is negative, or <code>len</code> is greater than
     * <code>b.length - off</code>
     * @see        java.io.InputStream#read()
     */
public int read(byte b[], int off, int len) throws IOException {
    // 参数检查，排除off、len小于0的情况，排除len大于b.length - off的情况
    Objects.checkFromIndexSize(off, len, b.length);
    if (len == 0) {
        return 0;
    }

    // 读取一个字节，判断文件是否为空
    int c = read();
    if (c == -1) {
        return -1;
    }
    b[off] = (byte)c;
    // 循环读取长度为len - 1的字节数据
    int i = 1;
    try {
        for (; i < len ; i++) {
            c = read();
            if (c == -1) {
                break;
            }
            b[off + i] = (byte)c;
        }
    } catch (IOException ee) {
    }
    return i;
}
```

```java
/**
 * @param      n   the number of bytes to be skipped.
 * @return     the actual number of bytes skipped which might be zero.
 * @throws     IOException  if an I/O error occurs.
 */
public long skip(long n) throws IOException {
    long remaining = n;
    int nr;

    if (n <= 0) {
        return 0;
    }
    // 利用buffer读取所要跳过字节数，效果相当于跳过
    int size = (int)Math.min(MAX_SKIP_BUFFER_SIZE, remaining);
    byte[] skipBuffer = new byte[size];
    while (remaining > 0) {
        nr = read(skipBuffer, 0, (int)Math.min(size, remaining));
        // 如果nr小于0，则已经读到文件末尾
        if (nr < 0) {
            break;
        }
        remaining -= nr;
    }
    // 此时remaining为未跳过的字节数，返回结果为已跳过的字节数
    return n - remaining;
}
```

