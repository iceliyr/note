# String字符串

## 理论

### 三个类区别

**String**:

- 不可变性：String 对象是不可变的，一旦创建就不能被修改。任何对 String 对象的修改都会创建一个新的 String 对象。
- 线程安全：String 对象是线程安全的，因为它的不可变性使得多个线程无法同时修改同一个 String 对象。
- 性能：由于不可变性，每次修改 String 对象都会创建新的对象，如果频繁进行字符串操作，会产生大量的临时对象，影响性能。

**StringBuilder**:

- 可变性：StringBuilder 是可变的，它的内容可以被修改，而不会创建新的对象。
- 线程不安全：StringBuilder 不是线程安全的，如果多个线程同时访问一个 StringBuilder 对象，需要手动进行同步处理，否则可能会导致线程安全问题。
- 性能：由于可变性，StringBuilder 适合在单线程环境下进行频繁的字符串操作，因为不需要创建大量的临时对象。

**StringBuffer**:

- 可变性：StringBuffer 也是可变的，与 StringBuilder 类似，它的内容可以被修改，而不会创建新的对象。
- 线程安全：StringBuffer 是线程安全的，它的方法都是同步的，因此可以安全地在多线程环境下使用。
- 性能：由于线程安全的代价，StringBuffer 的性能比 StringBuilder 差一些，在单线程环境下，通常使用 StringBuilder 更高效。

### StringBuilder、StringBuffer的数组扩容

1. **初始容量**：StringBuilder 和 StringBuffer 在创建时都会有一个初始容量，如果没有显式指定，默认容量一般为 16。
2. **容量检查**：每次执行追加操作时，会先检查当前字符串长度是否已经超过了数组的容量。如果超过了容量，就需要进行扩容操作。
3. **计算新容量**：扩容时会计算一个新的容量大小。一般来说，新容量的计算是根据当前容量和需要追加的字符串长度来决定的。一种常见的做法是将当前容量乘以一个固定的扩容因子，比如 2，以获得一个新的容量值。
4. **创建新数组**：根据新的容量值创建一个更大的字符数组。
5. **数据复制**：将原始数组中的数据复制到新数组中。
6. **更新引用**：将字符串对象的内部字符数组引用指向新的数组。

### String 能被继承吗 为什么用 final 修饰 难度系数：

不能被继承，因为 String 类有 final 修饰符，而 final 修饰的类是不能被继承的。
String 类是最常用的类之一，为了效率，禁止被继承和重写。
为了安全。String 类中有 native 关键字修饰的调用系统级别的本地方法，调用了操作系统的 API，如果方法可以重写，可能被植入恶意代码，破坏程序。Java 的安全性也体现在这里。

# Arrays

- 导入：import java.util.Arrays

