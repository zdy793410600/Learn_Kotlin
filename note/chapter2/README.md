# Kotlin笔记（二） - 数据类型

* 2.1  Boolean数据类型
* 2.2  Number数据类型
* 2.3  拆箱装箱与Char数据类型
* 2.4  基础数据类型转换与字符串
* 2.5  类和对象
* 2.6  空类型和智能类型转换
* 2.7  包（package）
* 2.8  区间（Range）
* 2.9  数组（Array）

## 2.1  Boolean数据类型
> Demo见：BooleanDataType.kt

Boolean 数据类型只有 true 和 false 两种值。
```
val aBoolean:Boolean = true
val otherBoolean:Boolean = false
```

## 2.2  Number数据类型
> Demo见：NumberDataType.kt

Kotlin 的 Number 数据类型对比 Java 的，将拆装箱合为一体，没有具体的区分拆箱和装箱。

![Number数据类型](img_number_type.png)

## 2.3  拆箱装箱与Char数据类型
> Demo见：CharDataType.kt

![Char数据类型 - 转义字符](img_char_type.png)

Char类型：
* 字符对应 Java 的 Character；
* 占两个字节，表示一个16位的 Unicode 字符；
* 字符用单引号 '' 引起来，例如：'a'，'0'，'\n'。

## 2.4  基础数据类型转换与字符串
> Demo见：StringDataType.kt

在 java 中，可以用高精度的基础数据类型去接收低精度的基础数据类型，如 float 声明的变量，可以接收 int 声明的变量，但是在 kotlin 中不支持这种方式。</br>
kotlin 中不支持隐式转换，只能通过以下方式接收：
```
val anInt:Int = 5
val aLong:Long = anInt.toLong()
```

kotlin 中的字符串：
* 字符串就是一串 char；
* kotlin 中的 == 与 java 中的 equals() 是等价的；
* kotlin 如果相比较两个对象是否相等，使用 === 。

kotlin 的字符串模版</br>
在 kotlin 中，拼接两个字符串也可以像 java 一样使用 + 号进行拼接，但是 kotlin 有一种更优雅的写法，就是字符串模板。</br>

kotlin 使用 $ 符进行引用和占位，也可以使用 ${} 进行运算：
```
val arg1: Int = 1
val arg2: Int = 2
println("$arg1+$arg2=${arg1 + arg2}")
```

如果我们想打印 $ 符号，需要对它进行转义：
```
val money: Int = 1000
println("\$$money")
```

在 kotlin 中，如果想打印原始字符串，使用一对三个引号包裹起来的就可以了：
```
val rawString = """\\\\\\$$money
        \n
        \t"""
println(rawString)
```

## 2.5  类和对象
> Demo见：ObjectDataType.kt

### 2.5.1 什么是类？
* 类，一个抽象的概念
* 具有某些特征的事物的概括
* 不特定指代任何一个具体的事物

举例：人、车、书都是类，数、字符串也都是类</br>
写法：class <类名>{<成员>}</br>

### 2.5.2 什么是对象？
* 是一个具体的概念，与类相对应
* 描述某一个具体的个体

举例：某些人、领导的车、你手里的那本《Java 编程思想》

### 2.5.3 类和对象的关系
![类和对象的关系是 1对多](img_one_class_to_objects.png)
* 一个类通常可以有很多个具体的对象
* 一个对象本质上只能从属于一个类
* 类和对象的关系是**一对多**的关系
* 对象也经常被称作 "类的对象" 或者 "类的实例"

举例：
* 城市 - 北京、上海、深圳
* 国家 - 中国、美国、加拿大
* 歌曲 - 双节棍、飞得更高
* 语言 - 汉语、日语、C语言 

### 2.5.4 类的继承
* 默认类是 final 修饰的，如果想要某个类可以被继承，那么需要在 class 前面加上 open 关键字；
* 提取多个类的共性得到一个更抽象的类，即父类；
* 子类拥有父类的一切特征；
* 子类也可以自定义自己的特征；
* 所有类都最终继承自 Any。

## 2.6  空类型和智能类型转换
> Demo见：NullSafe.kt

### 2.6.1 空类型安全
kotlin 中，任意类型都有**可空**、**不可空**两种，不可以定义一个**不可空**的类型为 null：
```kotlin
val notNull:String = null //错误，不能为空
val nullable:String ?= null //正确，可以为空
```

对于不为空的变量，可以直接使用；</br>
可空的变量，不能直接使用：
```kotlin
notNull.length //正确，不为空的值可以直接使用
nullable.length //错误，可能为空，不能直接获取长度
```

如果一定要使用可空变量的方法或属性，可以通过 ?. 或 !!. 两种方式：
```kotlin
nullable!!.lenght //正确，强制认定 nullable 不为空；如果为 nullable 为空，抛出异常：KotlinNullPointerException
nullable?.lenght //正确，若 nullable 为空，则返回空
```

### 2.6.2 智能类型转换
java 中的 instanceof 是用来判断**某个对象**是否是**某个类型**；</br>
在 kotlin 中有同样的判断字段 is，相对于 java，is 字段更加智能；</br>
使用 is 判断类型匹配之后，可以直接调用该类型对应的方法，而不需要进行强制类型转换。
```kotlin
val parent: Parent = Child()
//智能类型转换
if (parent is Child) {
    //因为前面已经判断了 parent 是 Child 类型了，所以不需要进行类型转换就可以调用 Child 的方法了。
    println(parent.logChild())
}
```

在 kotlin 中，对某个对象进行类型转换的时候，可以有两种方式，一种安全的，一种不安全的：
* as 强制转换为对应类型，如果类型转换失败，则抛异常 ClassCastException。
* as? 安全的类型转换，如果类型转换失败，则转换为null。
```kotlin
// as 强制类型转换为Child，如果类型转换失败，则抛异常 ClassCastException。
val parent2 = Parent()
val sub2 = parent2 as Child
sub2.logChild()

// as? 安全的类型转换，如果类型转换失败，则转换为null。
val parent3 = Parent()
val sub3 = parent3 as? Child
println(sub3?.logChild())
```

## 2.7  包（package）
* 包就是命名空间
* 包的声明必须在非注释代码的第一行
* 类的全名 com.clark.learn.kotlin.chapter2.HelloWorld

## 2.8  区间（Range）
> Demo见：RangeExample.kt
* 一个数学上的概念，表示范围
* ClosedRange 的子类，IntRange 最常用
* 基本写法：
```kotlin
0..100 //表示 [0,100]
0 until 100 //表示 [0,100)
i in 0..100 //判断 i 是否在区间 [0,100] 中
0 untils -1 //则是个没有数据的空区间
```

## 2.9  数组（Array）
数组的使用方法
* 基本写法
```
val array:Array<String> = arrayOf(...)
```
* 基本操作
```
print array[i]  //输出第i个成员
array[i] = "Hello"  //给第i个成员赋值
array.length    //数组的长度
```

基本类型的数组</br>
![基本类型的数组](img_basic_type_array.png)
