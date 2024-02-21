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

## 集合

## 联合类型

## 接口

## 类

## 对象

## 泛型

## 命名空间

## 模块

## 声明文件

## 检测

