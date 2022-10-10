# Java核心类

> 📌Java的`String`和`char`在内存中总是以Unicode编码表示

## `String`

`String`是一种**引用类型**, 内部是由 `char[]` 构成的. 

```java
String s = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});

```

字符串**不可变**.

### `String`操作

#### 比较

```java
String s1 = "hello";
System.out.println("123".equals(s1));
```

使用 `"str".equals("str_2")` 的方式来比较俩字符串是否相等. 

若使用 `==` 比较俩字符串的话, 是**在比较俩字符串的引用是否相同**. 

`"str".equalsIgnoreCase("str_2")` 忽略字符串大小写来比较

#### 搜索子串

`"str".contains("str_2")` str是否包含str\_2, 即str\_2是否为str的子串

`"str".indexof("str_2")` 搜索子串所在位置

`"str".lastIndexof("str_2")` 搜索最后一次出现的位置

`"str".startWith("str_2")` 判断str是否以str\_2为开头

`"str".endWith("str_2")` 判断str是否以str\_2为结尾

#### 提取子串

`"str".substring(a)` 提取 `str[a:]` 段的子串

`"str".substring(a, b)` 提取 `str[a:b]` 段的子串

#### 去除首尾的空白字符

`"str".trim()` 可以**返回**一个字符串**首尾**不包含 `\t|\r|\n`的字符串. 

`"str".strip()` 可以移除字符串**首尾**的空白字符, 除了 `\t|\r|\n` 外, 还会移除类似中文的空格字符 `\u3000`. 

```java
s1 = "\u3000\t 123 \u3000";
System.out.println(s1.trim());
System.out.println(s1.strip());
System.out.println(s1.stripLeading());
System.out.println(s1.stripTrailing());

```

**输出: 
**`　   123 　`   函数 `trim()` 无法去除 `\u3000` 空白字符
`123`                函数 `strip()` 会去掉字符串**首尾**的空白字符
`123 　`           函数 `stripLeading()` 只去掉字符串**首部**的空白字符
`　   123`        函数 `stripTrailing()` 只去掉字符串**尾部**的空白字符

#### 字符串判空

`isEmpty()`函数用于判断字符串是否为**空串**, `isBlank()`函数判断字符串是否为**空白串**. 

```java
"".isEmpty()        // true, 是空串
"  ".isEmpty()      // false, 不是空串
"".isBlank()        // true: 是空串, 也是空白串
"  \n".isBlank()    // true: 只有空白字符
" Hello ".isBlank() // false: 有内容, 不是空白串
```

#### 子串替换

```java
String s = "aaaB   Bccde";
System.out.println(s.replace('a', 'q'));
System.out.println(s.replaceAll("[\\s]", "q"));
```

**输出: **
`qqqB   Bccde`  函数 `replace('旧字符', '新字符')` 会使用新字符把**所有**的旧字符替代掉. 
`aaaBqqqBccde`  函数 `replaceAll("正则", "新子串")` 可以使用正则表达式来匹配并替换掉**所有**匹配子串. 

#### 分割字符串

```java
String s = "aaaB ! Bcc ! de";
String[] ss = s.split("!");
System.out.println(Arrays.toString(ss));
System.out.println(String.join("-+-", ss));
```

**输出: **
`[aaaB ,  Bcc ,  de]`  函数 `split("正则")` 使用正则来匹配目标子串, 并在**剔除目标子串**的情况下将字符串进行分割. 
`aaaB -+- Bcc -+- de`   函数 `String.join("连接字符串", "目标字符串数组")` 可以使用指定的字符串来连接字符串数组. 

#### 格式化字符串

使用 `String.format("字符串", "内容")` 可以生成指定内容的字符串. 

或者使用`printf` 再加上 `字符串 + 内容` 的方式输出指定内容字符串. 

```java
String s = "Hello %s, your GPA is %.2f. ";
String ss = String.format(s, "Eugene", 4.1);
System.out.println(ss);
System.out.printf(s, "Eugene", 4.1);
```

#### 类型转换

##### 其它类型转 `String` 

```java
String[] arr = {
        String.valueOf(123),
        String.valueOf(true),
        String.valueOf(123.3)
};
System.out.println(Arrays.toString(arr));
```

**输出: **
`[123, true, 123.3]` 函数 `String.valueOf()` 可以将输入的任意基本类型或引用类型转换为字符串. 

##### `String` 转其它类型

```java
String[] arr = {
        String.valueOf(123),
        String.valueOf(true),
        String.valueOf(123.3)
};
System.out.println(Integer.parseInt(arr[0]));
System.out.println(Boolean.parseBoolean(arr[1]));
System.out.println(Float.parseFloat(arr[2]));
```

**输出: **
`123` 

`true` 

`123.3` 

##### 关于 `Integer.getInteger()` 

它不是将字符串转换为 `int`，而是把该字符串**对应的系统变量**转换为 `Integer` 

```java
System.out.println(Integer.getInteger("sun.arch.data.model"));
```

