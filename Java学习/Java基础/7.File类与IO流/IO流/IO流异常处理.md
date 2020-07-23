### try...catch..finally

较为常见的IO流异常处理机制，通常在finally中释放资源，调用close方法

问题在于，这种处理机制较为繁琐

### JDK7和JDK9新增的try-with-resources机制

#### JDK7

在try后的括号中创建IO流对象，在编译器会自动生成响应的处理逻辑，用以释放回收系统资源

```Java
try(FileInputStrean input = new FileInuputStream(...);
	FileOutputStrean output = new FileOutputStrean(...)){
	...
}catch(){
	...
}
```

#### JDK9

在try语句前创建IO流对象，在编译器也会自动生成响应的处理逻辑，用以释放回收系统资源

```Java
FileInputStrean input = new FileInuputStream(...);
FileOutputStrean output = new FileOutputStrean(...);
try(input;output){
	...
}catch(){
	...
}
```

