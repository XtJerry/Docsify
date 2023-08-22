### 一、学习内容
1、接口
2、包
3、类
4、对象
### 二、接口
1、接口声明
``` kotlin
// interface 接口名称{}
interface MyInterface{
}
```
2、接口实现
``` kotlin
// class 类名称:接口名称{}
class MyClass : MyInterface {
}
```
3、可见性修饰符(private、protected、internal和public)
*类、对象、接口、构造函数、方法、属性和它们的setter都可以有可见性修饰符*
``` kotlin
open class UserBean {
    // private 仅对象内可见(和java相同)
    private val varA: Int = 1
    // protected 子类可见(和java相同)
    protected val varB: Int = 1
    // internal 模块内相同(和java相同,但这是java默认)
    internal val varC: Int = 1
    // public 随处可见default(和java相同，但这不是java默认)
    public val varD: Int = 1

    open fun login() {
        println("$varA,$varB,$varC,$varD")
    }
}

// 演示修饰符protected
class ManagerBean : UserBean() {

    override fun login() {
        // 由于varA是私有变量，即使子类也是不能访问
        println("$varB,$varC,$varD")
    }
}

// 演示修饰符internal
class LoginTest {
    val login = UserBean()
    fun test() {
        // 本处默认外部访问,varB由于是protected故不能被直接使用,如果不在此包下,varC也是不能访问的
        println("${login.varC},${login.varD}")
    }
}
```
4、接口冲突及解决机制
``` kotlin
interface InterfaceA {
    fun test() {
        print("InterfaceA.test()")
    }

    fun testPrint() {
        print("InterfaceA")
    }
}

interface InterfaceB {
    fun test()
    fun testPrint() {
        print("InterfaceB")
    }
}

// 与Java接口很类似，但是实现方法与Java抽象类有所差异(Java只能继承一个抽象类，因此kotlin接口实现方法的冲突调用有所不同)
class ClassA : InterfaceA, InterfaceB {
    // 多接口实现同一个接口时，如果有一个接口未实现，也需实现
    override fun test() {
        //super.test()
    }

    override fun testPrint() {
        // 调用接口InterfaceA的testPrint()方法
        super<InterfaceA>.testPrint()
        super<InterfaceB>.testPrint()
    }
}
```
5、接口使用示例
``` kotlin
// 用户登录接口
interface LoginInterface {
    // 接口中的属性定义
    val method: String
        get() = "/login"

    // 接口
    fun login(account: String, password: String)

    // 接口实现(类似java抽象类，但也有不同,不能做状态保存)
    fun result(code: Int, result: String) {
        println(result)
    }
}


// 接口实现(与java类似,java使用implements,kotlin使用:)
// 实现类名称:接口名称
class UserLogin : LoginInterface {

    // 实现接口(与java类似,java使用注解@Override,kotlin直接使用标识符override)
    override fun login(account: String, password: String) {
        if (account == "test" && password == "test") {
            result(200, "登录成功!")
        } else {
            result(201, "登录失败!")
        }
    }

    override fun result(code: Int, result: String) {
        super.result(code, result)
    }

}
```

### 三、包
kotlin所有接口、类都必须有包，可以理解为模块，一个完整的类路径是包名+类名，接口也是如此。
``` kotlin
// 格式
// package 包名
package day2
class TestClass{

}

// 完整的类路径为，非当前包引用都需要加包名，kotlin文件头会列出类名所在包
// day2.TestClass
```

### 四、类
类是接口的实现
1、类的声明
``` kotlin
// 声明格式
// class 类名{}
class Http {
    // 这是一个空类
}
```
2、构造函数
``` kotlin
// 构造函数
class HttpResult {
    var state: Int = 200
    var message: String? = null
    var desc: String? = null
    var data: Object? = null

    // 构造函数(和java类似,java是类名;kotlin是关键字constructor)
    // constructor(字段:类型,...)
    constructor(state: Int) {
        this.state = state
    }
}
```
3、数据类
``` kotlin
// 数据类：只保存数据的类，不做其他业务，如网络请求
// 数据类必须满足要求:
// 1.主构造函数需要至少有一个参数;
// 2.主构造函数的所有参数需要标记为val或var;
// 3.数据类不能是抽象、开放、密封或者内部的;
data class LoginReqBean(val account: String, val password: String)
```
4、嵌套类
``` kotlin
// 类是可以被嵌套的
class ClassUser {
    class UserInfo {

    }
}
```

5、内部类
``` kotlin
// 类可以标记为inner以便能够访问外部类的成员。内部类会带有一个对外部类的对象的引用
class ClassAdmin {
    inner class UserInfo {
        fun print() {
            println("UserInfo")
        }
    }
}

fun testClassAdmin() {
    // 内部内访问
    ClassAdmin().UserInfo().print()
}
```

6、枚举类
``` kotlin
// 枚举类声明：
// enum class 类名
enum class WEEKS {
    MONDAY, TUESDAY, WEDNESDAY
}
```
7、匿名内部类
*一般回调时使用比较多*

8、密封类
``` kotlin
// TODO 后续深入
```

9、类拓展
``` kotlin
// TODO 后续深入
```

10、泛型
``` kotlin
// TODO 后续深入
```

### 五、对象
1、对象声明
``` kotlin
// 格式：
// 对象名称()
fun main() {
    TestBean()
}

// 测试类
class TestBean {
    fun print() {
        println("TestBean")
    }
}
```
2、对象使用
``` kotlin
    val testBean = TestBean()
    testBean.name = "Join"//简单赋值
    testBean.print()//方法调用
```