## 1、kotlin语言中文站地址
[https://www.kotlincn.net/docs/reference/][1]
视频教程地址：[http://coding.imooc.com/learn/list/108.html][2]（笔记来自于此教程）

## 2、如何添加kotterknife?
首先添加butterknife，然后添加kotterknife。

``` gradle
dependencies {
  compile fileTree(dir: 'libs', include: ['*.jar'])
  compile "org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
  compile 'com.jakewharton:butterknife:8.8.1'
  annotationProcessor 'com.jakewharton:butterknife-compiler:8.8.1'
  compile 'com.jakewharton:kotterknife:0.1.0-SNAPSHOT'
}
repositories {
    mavenCentral()
    maven {
        url 'https://oss.sonatype.org/content/repositories/snapshots/'
    }
}
```

## 3、判断类型和类型转换

``` delphi
object is String
object as? String // 安全转换
object as String // 强制转换
```
注意，与java不同，kotlin不支持隐式转换，比如int类型不能直接赋值给long类型，必须使用int.toLong()进行显示转换。

## 4、新建对象不要用new

``` stylus
var msg: StringBuilder = StringBuilder()
```
## 5、对象相等判断
kotlin中==类似于java中的equals，比较的是内容，===类似于java中的==，比较的是对象是否是同一个。举例：

``` javascript
        var a : String = "Hello"
        var b : String = String(charArrayOf('H','e','l','l','o'))
        println(a == b) // true
        println(a === b) // false
```
## 6、区间
0..100 [0,100]
0 util 100 [0,100)
i in 0..100 判断i是都在[0,100]内

## 7、编译期常量
**编译期常量的值在编译期就可以确定的常量。** 也就是说在编译的时候（字节码）就已经为用到这些常量的地方赋好值了，提高代码运行效率。而运行时常量会分配内存空间，在需要使用的时候从内存中获取。
(字节码：虚拟机可识别的汇编语言，Java和kotlin最终都会变成字节码)

在Kotlin中val修饰不可变的值，他**不完全等同于java中的final修饰**,因为java中的final修饰的基本类型和直接申明的string(不是new String())，都是编译期常量，而val必须在前面添加const组成 const val a : String，才是编译期常量。也就是val x = getX()是运行时常量，const val y = 10是编译期常量。

而之所以既有val又有const val是因为，有些不可编辑的常量是运行中才能确认赋值的。

想要在as中查看字节码，shift+ctrl+A，搜索bytecode，kotlin可以使用kotlin bytecode。

## 8、匿名函数

``` kotlin
println(int2Long(3))
val int2Long = fun(x: Int): Long {
    return x.toLong()
}
```
把函数体当做一个变量来使用，必须赋值给变量，否则函数不能没有名字。

## 9、返回和跳转

``` stata
agrs["1","2","3","4","q","m","e"]
lambda中跳过特定循环 out:1 2 3 4 m e
    args.forEach ForEach@ {
        if (it == "q") return@ForEach
        println(it)
    }
lambda中退出循环 out:1 2 3 4
    run Breaking@{
        args.forEach {
            if (it == "q") return@Breaking
            println(it)
        }
    }
```


循环里面正常使用break和continue就可以了，标签不一定需要

## 10、声明类内私有变量的格式
如果不使用kotterknife

``` scala
private var tvResult: TextView？ = null
```


否则

``` kotlin
private val tvResult: TextView by bindView(R.id.tv_result)
```


## 11、类的内部属性
kotlin当中有一个field的幕后字段，主要用于自定义访问器（get和set）时，代表这个属性以前的值。

``` cs
    var name: String? = null
        get() = "hello " + field
        set(value) {
            if (!field.equals("ww")) {
                field = "dd"
            }
        }
```


var name : String = "a" 等同于java中的private String name = "a",kotlin会默认添加get和set方法。如果你希望你的get和set方法不是public,可以在前面修饰private或者protected.对于不能直接确认初始值的变量可以加前缀
lateinit var c: String

如果常量不能直接确认初期值可以（X是一个自定义类型）

``` fsharp
    val e : X by lazy{
        println("init X")
        X()
    }
```


但是，建议在初始化函数中，完成所有值的赋值。

## 12、复写运算符

``` kotlin
class Complex(var real: Double, var imaginary: Double) {
    operator fun plus(other: Complex): Complex {
        return Complex(real + other.real, imaginary + other.imaginary)
    }

    operator fun plus(other: Int): Complex {
        return Complex(real + other, imaginary)
    }

    operator fun plus(other: Any): Int {
        return real.toInt()
    }

    operator fun invoke(): Double {
        return Math.hypot(real, imaginary)
    }
}
```


kotlin同时支持自定义中缀表达式（运算符号在操作对象之间，并且只有一个参数），用infix修饰

``` kotlin
class Pen {
    // 中缀表达式 允许不用.括号这种形式调用方法
    infix fun on(any: Any): Boolean {
        return false
    }
}
class Desk

if (Pen() on Desk()) {// dsl
}
```



## 13、接口代理
kotlin支持使用by关键词进行接口代理。比如，原先我们有一个管理员类，实现了Driver和Writer的接口，那么我们必须实现这两个接口中的函数，但是接口代理帮助我们简化代码，直接把函数的实现交给代理类。

``` kotlin
class SeriorManager(var driver: Driver, var writer: Writer) : Driver by driver, Writer by writer

class CarDriver : Driver {
    override fun driver() {
        println("开车呢")
    }
}

class PPTWriter : Writer {
    override fun writer() {
        println("写PPT呢")
    }
}

interface Driver {
    fun driver()
}

interface Writer {
    fun writer()
}

fun main(args: Array<String>) {
    val carDriver = CarDriver()
    val pptWriter = PPTWriter()
    val seriorManager = SeriorManager(carDriver, pptWriter)
    seriorManager.driver()
    seriorManager.writer()
}
```


## 13.单例模式
kotlin中的单例类请使用object修饰

``` kotlin
object MusicPlayer : Player(){      
    fun stop() {}
}
// java中调用
MusicPlayer.INSTANCE.stop();
// kotlin调用
MusicPlayer.stop()
```


## 14、伴生对象
如果类中想要写静态常量和静态方法，请使用伴生对象

``` kotlin
class Latitude private constructor(val value: Double) {
    // 伴生对象，内容包括类的静态常量和方法
    companion object {
        // 标注后java可使用该静态方法，kotlin调用可写可不写
        @JvmStatic
        fun ofDouble(double: Double): Latitude {
            return Latitude(double)
        }

        fun ofLatitude(latitude: Latitude): Latitude {
            return Latitude(latitude.value)
        }

        // 标注后java可使用该静态常量，kotlin调用可写可不写
        @JvmField
        val TAG: String = "Latitude"
    }
}
```


## 15、数据类
dataclass作为数据类，定义之后自带equals、hashCode、toString、componentN（解构声明相关函数：把一个对象分解成多个变量，使得我们可以val (name,age)= person这样访问person的name和age）、copy等方法。但是，如果想完全替代javabean，他有两个问题需要解决。首先，javabean可继承，而数据类是final类型，并且没有无参构造函数。可以使用noarg（无参）和allopen（final）来解决。以android代码结构为例，需要在application的build.gradle的dependencies中添加

``` nginx
 classpath "org.jetbrains.kotlin:kotlin-noarg:$kotlin_version"
 classpath "org.jetbrains.kotlin:kotlin-allopen:$kotlin_version"
```


然后在app的build.gradle中添加

``` ceylon
apply plugin: 'kotlin-noarg'
apply plugin: 'kotlin-allopen'
noArg{
    annotation("com.fujitsu.cn.fnst.iov.mykotlinapplication.annotation.PoKo")
}


allOpen{
    annotation("com.fujitsu.cn.fnst.iov.mykotlinapplication.annotation.PoKo")
}
```

其中PoKo是一个自定义的类，用来当做标签使用，名字任意

``` kotlin
annotation class PoKo
```


声明完成后，如果有数据类希望能完成javabean的功能，只要在类前注解@PoKo即可

``` delphi
@PoKo
data class Country constructor(var id: Int, var name: String)
```


由于无参构造会在编译时生成，所以在代码中我们无法使用无参构造函数，但是部分框架在反射的时候可以使用无参构造函数。

## 16、内部类
kotlin中默认内部类为静态内部类（不持有外部类,可直接实例化,new Inner()-java,Outter.Inner()-kotlin），如果想转换成非静态内部类（持有外部类对象，可以访问外部类方法和变量，需要外部类对象来实例化(new Outter()).new Inner()-java,Outter().Innner()），需要使用关键词inner。


## 17、密封类和枚举类
密封里子类可数，枚举类实例可数

## 18、高阶函数
传入或者返回函数的函数

``` kotlin
data class Person(val name: String, val age: Int) {
    fun work() {
        println("$name is working")
    }
}

fun main(args: Array<String>) {
    val list = listOf(1, 2, 3, 4, 5, 6)
    // 将数据经过map内的转换添加到一个arrayList中返回
    val newList = list.map {
        it * 2 + 3// 5 7 9 11 13 15
    }

    newList.forEach(::println)
    // 过滤
    println(newList.filter { it % 2 == 1 })

    val list2 = listOf(
            1..9,
            2..5
    )
    // 可以将范围类型的list进行展开
    val flatList = list2.flatMap { it }
    // 1 2 3 4 5 6 7 8 9 2 3 4 5
    flatList.forEach(::println)

    val flatList2 = list2.flatMap { intRange ->
        intRange.map { intElement ->
            "No. $intElement"// NO. 1
        }
    }
    flatList2.forEach(::println)
    // 求和
    println(flatList.reduce { acc, i -> acc + i })

    (0..6).map(::factorial).forEach(::println)

    // 带有5的初期值求和
    println((0..6).map(::factorial).fold(5) { acc, i -> acc + i })
    // 拼接字符串 1，1，2，6，24，120，720，
    println((0..6).map(::factorial).fold(StringBuffer()) { acc, i -> acc.append(i).append("，") })
    // 倒序
    println((0..6).map(::factorial).foldRight(StringBuffer()) { i, acc -> acc.append(i).append("，") })
    // 遇到第一个不符合条件的，留下前面的作为集合返回
    println((0..6).map(::factorial).takeWhile { it % 2 == 1 })

    // 统一判断
    findPerson()?.let { person ->
        person.work()
        person.name
    }

    findPerson()?.apply {
        work()
        name
    }

    val br = BufferedReader(FileReader("hello.txt"))
    with(br){
        var line: String?
        while (true){
            line = readLine()?: break
            println(line)
        }
        close()
    }

    BufferedReader(FileReader("hello.txt")).use {
        var line: String?
        while (true){
            line = it.readLine()?: break
            println(line)
        }
    }
}

/**
 * 阶乘
 */
fun factorial(n: Int): Int {
    if (n == 0) return 1
    return (1..n).reduce { acc, i -> acc * i }
}

fun findPerson(): Person? {
    return Person("ww", 11)
}
```

## 19、尾递归优化
迭代与递归相比效率更高，递归结构清晰。
尾递归：最后一步调用自身没有其他操作，可转换成迭代。
在Kotlin中可以使用tailrec关键字修饰尾递归函数，进行优化，避免stackoverflow

``` kotlin
tailrec fun finListNode(head: ListNode?, value: Int): ListNode? {
    head ?: return null
    if (head.value == value) return head
    return finListNode(head.next, value)
}
```

## 20、闭包
闭包是指，函数的运行环境，持有函数的运行状态，函数内部可以定义函数，也可以定义类。
多次调用makeFun函数，count的值会递增，说明作用域没有得到释放，否则count的输出值会保持不变。

``` kotlin
fun makeFun(): () -> Unit {
    var count = 0
    return fun() {
        println(++count)
    }
}
```

## 21、复合函数、科理化、偏函数（了解）
复合函数：andThen compose

``` kotlin
val add5 = {i: Int -> i + 5} // g(x)
val multiplyBy2 = {i : Int -> i * 2} // f(x)

fun main(args: Array<String>) {
    println(multiplyBy2(add5(8))) // (5 + 8) * 2

    val add5AndMultiplyBy2 = add5 andThen multiplyBy2
    val add5ComposeMultiplyBy2 = add5 compose  multiplyBy2
    println(add5AndMultiplyBy2(8)) // m(x) = f(g(x))
    println(add5ComposeMultiplyBy2(8)) // m(x) = g(f(x))
}
// infix 中缀标记 成员方法或者扩展方法,只有一个参数使用infix关键词描述
// P1 P2 为参数，R为返回值，Function1是只有一个参数的方法
// 传入P1：8，返回P2：add5(8)，然后将P2作为参数交给function，R：multiplyBy2(P2)作为最后的结果返回
// 这里的function->multiplyBy2，this->add5
infix fun <P1, P2, R> Function1<P1, P2>.andThen(function: Function1<P2, R>): Function1<P1,R>{
    return fun(p1: P1): R{
        return function.invoke(this.invoke(p1))
    }
}
// compose计算顺序和andThen相反，先计算后面的，在计算前面的
// 把P1:8作为参数传给function，返回P2：multiplyBy2(8)，将P2作为参数传给对象，返回R：add5(P2)
// 这里的function->multiplyBy2，this->add5
infix fun <P1,P2, R> Function1<P2, R>.compose(function: Function1<P1, P2>): Function1<P1, R>{
    return fun(p1: P1): R{
        return this.invoke(function.invoke(p1))
    }
}
```


科理化：多元函数变成一元函数链

``` kotlin
fun log(tag: String, target: OutputStream, message: Any?){
    target.write("[$tag] $message\n".toByteArray())
}

fun log(tag: String)
    = fun(target: OutputStream)
    = fun(message: Any?)
    = target.write("[$tag] $message\n".toByteArray())

fun main(args: Array<String>) {
    log("benny", System.out, "HelloWorld")
    ::log.curried()("benny")(System.out)("HelloWorld Again.")
}

fun <P1, P2, P3, R> Function3<P1, P2, P3, R>.curried()
    = fun(p1: P1) = fun(p2: P2) = fun(p3: P3) = this(p1, p2, p3)
```

偏函数：在科理化的基础上传入部分参数返回新函数

## 22、Android项目中的代码简化
**如果只是要省略findViewById，kotlin支持插件kotlin-android-extensions**
在build.gradle中添加``` apply plugin: 'kotlin-android-extensions' ``` 
假如有以下button
``` xml
    <Button
        android:id="@+id/btn_click"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"/>
``` 
activity中可以直接使用
``` java
btn_click.setOnClickListener(View.OnClickListener {
            textView.text = "${System.currentTimeMillis()}"
        })
```
**响应事件也可以简化，在build.gradle中添加``` compile 'org.jetbrains.anko:anko-sdk15:0.9.1' ```** 
``` java
btn_click.onClick {
        textView.text = "${System.currentTimeMillis()}"
}
```
或者**启动其他activity**
``` java
// 简化前
var intent = Intent(this@Main2Activity, Main3Activity::class.java)
intent.putExtra("key", "from main2")
startActivity(intent)

// 简化后
startActivity<Main3Activity>("key" to "from main2")
```
Main3Activity接收参数
``` java
textView2.text = intent.extras["key"]?.toString() ?: ""
```

## 23、运算符重载
重载invoke可以将()转换成特殊的扩展方法，该方法使用场景如下。

``` kotlin
open class Tag(val name: String) : Node {
 val properties = HashMap<String, String>()

    // string的拓展方法
    // 括号被转换为带有正确参数的 invoke 参数   
    operator fun String.invoke(value: String) {
        properties[this] = value
    }
}

fun html(block: Tag.() -> Unit): Tag {
    return Tag("html").apply(block)
}

fun main(args: Array<String>) {
    html {
        "id"("htmlId")
       ......
    }.render().apply(::println)
}
```

文档：http://www.kotlindoc.cn/Other/Opetator-overloading.html
补充：http://blog.csdn.net/love667767/article/details/72589260

  [1]: https://www.kotlincn.net/docs/reference/
  [2]: http://coding.imooc.com/learn/list/108.html
 