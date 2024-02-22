## TypeScript是什么?

TypeScript是JavaScript的超集合，也可以是说是JavaScript的拓展。支持 ECMAScript6(ES6)标准。

## 开发环境

1. 仅仅是为了学习TypeScript，因此直接使用浏览器编辑和运行[www.typescriptlang.org](https://www.typescriptlang.org/play)。
2. 其他本地安装方式(后续完善,如：Idea、VsCode...)

## 基础语法

TypeScript由以下部分组成

+ 模块
+ 函数
+ 变量
+ 表达式
+ 注释

HelloWorld.ts

```typescript
const hello: string = "Hello World"
console.log(hello)
```

执行运行时会将ts文件内容转为js，最后由程序执行(可以是浏览器，也可以是其他可以运行js的程序)

```shell
//运行输出结果
[LOG]: "Hello World"
```

## 基础类型

如果对下面类型写法不理解，请先看变量声明`let 变量名称:类型=值`。

+ any 任意

```typescript
let a1: any = 13 //声明任务类型，并赋值number类型13
al = "hello" //由于是任意类型，所以修改时可以赋值为string类型hello
```

+ number 数字，双精度64位浮点值。它可以用来表示整数和分数。

```typescript
//下面声明的number值都为10
let b1: number = 0b1010 //二进制,0b开始
let o1: number = 0o12 //八进制,0o开始
let d1: number = 10 //十进制
let h1: number = 0xa; //16进制,0x开始，末尾`;`号是结束标识，可以不用写，写了也会不会错
//浮点型
let f1: number = 1.2
```

+ string 字符串

```typescript
// 字符串可以用'',"",``包含起来，表示字符串，也可以摘``中嵌套${表达式}
let hello: string = 'Hello'
let world: string = "world"
let helloWorld = `${hello},${world}`

```

+ boolean 逻辑

```typescript
// 最简单的类型，值仅有true或false
let flag: boolean = true
```

下面的类型直接作为基本类型有点不恰当，其他语言未将其列为基本类型，后续学完后再确定是否删除该语句。

+ 数组类型类型[]

```typescript
//数字数组
let arr: number[] = [1, 3]
//数组泛型
let arr2: Array<number> = [1, 2]
```

+ 元组[类型,类型n]，很有意思的类型

```typescript
// []中值类型必须与前面声明的类型相同，否则报错
let x: [string, number] = ["Rock", 12]
```

+ enum 枚举类型

```typescript
enum Color {Red, Green, Blue}

let c: Color = Color.Red
console.log(c)//将输出0,输出值默认为从0开始的编号
```

+ void 无返回类型

```typescript
function printTag(): void {
    console.log("Tag")
}
```

+ null 表示对象缺失
+ undefined 表示声明了变量，未赋值
+ never 不会出现的值(是其他类型的子类？)

## 变量声明

+ 什么是变量？
  TypeScript中有常量和变量，常量是一个固定的值，创建后不可更改。变量简单理解就是可以改变的值。
+ 变量命名规范
    + 变量名可以使用字母、数字、下划线_,美元符$(很特别)，不能以数字开头(但可以用$_字母开头)。
+ 声明变量

  __var 变量名:[类型] = [值]__
  <br/>示例:
  ```typescript
    var _name:string="Rock" //声明&赋值，不建议使用name,DOM中全局windows对象下有name会重名。
    var _age:number //声明
    _age=20 //赋值
    
    // 类型推断
    var _address="China" //系统推断string
  ```
+ 变量作用域
  变量作用域是变量声明后可以在什么范围内使用或有效，包含(全局、类和局部作用域)。
  ```typescript
  var global_value="global" //全局变量
  class User{
      age=13 //类变量
      static group_name="Group 1" //静态变量
      printTime():void{
          var time="2024-02-21" //局部变量
          console.log(global) //全局变量
          console.log(User.global)//静态变量
          console.log(this.global)//类变量，在本类使用，请使用this.类变量名
      }
  }
  //使用
  var user:User=new User()
  console.log(global_value) //全局变量可以随便使用
  console.log(User.group_name) //静态变量类名.变量名
  console.log(user.age)//类变量需要创建对象，再通过对象变量名.类变量
  user.printTime()
  ```
+ var与let区别
  在TypeScript中，都可以使用let和var声明变量，但他们有相同和不同。
    1. 重复声明，let作用域内不能重复声明,var作用域内可以重复声明。
       ```typescript
       //情况1
       var var0=0
       var var0=0
       //情况2
       let let0=0
       //let let0=0//出错
       //情况3
       let let1=0
       //var let1=0//出错
       //情况4
       var var1=0
       //let var1=0//出错
       ```
    1. 作用域，let作用域是块，var作用域是函数
       ```typescript
       function testLet(){
           let a=0
           if(true){
               let a=1
               console.log(`a = ${a} in if`)//输出:1
           }
           console.log(`a = ${a}`) //输出:0
       }
  
       function testVar(){
           var a=0
           if(true){
               var a=1
               console.log(`a = ${a} in if`)//输出:1
           }
           console.log(`a = ${a}`)//输出:1
       }
       testLet()
       testVar()  
       ```
    1. 变量提升，let声明变量不会发生提升，var声明变量存在提升现象
       ```typescript
        //待实现
       ```
    1. 全局对象属性
       ```typescript
       //待实现
       ```

## 运算符

TypeScript中包含算术、逻辑、关系、位、赋值、三元、字符和类型运算符，虽然看着很多，该分类是根据不同使用场景划分的，每个场景使用到的运算符很少。
下面分别对其说明

+ 算术运算符
    + \+ 加法
    + \- 减法
    + \* 乘法
    + \ 除法
    + % 取模(取余)
    + ++ 自增(+=1)
    + -- 自减(-=1)

+ 逻辑运算符
    + == 等于
    + != 不等于
    + \> 大于
    + \< 小于
    + \>= 大于等于
    + \<= 小于等于

+ 关系运算符
    + && 与(and)
    + || 或(or)
    + ! 非(not)

+ (按)位运算符
    + & 与and
    + | 或or
    + ~ 非not
    + ^ 异或
    + << 左移n位，低位补0
    + \>> 右移n位，
    + \>>> 无符号右移n位,高位补0
+ 赋值运算符
    + = 赋值
    + += 先加再赋值(let a=1,a+=1 or a=a+1)
    + -= 减
    + *= 乘
    + /= 除
+ 三元运算符
    + `条件 ? one : two`,满足条件one,否则two，其实就是一个if/else的精简版
+ 字符运算符
    + \+ 将二个或个字符串连接成新的字符串。
+ 类型运算符
    + typeof 一个一元运算符，返回变量类型

## 控制语句(条件,循环)

TypeScript中控制语法有条件(if)，选择(switch)，循环(for,while)

+ 条件if...else if...else
  ```typescript
  let inputNum=0
  if(inputNum<-1){
      console.log("负数")
  }else if(inputNum<10){
      console.log("小于10")
  }else if(inputNum<100){
      console.log("小于100")
  }else{
      console.log("这个数真大")
  }
  //其中else if和else分支可以不用实现，根据事件情况而定
  ```
+ 选择(条件的一种)switch
  ```typescript
  let inputNum=0
  switch (inputNum) {
      case 0:
          console.log("0")
          break //跳出switch模块，如果不使用该关键字，会继续往下执行
      case 1:
          console.log("1")
          //case 2的代码会执行
      case 2:
          console.log("2")
          break
      default: //相当于else,如果不处理未包含的条件，可以不用实现
          console.log("default")
      break
  }
  ```
+ 循环for...
  ```typescript
  //常用1
  for(let i=0;i<3;i++){
      if(i==2){ 
          break //退出for循环
      }else if(i==0){
          continue //终止本次循环，继续循环
      }
      console.log(`i = ${i}`) //仅会输出1
  }
  
  //常用2,在对array，enum，对象中很常用
  //for...in
  let list:string[]=["Hello","World"]
  for(let i in list){
      console.log(`list[${i}] = ${list[i]}`)
  }
  //常用3,与in类似，in是根据显示位置，of是直接显示对象
  //for...of
  for(let val of list){
      console.log(`${val}`)
  }
  //常用4
  //forEach，in和of结合
  list.forEach((val,i,array)=>{ //不使用array可以不用写
      console.log(`list[${i}] = ${val}`)
  })
  //其他5,判断数组是否都满足条件
  var isAll=list.every((val,i)=>{
      return i==1 ? false : true //false就是break简写
  })
  //isAll，true都满足条件，false至少有1个不满足条件
  //其他6，判断数组是否有1个或多个满足条件
  var isAll=list.some((val,i)=>{
      return i==1 ? false : true //true就是break简写
  })
  ```
+ 循环while
  ```typescript
  let i=0           
  //常用1，先判断再执行
  while(i<3){
      console.log(`i = ${i++}`)
      if(i==0){
          continue //中断本次，继续循环,continue,break是循环中的二个选项，辅助
      }else if(i==3){
          break //中断执行
      }   
  }
  //常用2,先执行，再判断
  do{
      console.log(`i = ${i++}`) 
  }while (i<0)
  ```

## 函数

简单的说函数就是一组代码，也可以说是对功能的封装，就像工厂一样，通过传递参数，函数内部数据加工，最后将结果返回。

+ 简单函数
  <br/>最简单的函数，没有参数也没有返回值
  ```typescript
  //常用1
  function functionName() {
      //执行代码  
      console.log("to do")
  }
  
  //函数调用
  functionName()
  ```
+ 带参数的函数
  ```typescript
  //常用2，带参数的函数
  function add(a: number, b: number): number {
      return a + b
  }
  
  add(1, 2)
  
  //常用3,
  function joinMessage(name: string, ...word: string[]) {
      return name + ":" + word.join(",")
  }
  
  console.log(joinMessage('Rock', 'It\'s', 'wonderful', 'today.'))
  
  //常用3,变种
  function joinMessage2(name: string, word: string[]) {
      return name + ":" + word.join(",")
  }
  
  console.log(joinMessage2('Rock', ['It\'s', 'wonderful', 'today.']))
  ```
+ 匿名函数
  没有函数名的函数
  ```typescript
  var add:number=function(x:number,y:number):number {
    return x+y
  }
  ```
+ 构造函数
  ```typescript
  class User{
      name:string
      age:number
      constructor(name:string,age:number){
          this.name=name
          this.age=age
      }
  }
  let user=new User('Rock',8)
  console.log(user)
  ```
+ 构建函数Function，动态创建函数
  ```typescript
  let addFun=Function("x","y","return x+y")
  console.log(addFun(1,2))
  ```
+ 递归函数
  自己调用自己
  ```typescript
  function autoAdd(start:number,end:number):number{
      let result=start
      if(start<end){
          result+=autoAdd(start+1,end) //递归函数
      }
      return result
  }
  ```
+ Lambda函数
  格式:([param1,param2,...param n])=>statement
  ```typescript
  let add=(x:number,y:number)=>x+y
  console.log((add(1,2)))
  
  //练习
  let printType=(x:any)=>{
    if(typeof x=="number"){
        console.log(`${x} is a number.`)
    }else{
        console.log(`${x} is a ${typeof x}`)
    }
  }
  printType(123)
  printType("123")
  //参数是可选的
  ```
+ 函数重载
  函数名相同，但参数不同，返回值无所谓
  ```typescript
  //方式1,编译器直接报错，感觉我自己没搞懂...
  function add(x:number,y:number){
      return x+y
  } 
  function add(x:number,y:number,z:number){
      return x+y+z
  }
  
  //方式2
  function add(x:number,y:number,z?:number):number{
    if(z == undefined){
        return x+y
    }else{
        return x+y+z
    }
  }
  
  console.log(add(1,2))
  console.log(add(1,2,3))
  ```

## 集合

## 联合类型

## 接口

官话：接口是一系列抽象的集合。简单的对具体实现的结构定义，但又不实现。

+ 接口
  ```typescript
  interface IUser{
      name:string,
      age:number,
      print:()=>void
  }
  
  var user:IUser={
      name: "",
      age: 0,
      print: function(): void {
          console.log(`name = ${this.name},age = ${this.age}`)
      }
  }
  user.name='Rock'
  user.age=20
  user.print()
  ```
+ 接口和数组，对数组起到类型一致性作用

```typescript
  interface INameList {
    [index: number]: string
}

let names: INameList = ["Rock", "Tom", "Jerry"]
//let names:INameList=["Rock","Tom","Jerry",123]//会报错
  ```

+ 接口继承extends
  ```typescript
  interface IUser{
    name:string
    getName:()=>string
  }
  // 单继承
  interface IGroup extends IUser{
    groupName:string
    getName:()=>string
  }
  //多继承
  interface IAdamin extends IUser,IGroup{
    nickName:string
    getName:()=>string
  }
  
  let admin:IAdamin={
    nickName: "Rock",
    name: "Tom",
    groupName: "Abc",
    getName: function(): string {
      return this.name
    }
  }
  ```

## 类

类是描述不同对象的相同属性和方法。

+ 类
  类包含类名，方法(访问控制，方法名，参数，返回类型)，构造方法(没有返回值，方法名固定constructor)，属性等
  访问控制可以修饰类和方法，包含public(默认)、protected和private。
  ```typescript
  //结构
  class User{
    public name:string //public默认
    private age:number //除了自己本身，其他都不可以方法
    protected address:string //受保护，可以被自己和子类访问
    constructor(name:string,age:number,add:string){
      this.name=name
      this.age=age
      this.address=add
    }
  
    // 类方法，不用写function,返回为void,可以不用写
    print():void{
      console.log(`name=${this.name},age=${this.age}`)
    }
  }
  //创建使用关键字new
  let user=new User("Rock",30)
  user.print()
  ```
+ 类继承extends
  类的继承和接口大相径庭。<br/>
  注意：类不支持多继承，子类可以对父类方法进行重写。
  ```typescript
  //结构
  class User{
      name:string
      constructor(name:string){
          this.name=name
      }
  
      print():void{
          console.log(`name=${this.name}`)
      }
  }
  
  class Group extends User{
      groupName:string
      constructor(name:string,groupName:string){
          super(name)//实现父类的构造方法，和java差不多
          this.groupName=groupName
      }
  
      print():void{
          //如果需要调用父类方法可以使用
          super.print()
          console.log(`groupName=${this.groupName}`)
      }
  }
  
  //创建使用关键字new
  let user=new Group("Rock","Abc")
  user.print()
  ```
+ 类与接口

  ```typescript
  interface IUser {
    name: string
    print:()=>void
  }
  
  //使用implement实现接口
  class User implements IUser {
    name: string;
    constructor(name:string){
      this.name=name
    }
    print(){
      console.log(`name=${this.name}`)
    }
  }
  
  //使用,user,user2使用都可以，和Java几乎一样
  let user:IUser=new User("Rock")
  let user2:User=new User("Rock")
  user.print()
  console.log(user instanceof User)
  ```

+ static关键字
  static修饰类的变量或方法可不在不创建该类的情况下直接使用，可以不用创建该类。由static修饰的变量叫静态变量，修饰的方法叫静态方法。
  ```typescript
  class MathUtil{
      static PI:number=3.141592
      static add(x:number,y:number):number{
          return x+y
      }
  }
  
  console.log(MathUtil.PI)
  console.log(MathUtil.add(1,2))
  ```
+ instanceof运算符合，判断对象是否属于某个类
  ```typescript
  class User{
  
  }
  class Group{
  
  }
  let user:User=new User()
  console.log(user instanceof Group) //false
  console.log(user instanceof User) //true
  ```

## 对象

TypeScript中的对象是以key:Value方式的实例。Value可以是函数、对象、基础类型等等。

  ```typescript
  let user = {
    name: "Rock",
    age: 30,
    print: function () {
        console.log(this)
    }
}
//使用
console.log(user.name)
user.print()
//创建好后可以修改值，但不能新增和删除key
//user.add="China" //错误示范
  ```

__鸭子类型__
在TypeScript中对象和类极为相类，可以在一些函数参数中相互转换，如:

```typescript
interface IPoint {
    x: number
    y: number
}

class Point {
    a: number
    b: number

    constructor(a: number, b: number) {
        this.a = a
        this.b = b
    }
}

//这儿为了演示接口和类相同，股p0,p1分别使用了接口和类
function addPoint(p0: IPoint, p1: Point): IPoint {
    return {x: p0.x + p1.a, y: p0.y + p1.b}
}

//是否很有意思
var p0 = addPoint({x: 1, y: 2}, {a: 2, b: 1})
console.log(p0)
```

## 包装类型(Number,String,Array,Map)
+ Number
```typescript
let num=new Number(100)
console.log("MIN_VALUE",Number.MIN_VALUE)//min
console.log("MAX_VALUE",Number.MAX_VALUE)//max
console.log("NEGATIVE_INFINITY",Number.NEGATIVE_INFINITY) //-∞
console.log("POSITIVE_INFINITY",Number.POSITIVE_INFINITY)//+∞
console.log("toFixed",num.toFixed(2))//保留2位小数，无小数.00
console.log("toLocaleString",num.toLocaleString())//转为字符串，10进制
console.log("toString",num.toString(16))//默认10进制,2进制，8进制,16进制，其实这个数字可以随便写(2-36)
console.log("valueOf",num.valueOf())//拆箱，输出原始类型，如果不拆箱，不能与number进行比较
console.log("toPrecision",num.toPrecision(2))//把数字格式化为指定的长度。
```

+ String
```typescript
let txt = new String("string");
// 或
// let txt="string"
console.log("length",txt.length)//字符串长度，中文也仅占用1
console.log("charAt",txt.charAt(2))//返回在指定位置的字符。
console.log("charCodeAt",txt.charCodeAt(2))//返回在指定的位置的字符的 Unicode 编码。
console.log("concat",txt.concat("123","4"))//+，将一个或多个字符串相加
console.log("indexOf",txt.indexOf("r"))//首次出现某个字符串的位置
console.log("lastIndexOf",txt.lastIndexOf("ing"))//最后出现某个字符串的位置
//如果调用字符串在比较字符串之前，则返回负数（通常是 -1）。
//如果调用字符串与比较字符串相等，则返回 0。
//如果调用字符串在比较字符串之后，则返回正数（通常是 1）
console.log("localeCompare",txt.localeCompare("string字符"))//用本地特定的顺序来比较两个字符串。
console.log("match",txt.match("str"))

```

+ Array
```typescript

```

+ Map
```typescript

```

## 泛型

泛型是一种编程语言特性，允许定义函数、类、接口等时，采用占位符的代表具体的类型。在编程上，既提高了代码的灵活性，也保证了代码的安全性，开发中极为有用

+ 泛型使用(类、接口，方法)
  ```typescript
  //方法
  function joinAny<T>(a: T, b: T): string {
    return `${a}${b}`
  }
  
  //接口
  interface Store<K, V> {
    key: K
    value: V
  }
  
  //类
  class Store<K, V> {
    key: K
    value: V
  
    constructor(key: K, value: V) {
      this.key = key
      this.value = value
    }
  }
  
  //方法使用
  console.log(joinAny<number>(1, 2))
  
  //接口使用
  let store: Store<string, number> = {
    key: "Rock",
    value: 20
  }
  
  //类使用
  let store2 = new Store<string, string>("name", "Rock")
  console.log(store, store2)
  ```

+ 泛型约束
  对泛型属性的一些限制
  ```typescript
   //用户接口
  interface IUser{
    name:string
  }
  //组接口
  interface IGroup extends IUser{
    groupName:string
  }
  //标签接口
  interface ITag {
    name:string
  }
  
  function print<T extends IGroup>(obj:T){
    console.log(obj)
  }
  
  let group:IGroup={
    name: "Rock",
    groupName: "Abc"
  }
  let tag:ITag={
    name:"Tag"
  }
  print(group)
  //print(tag)//报错
  ```

+ 泛型默认
  默认如果不修改默认为什么类型
  ```typescript
  function add<T=number>(x:T,y:T):string{
    return `${x}${y}`
  }
  add(1,3)
  add<string>(add(1,2),"a")//string可以不用写，会推撤出类型
  ```

## 命名空间
主要解决名称重复的问题
```typescript
namespace Test{
  //export关键字表示是否允许在命名空间外部使用
    export class User{
        name:string
        constructor(name:string){
            this.name=name
        }
    }

    //命名空间嵌套
   export namespace Group{
        export class Group{
            name:string
            constructor(name:string){
                this.name=name
            }
        }
    }
}
 class User{
    name:string
    constructor(name:string){
        this.name=name
    }
}

//使用
let testUser:Test.User=new Test.User("TRock")
let user:User=new User("TRock")
let grop:Test.Group.Group=new User("TRock")
```
## 模块
对TypeScript，在不同文件中即为不同的模块，如果需要引入或则可以让其他模块引入则需要使用到export和import关键字。

IUser.ts
```typescript
export interface IUser{
    //如果不添加export,IUser仅可以在本文件(IUser.ts)中使用，其他文件不能访问。
}
  
```
User.ts
```typescript
import iUser=require("./IUser")
export class User implements iUser.IUser{
    
}
```

## 声明文件


## 检测

