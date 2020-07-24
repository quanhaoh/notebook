## OutputStream

| 方法                                   | OutputStream | FileOutputStream | ByteArrayOutputStream | BufferedOutputStream |
| -------------------------------------- | ------------ | ---------------- | --------------------- | -------------------- |
| abstract void write(int b)             |              |                  |                       |                      |
| void write(byte[] b)                   |              |                  |                       |                      |
| void write(byte[] b, int off, int len) |              |                  |                       |                      |
| void flush()                           | 不实现       |                  |                       |                      |
| void close()                           | 不实现       |                  |                       |                      |

### 常用方法

#### 三种write方法

```java
/**
 * 将一个字节写入流中，只取b的后8位；抽象方法，由子类具体实现
 * @param      b   the <code>byte</code>.
 * @exception  IOException  if an I/O error occurs. In particular,
 *             an <code>IOException</code> may be thrown if the
 *             output stream has been closed.
 */
public abstract void write(int b) throws IOException;

/**
 * 将字节数组b写入流中，直接调用另一个write方法
 * @param      b   the data.
 * @exception  IOException  if an I/O error occurs.
 * @see        java.io.OutputStream#write(byte[], int, int)
 */
public void write(byte b[]) throws IOException {
    write(b, 0, b.length);
}

/**
 * 将字节数组b[off]-b[off + len - 1]数据写入流中
 * @param      b     the data.
 * @param      off   the start offset in the data.
 * @param      len   the number of bytes to write.
 * @exception  IOException  if an I/O error occurs. In particular,
 *             an <code>IOException</code> is thrown if the output
 *             stream is closed.
 */
public void write(byte b[], int off, int len) throws IOException {
    Objects.checkFromIndexSize(off, len, b.length);
    // len == 0 condition implicitly handled by loop bounds
    for (int i = 0 ; i < len ; i++) {
        write(b[off + i]);
    }
}
```

