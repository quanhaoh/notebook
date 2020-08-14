## FileOutputStream

### 重要成员变量

```java
// 文件描述符
private final FileDescriptor fd;
// 文件通道，懒加载（NIO中的引用）
private volatile FileChannel channel;
// 文件路径
private final String path;
// 关闭锁，在调用close()过程中使用
private final Object closeLock = new Object();
// 输入流是否关闭的标志
private volatile boolean closed;
```

#### 构造方法

获取文件描述符，将该对象与文件描述符绑定

FileOutputStream(String name)

- 调用this(name, false)

FileOutputStream(String name, boolean append)

- 当append为true时，为追加模式
- 调用this(name != null ? new File(name) : null, append);

FileOutputStream(File file)

- 调用this(file, false)

FileOutputStream(File file, boolean append)

- 当append为true时，为追加模式

```java
/**
 *
 * @param      file               the file to be opened for writing.
 * @param     append      if <code>true</code>, then bytes will be written
 *                   to the end of the file rather than the beginning
 * @exception  FileNotFoundException  if the file exists but is a directory
 *                   rather than a regular file, does not exist but cannot
 *                   be created, or cannot be opened for any other reason
 * @exception  SecurityException  if a security manager exists and its
 *               <code>checkWrite</code> method denies write access
 *               to the file.
 * @see        java.io.File#getPath()
 * @see        java.lang.SecurityException
 * @see        java.lang.SecurityManager#checkWrite(java.lang.String)
 * @since 1.4
 */
public FileOutputStream(File file, boolean append) throws FileNotFoundException{
    // 获取文件路径
    String name = (file != null ? file.getPath() : null);
    // 获取安全管理器，判断是否具备读写文件的权限
    SecurityManager security = System.getSecurityManager();
    if (security != null) {
        security.checkWrite(name);
    }
    // 判断文件是否存在
    if (name == null) {
        throw new NullPointerException();
    }
    if (file.isInvalid()) {
        throw new FileNotFoundException("Invalid file path");
    }
    // 获取文件描述符（无效）
    this.fd = new FileDescriptor();
    fd.attach(this);
    this.path = name;
	// 打开文件，调用的是本地方法open0(name, append)
    open(name, append);
    FileCleanable.register(fd);   // open sets the fd, register the cleanup
}
```

### 常用方法

```java
// 本地方法
private native void write(int b, boolean append) throws IOException;
// 本地方法
private native void writeBytes(byte b[], int off, int len, boolean append) throws IOException;

// 写入一个字节
public void write(int b) throws IOException {
    write(b, fdAccess.getAppend(fd));
}
// 写入字节数组b
write(byte b[]) {
    writeBytes(b, 0, b.length, fdAccess.getAppend(fd));
}
// 写入字节数组b[oof]-b[off + len -1]的数据
write(byte b[], int off, int len) {
    writeBytes(b, off, len, fdAccess.getAppend(fd));
}
```

#### close()、getFD()、getChannel()

```java
/**
 * 关闭输出流，释放相关资源
 * @exception  IOException  if an I/O error occurs.
 *
 * @revised 1.4
 * @spec JSR-51
 */
public void close() throws IOException {
    if (closed) {
        return;
    }
    synchronized (closeLock) {
        if (closed) {
            return;
        }
        closed = true;
    }
	// 关闭文件通道
    FileChannel fc = channel;
    if (fc != null) {
        fc.close();
    }
	// 关闭文件描述符
    fd.closeAll(new Closeable() {
        public void close() throws IOException {
           fd.close();
       }
    });
}

/**
 * 获取文件描述符对象
 * @return  the <code>FileDescriptor</code> object that represents
 *          the connection to the file in the file system being used
 *          by this <code>FileOutputStream</code> object.
 *
 * @exception  IOException  if an I/O error occurs.
 * @see        java.io.FileDescriptor
 */
 public final FileDescriptor getFD()  throws IOException {
    if (fd != null) {
        return fd;
    }
    throw new IOException();
 }

/**
 * 获取文件通道对象
 * @return  the file channel associated with this file output stream
 *
 * @since 1.4
 * @spec JSR-51
 */
public FileChannel getChannel() {
    FileChannel fc = this.channel;
    if (fc == null) {
        synchronized (this) {
            fc = this.channel;
            if (fc == null) {
                this.channel = fc = FileChannelImpl.open(fd, path, false,
                    true, false, this);
                if (closed) {
                    try {
                        // possible race with close(), benign since
                        // FileChannel.close is final and idempotent
                        fc.close();
                    } catch (IOException ioe) {
                        throw new InternalError(ioe); // should not happen
                    }
                }
            }
        }
    }
    return fc;
}
```