**输出: **
`64` 即当前JVM是64位的. 

## `StringBuilder`

若直接对字符串进行 `+` 操作, 会产生大量的临时字符串, 会浪费内存和影响GC效率. 

`StringBuilder` 是一个可变对象, 能**预分配缓冲区**. 这样在 `StringBuilder` 中新增字符时不会创建新的临时对象. 

### `StringBuilder` 的链式操作

```java
public StringBuilder chainOperation() {
    this.sb.append("Hi").append(", ").append("Eugene").append(". ");
    this.sb.insert(0, "2022年9月28日 ");
    return this.sb;
}
```

**输出: ** 
`2022年9月28日 Hi, Eugene. `

> 📌对普通的字符串 `+` 操作, 不需要特别将其改写为 `StringBuilder` 操作. 编译器会自动地把多个 `+` 操作编码为 `StringConcatFactory` 操作. 在运行时 `StringConcatFactory`操作会自动地把字符串 `+` 操作优化为数组复制或 `StringBuilder` 操作. 

## `StringJoiner`

`StringJoiner` 工具是为了方便使用分隔符拼接数组. 

### 只指定连接符

```java
String[] names = {"Bob", "Alice", "Grace"};
// 指定连接符为 ", "
var sj = new StringJoiner(", ");
for (String name : names) {
    sj.add(name);
}
System.out.println(sj.toString());
```

**输出: **
`Bob, Alice, Grace`

### 指定连接符与开头和结尾

```java
String[] names = {"Bob", "Alice", "Grace"};
// 指定连接符、开头、结尾
var sj = new StringJoiner(", ", "Hello ", "!");
for (String name : names) {
    sj.add(name);
}
System.out.println(sj.toString());
```

**输出: **
`Hello Bob, Alice, Grace!
`

### `String.join()`

`String.join()` 的功能与 `StringJoiner` 很相似. 

**不用指定开头结尾时** `String.join()` 更方便. 

## 包装类型

所有的整数和浮点数的包装类型都继承自`Number`. 

Java的基本类型对应的引用类型叫做 `包装类 Wrapper Class`. 

### 使用包装类型

1.  使用 `new` 操作符创建 `Integer` 实例 (不推荐, 已过时的操作): 

    `Integer n1 = new Integer(i);`

2.  使用静态方法创建: 

    *   通过静态方法 `Integer.valueof(int)` 来创建: 

        `Integer n2 = Integer.valueOf(i);`

    *   通过静态方法 `Integer.valueof(String)` 来创建: 

        `Integer n3 = Integer.valueOf("100");`

在上述的两种方法的区别: 

1.   `方法 1` 始终会创建新的 `Integer` 对象; 

2.   `方法 2` 把内部优化留给 `Integer` 的**实现者**去做. 

### 自动装箱 `Auto Boxing`

```java
Integer n = 100;
int x = n;
```

将 `基本类型` 转为 `引用类型` 的操作叫做 `装箱 (Boxing)`, 反之叫做 `拆箱 (Unboxing)`. 

> 📌自动装箱与自动拆箱只发生在编译阶段. 

### 不变类

> 📌所有的包装类型都是不变类. 

如 `Integer` 类的定义: 

`public final class Integer extends Number implements Comparable<Integer> {}`

一旦创建了 `Integer` 对象, 它就是不变的. 

### 引用类型的比较

引用类型的比较**不能**使用比较符 `==`, 因为这样比较的是引用地址是否相同, 而不是引用值是否相同. 

应该使用 `equals()` 来比较. 

例如: 

1.  例1

    ```java
    Integer x = 127;
    Integer y = 127;
    System.out.println(x == y);
    System.out.println(x.equals(y));
    ```

    **输出: **
    `true` :**特例**. 为了节省内存, `Integer.valueOf()`对于较小的数, 始终返回相同的实例. 所以这里恰好连引用也相同. 
    `true`

2.  例2

    ```java
    Integer x = 1024;
    Integer y = 1024;
    System.out.println(x == y);
    System.out.println(x.equals(y));
    ```

    **输出: **
    `false` : 虽然值一样, 但是这是两个实例. 因此其引用不同. 
    `true`

## `JavaBean`

若一个类的字段读写方法符合以下命名规范, 则这种类叫做 `JavaBean`. 

```java
public DataType getField();
public void setField(DataType value);
```

即 `getter` 和 `setter`. 

其中, `boolean` 类型的读写命名规范为: 

```java
public boolean isField();
public void setField(boolean value);
```

> 📌`JavaBean` 中拥有 `getter` 或者 `setter` 或者兼而有之的字段, 叫做 `属性`. 

### 作用

这是一种**数据封装**的方法. 

`JavaBean` 主要被用于传递数据. 将一组数据封装成 `JavaBean` 后能便于传输. 且 `JavaBean` 能方便地被IDE分析, 生成读写属性的代码. 

### 枚举 `JavaBean` 的属性

Java核心库的 `Introspector`: 

