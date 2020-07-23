### Object类

Object类是所有类的父类，所有类都可以使用Object类的方法

#### 1、toString()方法
默认直接打印对象的地址值，所以一般都需要重写，从而打印类的属性

#### 2、equals()方法
默认比较对象的地址值，返回boolean值，可重写来做值得比较

Object类得equals()方法容易抛出空指针异常，即方法参数不能是null

**Objects**类equals(Object1, Object2)可避免空指针异

### Date类
表示日期和时间，精确到毫秒，时间/日期于毫秒间可以相互转换，以进行计算时间/日期

**0毫秒**：1970年1月1日00：00：00（英国格林威治时间）

把日期转换为毫秒、把毫秒转换为日期

#### 1、Date的构造方法和成员方法
无参构造方法：获取当前系统的日期和时间
```
Date date = new Date();
```
有参构造方法：传递毫秒值，将毫秒值转换为时间和日期
```
Date date = new Date(0);    // 返回0毫秒所转换的时间和日期
```
成员方法：将对象所至的时间和日期转换为毫秒值并返回
```
Date date = new Date();   
long time = date.getTime();
```

#### 2、DateFormat类和SimpleDateFormat类
将日期/时间格式化子类的抽象类：由日期/时间格式化为文本，将文本解析为日期/时间

成员方法
- parse
- format

## Calendar抽象类

是一个抽象类，提供了很多操作日历字段的方法  
包含一个静态方法getIntance()，返回一个子类的对象，即多态，从而获得当前系统的日历

```
Calendar claendar = Calendar.getInstance();
```

#### 1、public int get(int field)

返回给定日历字段（YEAR、MONTH等）的值

#### 2、public void set(int field, int value)
设置日历的字段值

#### 3、public abstract void add(int field, int amount)

对日历字段值作加减

#### 4、public Date getTime()
将Calendar对象转换为Date对象
