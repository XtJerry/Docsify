### 一、学习内容
1、输出Hello,World!
2、kotlin(kt/KT)基本类型
3、kt控制流

### 二、输出HelloWorld
``` kotlin
//包名 与java一样
package day1

// kt
// fun 函数名(变量名: 变量类型){}

// go
// func 函数名(变量名 变量类型){}

// java
// function 函数名(变量类型 变量名){}

fun main(args: Array<String>) {
    print("Hello,World!") //打印输出
}
```

### 三、基本类型
1、常量
*常量：定义完成后值不可修改，地址类型是地址不可修改，但地址指像的变量可以修改(暂时先记下)*
``` kotlin
    // 基础数据类型Byte、Short、Int、Long、Float、Double
    /*
	Type					Bit width
	Double					64
	Float					 32
	Long				      64
	Int					   32
	Short					 16
	Byte					  8
    */

    //常量表示格式
    123         //十进制Int
    123L        //十进制Long
    0xff        //十六进制Int
    0b00000001  //二进制Int
    //不支持八进制
    123.0       //Double
    123.0f      //Float

    //常量定义
    //① val 常量名称:常量类型=值
    //② val 常量名称=值
    val byte = 0b1000_0001
    val short = 65535
```
2、变量定义
*与常量相反，定义后值可以被修改*
``` kotlin
    //变量定义,与常量相似，但使用var
    //① var 常量名称:常量类型=值
    //② var 常量名称=值
    var varIntA: Int = 10
    var varIntB = 10
```
3、表达式
```kotlin
    //表达式
    val intA: Int = 10000
    val boxedIntA: Int? = intA
    val boxedIntB: Int? = intA
    println(boxedIntA == boxedIntB)//值比较,true
    println(boxedIntA === boxedIntB)//地址比较,false
```
4、数据转换
```kotlin
    //显示转换,Kotlin隐式转换只能用在可以推到出类型的情况下使用(自动将小类型转换为大类型)
    val intB: Int = 10000
    //var LongB: Long = intB    //不能通过编译
    val longB: Long = intB.toLong()
    var longC: Long = 1L + 10   //隐式转换(自动将10转为Long)

    /*
     每个数字类型支持如下的转换:
     .toByte(): Byte
     .toShort(): Short
     .toInt(): Int
     .toLong(): Long
     .toFloat(): Float
     .toDouble(): Double
     .toChar(): Char
    */
```
5、位运算
```kotlin
    //位运算
    val x = (1 shl 2) or 0x1    //1<<2=4+1=5
    println("x=$x")

    /*
    这是完整的位运算列表(只用于 Int 和 Long ):
    shl(bits) //有符号左移 (Java 的 << )
    shr(bits) //有符号右移 (Java 的 >> )
    ushr(bits)//无符号右移 (Java 的 >>> )
    and(bits) //位与
    or(bits)  //位或
    xor(bits) //位异或
    inv()     //位非
    */
```
6、Char
```kotlin
    //字符Char(符字面值用单引号括起来: '1' 。 特殊字符可以用反斜杠转义。 支持这几个转义序 列:\t 、 \b 、\n 、\r 、\' 、\" 、\\ 和 \$ 。编码其他字符要用Unicode转义序 列语法: '\uFF00'。)
    val charA: Char = '\t'
    val charB: Char = 'A'
```
7、Boolean
```kotlin
    //布尔Boolean(运算&& || !，与或非)
    val booleanA: Boolean = true
    val booleanB: Boolean = false
```
8、Array
```kotlin
    //数组(Array)
    val array = Array(5) { i -> (i * i).toString() }
    //生类型数组(ByteArray,ShortArray,IntArray,不继承Array)
    val arrayInt: IntArray = intArrayOf(1, 2, 3)
```
9、String
```kotlin
    //字符串String
    val stringA: String = "Hello"
    val stringB = "Hello"
    val stringC = """ stringC = "Hello" """ //""" """表示内部内容原样输出类似(可换行)``` ```
    println("stringC=$stringC")// stringC = "Hello"
```
10、字符串模板
```kotlin
    //字符串模板
    val stringName = "Xt"
    var stringHello = "Hello,$stringName."//类似java String.format("Hello,%s",stringName)
    println(stringHello)    //Hello,Xt.
```
### 四、控制流
1、If
``` kotlin
    //If
    //使用方式一(和java一样)
    var a = 0
    var gender = "Male"
    if (a == 0) {
        gender = "Male"
    } else {
        gender = "Female"
    }
    //使用方式二(与三元运算符比较类似 ? :)
    gender = if (a == 0) "Male" else "Female"

    //使用方式三(相当于对三元运算符的拓展)
    gender = if (a == 0) {
        println("Male")
        "Male"
    } else {
        println("Female")
        "Female"
    }

    //方式四(多个else if)
    if (a == 0) {
        gender = "Male"
    } else if (a == 1) {
        gender = "Female"
    } else {
        gender = "Unknown"
    }
```
2、When
``` kotlin
    //When
    //使用方式一(和java switch类似)
    var b = 0
    when (b) {
        0 -> print("Male")
        1 -> print("Female")
        else -> print("Unknown")
    }

    //使用方式二(和java switch类似(去掉break;))
    when (b) {
        0, 1 -> print("Normal")
        else -> print("Unknown")
    }

    //使用方式三(对java switch进行拓展，可以是函数)
    when (b) {
        femaleValue(b) -> print("Female")
        else -> print("Male")
    }

    //使用方式四(对java switch进行拓展,在方式三基础上,in包含,!in不包含,in是一个区间[],还有is,!is)
    when (b) {
        in 1..10 -> print("Female")
        !in 11..20 -> print("Unknown")
        else -> "Male"
    }
    // when和if在大多数情况下是可以互换的,when也是可以给变量赋值的

```
3、For
``` kotlin
    //for
    //使用方式一(和java for(int i=0,i<=2,i++)类似)
    for (item in 0..2) {
        println("for item = $item")
    }

    //使用方式二(和java for(int i=0,i<len(array);i++)类似)
    val array = arrayOf("A", "B", "C")
    for (i in array.indices) {
        println("array[$i] = ${array[i]}")
    }

    //使用方式三(和java for(int : item array) 类似)
    for ((i, item) in array.withIndex()) {
        println("array[$i] = $item")
    }
```
4、While/Do...while
``` kotlin
    //while,do while
    //方式一(与java一样,执行0次到n次)
    var wx = 0
    while (wx < 2) {
        println("wx=$wx")
        wx++
    }

    //方式二(与java一样，执行1次到n次)
    do {
        println("wx=$wx")
        wx++
    } while (wx < 2)
```
5、Break和Continue
``` kotlin
    //使用方式一和java类似
    //break     中断循环，不在判断条件
    //continue  中断本次循环，继续条件判断
    for (i in 0..10) {
        if (i == 0) {
            println("continue")
            continue
        } else {
            println("break")
            break
        }
    }
    //使用方式二(对java多循环嵌套不能直接跳出的一个拓展)
    //在 Kotlin 中任何表达式都可以用标签(label)来标记。 标签的格式为标识符后跟 @ 符号，例如: male@要为一个表达式加标签，我们只要在其前加标签即可。
    male@ for (i in 0..10) {
        for (j in 0..10) {
            if (j == 1) {
                println("中断male@标签的循环")
                break@male
            }
        }
        println("我不会执行！")
    }
	
```