```java
BeanInfo info = Introspector.getBeanInfo(Person.class);
for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
    System.out.println("pd.getName():\t\t\t" + pd.getName());
    System.out.println("pd.getReadMethod():\t\t" + pd.getReadMethod());
    System.out.println("pd.getWriteMethod():\t" + pd.getWriteMethod() + "\n\n");
}
```

其中的例子 `Person` 类的定义为: 

```java
class Person {
    private String name;
    private int age;
    private String useless;
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}
```

即有四个字段 (三个自定义字段 + 一个隐含字段 `class`). 

但是类 `Person` 的四个字段中, `useless` 字段没有 `getter` 或者 `setter`, 那么它就不会被 `内省 Introspector` 给展示出来. 

> 📌也即是说 `Introspector` 只会展示 `JavaBean` 的**属性**. 

在代码 (Java 代码块) 的执行输出为: 

*    **输出: **
    pd.getName():      age
    pd.getReadMethod():    public int eugene.Person.getAge()
    pd.getWriteMethod():  public void eugene.Person.setAge(int)

    pd.getName():      class
    pd.getReadMethod():    public final native java.lang.Class java.lang.Object.getClass()
    pd.getWriteMethod():  null

    pd.getName():      name
    pd.getReadMethod():    public java.lang.String eugene.Person.getName()
    pd.getWriteMethod():  public void eugene.Person.setName(java.lang.String)

## 枚举类

为了避免某个值不在 `枚举` 的集合范围内以及避免不同用途 `枚举` 的混用, 我们可以使用 `enum` 来定义枚举类: 

```java
public enum Weekdays {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

`enum` 常量**本身带有类型信息**, 编译器可以发现不同类型的混用错误: 

```java
int temp = 1;
Weekdays day = Weekdays.FRI;
if (temp == day) {
    // Operator '==' cannot be applied to 'int', 'eugene.Weekdays'
    ...
}
```

编译器会检查出来两种不同类型的值在做比较, 从而报错. 

### `enum` 的比较

虽然 `enum` 是引用类型, 但是 `enum` 的每个常量在 `JVM` 中**只有一个实例**, 因此可以用 `==` 直接比较. 

所以对于 `enum` 类型, `day == Weekdays.FRI` 与 `day.equals(Weekdays.FRI)` 均合法. 

### `enum` 与普通类的区别

`enum` 与普通类**没有任何区别**. 但是 `enum` 类有以下几种特点: 

*   `enum` 无法被继承, 且它继承自 `java.lang.Enum;` 

*   只能定义出 `enum` 实例, 无法使用 `new` 操作符创建 `enum` 实例; 

*   定义的每个实例都是引用类型的唯一实例; 

*   `enum` 可用于 `switch`. 

### `enum` 常用操作

1.  使用 `name()` 返回常量名: 

    ```java
    System.out.println(Weekdays.FRI.name());
    ```

    **输出:** 
    `FRI`

2.  使用 `ordinal()` 返回常量的顺序号: 

    ```java
    System.out.println(Weekdays.FRI.ordinal());
    ```

    **输出: ** 

    `5`: 即 `SUN, MON, TUE, WED, THU, FRI, SAT` 中索引为 `5`. 

3.  带参构造方法: 

    如果枚举中需要包含更多信息的话, 可以为其添加一些字段. 比如下面示例中的 `dayValue`, 此时需要为枚举添加一个**带参的构造方法**, 这样就可以在定义枚举时添加对应的名称了. 

    ```java
    enum Weekdays {
        SUN("周日"), MON("周一"), TUE("周二"), WED("周三"), THU("周四"), FRI("周五"), SAT("周六");
        public final String dayValue;
        Weekdays(String dayValue) { this.dayValue = dayValue; }
    }
    
    // 便可通过访问 dayValue 来访问某一天的名字
    System.out.println(Weekdays.FRI.dayValue);
    
    ```

## 记录类 (TODO)

*   [ ] `Java 14` 才引入的新特性. 暂时不记录

## `BigInteger`

用来大数计算. 

使用 `BigInteger` 进行运算时, **只能使用实例方法**: 

```java
BigInteger i1 = new BigInteger("1234567890");
BigInteger i2 = new BigInteger("12345678901234567890");
// 使用 add() 来进行加法运算
BigInteger sum = i1.add(i2);
```

将 `BigInteger` 类型转为基本类型:

*   转换为 `byte`：`byteValue()` 

*   转换为 `short`：`shortValue()` 

*   转换为 `int`：`intValue()` 

*   转换为 `long`：`longValue()` 

*   转换为 `float`：`floatValue()` 

*   转换为 `double`：`doubleValue()` 

`BigInteger` 转为基本类型, 有可能会丢失精度. 

若在丢失精度时报错, 可以使用`intValueExact()`、`longValueExact()`等方法, 这类方法在丢失精度时会抛出 `ArithmeticException` 异常. 

## `BigDecimal`

类似 `BigInteger`, 这玩意用来表示一个任意精度和任意大小的浮点数. 