- 方法

  - toString(arr)    返回一个字符串

  - sort                      排序，默认升序

  - equals                比较两个数组内容是否相等

  - binarySearch

    - int Arrays.binarySearch( Datatype[], Datatype key)

      - int[] a = {1,5,6,7};

      - Arrays.binarySearch(a,2)  //没找到，插入点为1，则返回 -2

      - Arrays.binarySearch(a,4)  //没找到，插入点为1，则返回 -2

      - Arrays,binarySearch(a,8)  //没找到，插入点为4，则返回 -5

      - Arrays.binarySearch(a,5)  //找到了！返回下标 1

      - 只要返回值 ≥ 0 ，就代表找到了。

  - copyOf

    - .![img](https://api2.mubu.com/v3/document_image/82b1dc4b-cd35-452e-a3cf-8adf16a5da3b-18846868.jpg)

    - 会改变拷贝数组的长度

- 实现接口（sort）

  - 接口实现（需要用到类）（Integer)

    - 匿名内部类![img](https://api2.mubu.com/v3/document_image/a8521f10-8086-4bbc-9e65-0c5db3cd3058-18846868.jpg)

    - Lambda![img](https://api2.mubu.com/v3/document_image/2ba3b574-7bae-42d2-a1af-f8dadfffc229-18846868.jpg)

    - 新建类

# 基本类型包装类

## 理论

### 意义

方便调用相应方法

用于java集合

### 装箱/拆箱

装箱：将基本类型转换成包装类对象
拆箱：将包装类对象转换成基本类型的值

原理

javac 编译器的语法糖，底层是通过 Integer.valueOf()和 Integer.intValue()方法实现

### 基本类型与包装类区别



# Properties

- 应用：![img](https://api2.mubu.com/v3/document_image/15f71389-efd2-4e14-9e03-c16ad5dd9a8a-18846868.jpg)

- 读取![img](https://api2.mubu.com/v3/document_image/5278b394-d3b2-41fc-ae05-66430786cc38-18846868.jpg)

- 获取关键字![img](https://api2.mubu.com/v3/document_image/745e6e1b-7d76-4b41-a9e6-2f12ab2ad6d5-18846868.jpg)

- 获取右边值![img](https://api2.mubu.com/v3/document_image/d8ce4222-4380-4a00-9ece-8c6f8cf8a769-18846868.jpg)

- 修改,创建

  

  - 有则改之，无则创建

# 数字类型

### BigDecimal(小数位多）

- 小数点特别大时

- 定义

  - BigDecimal de1=new BigDecimal("1234.12343546787654321345678");

- 方法

  - add

  - substract

  - multiply

  - divide
    - 对于除不尽的数，需要精度

### BigInteger（整数长）

- 需要特别大整数

- 定义：

  - BigInteger in1= new BigInteger("1311564376542764879857");

- 方法：

  - add   (加![img](https://api2.mubu.com/v3/document_image/9f8af484-e1aa-4f55-ab4f-bc417eaf4b60-18846868.jpg)

  - subtract（减

  - multiply（乘

  - divide（除

### Math

- 静态常量

  - Math.E     自然对数

  - Math.PI   圆周率

- 绝对值，最值

  - static int abs(int a)	返回 a 的绝对值

  - static long abs(long a)	返回 a 的绝对值

  - static float abs(float a)	返回 a 的绝对值

  - static double abs(double a)	返回 a 的绝对值

  - static int max(int x,int y)	返回 x 和 y 中的最大值

  - static double max(double x,double y)	返回 x 和 y 中的最大值

  - static long max(long x,long y)	返回 x 和 y 中的最大值

  - static float max(float x,float y)	返回 x 和 y 中的最大值

  - static int min(int x,int y)	返回 x 和 y 中的最小值

  - static long min(long x,long y)	返回 x 和 y 中的最小值

  - static double min(double x,double y)	返回 x 和 y 中的最小值

  - static float min(float x,float y)	返回 x 和 y 中的最小值

- 取整

  - static double ceil(double a)	返回大于或等于 a 的最小整数

  - static double floor(double a)	返回小于或等于 a 的最大整数

  - static double rint(double a)	返回最接近 a 的整数值，如果有两个同样接近的整数，则结果取偶数

  - static int round(float a)	将参数加上 1/2 后返回与参数最近的整数

  - static long round(double a)	将参数加上 1/2 后返回与参数最近的整数，然后强制转换为长整型

- 三角函数

  - static double sin(double a)	返回角的三角正弦值，参数以孤度为单位

  - static double cos(double a)	返回角的三角余弦值，参数以孤度为单位

  - static double asin(double a)	返回一个值的反正弦值，参数域在 [-1,1]，值域在 [-PI/2,PI/2]

  - static double acos(double a)	返回一个值的反余弦值，参数域在 [-1,1]，值域在 [0.0,PI]

  - static double tan(double a)	返回角的三角正切值，参数以弧度为单位

  - static double atan(double a)	返回一个值的反正切值，值域在 [-PI/2,PI/2]

  - static double toDegrees(double angrad)	将用孤度表示的角转换为近似相等的用角度表示的角

  - staticdouble toRadians(double angdeg)	将用角度表示的角转换为近似相等的用弧度表示的角

- 指对数运算

  - static double exp(double a)	返回 e 的 a 次幂

  - static double pow(double a,double b)	返回以 a 为底数，以 b 为指数的幂值

  - static double sqrt(double a)	返回 a 的平方根

  - static double cbrt(double a)	返回 a 的立方根

  - static double log(double a)	返回 a 的自然对数，即 lna 的值

  - static double log10(double a)	返回以 10 为底 a 的对数
