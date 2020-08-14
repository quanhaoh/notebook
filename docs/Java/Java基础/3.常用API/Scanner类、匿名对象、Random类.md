### Scanner 类

位于 java.util 包中

创建 Scanner 对象，输入数据
```
Scanner input = new Scanner(System.in);
String sta = input.next();  // 输入字符串
int num = input.nextInt();  // 输入整数
data_type data = input.nextData_type(); // 输入任意类型数据
```

### 匿名对象
匿名对象只能使用一次，即不能再进行调用

常用于创建只使用一次的对象

### Random 类
用于产生各种类型的随机数

使用方式与 Scanner 类基本相同

```
Random temp = new Random();
int num1 = temp.nextInt();  // 产生政府范围内的随机数
int num2 = temp.nextInt(n); // 产生【0，n)范围内的随机数
int num2 = temp.nextInt(n) + m  // 产生【m，n+m)范围内的随机数
```
