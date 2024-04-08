## Object语法

### Hello World
```objective-c
// 注释

#import <Foundation/Foundation.h>

//方法|函数，int 返回类型，int argc 函数参数
int main(int argc, const char * argv[]) {
    @autoreleasepool { //自动释放池，自动对资源进行回收
        // 输出日志
        NSLog(@"Hello, World!");
        // %@ 对象
        NSLog(@"%@, World!",@"Hello");
        // %d %i 整型
        NSLog(@"%d+%d=%i",1,1,2);
        // %f 浮点型
        NSLog(@"π=%f",3.141592);
        // %c 字符
        NSLog(@"%c, World!",'K');
    }
    
    return 0;//返回值
}
```

### 基本数据类型
```objective-c
        // 整型
        int intValue = 5;
        // 短整
        short shortValue = 5;
        // 长整
        long longValue = 5;
        // 更长整
        long long longlongValue =5;
        
        NSLog(@"Int:%i,%hi,%li,%lli",intValue,shortValue,longValue,longlongValue);
        
        // 浮点型
        float floatValue = 5.0;
        // 双精度
        double doubleValue = 5.0;
        // 科学计数法
        double doubleValue2 = 6.6e10;
        
        NSLog(@"Float:%f,%f,%e",floatValue,doubleValue,doubleValue2);
        
        // 字符类型,utf-8
        char charValue='a';
        char charValue2='\%';
        NSLog(@"char:%c,%c",charValue,charValue2);
        
        // 布尔类型
        bool boolValue = true;
        NSLog(@"%i",boolValue);
        
        // id 类型 存储为对象，可以理解为地址引用
```
### 运算符
```objective-c
        //=,赋值运算
        int a=2; //
        int b=3;
        
        //算术运算符
        NSLog(@"a+b=%i",a+b);
        NSLog(@"a-b=%i",a-b);
        NSLog(@"a*b=%i",a*b);
        NSLog(@"a/b=%i",a/b);
        NSLog(@"a%%b=%i",a%b);//取模(余)
        NSLog(@"a++=%i",a++); //先运算，后做加法，++,--
        NSLog(@"++b=%i",++b);//先做加法，再运算
        NSLog(@"+=b=%i",b+=1);//拓展运算符，+=，-=，/=,*=
        
        //比较运算符>,<,>=,<=,==,!=
        NSLog(@"1>1=%i",1>1);
        NSLog(@"1==1=%i",1==1);
        NSLog(@"1!=1=%i",1!=1);
        
        //逻辑运算符,&&,|| !
        NSLog(@"1>1&&1==1=%i",1>1&&1==1); //and
        NSLog(@"1>1||1==1=%i",1>1||1==1); //or
        NSLog(@"!(1>1)=%i",!(1>1)); //not,true->false,false->true
        
        //三目运算符,?:
        NSLog(@"1>2?true:false=%s",1>2?"true":"false");
```
### 选择(条件)语句
```objective-c
        // 选择语句
        // if(条件)...else if(条件2)...else...
        if(1>2){
            NSLog(@"1>2");
        }else if(1<2){
            NSLog(@"1<2");
        }else{
            NSLog(@"else");
        }
        
        //switch...case...default...
        int x=1;
        switch (x) {
            case 1:
                NSLog(@"x=1");
                break;
            case 2:
                NSLog(@"x=2");
                break;
            default:
                NSLog(@"default");
                break;
        }
```

### 循环语句
```objective-c
        //循环语句
        //for
        for(int i=0;i<2;i++){
            NSLog(@"for,i=%d",i);
        }
        
        // while
        int i=0;
        while(i<2){
            NSLog(@"while,i=%i",i++);
        }
        NSLog(@"while out,i=%i",i);
        
        // do...while
        i=0;
        do{
            i++;
            if(i<2){
                NSLog(@"continue");
                continue;
            }else if(i==2){
                NSLog(@"break");
                break;
            }
            NSLog(@"do,i=%i",i);
            
        }while(i<2);
        NSLog(@"do,out,i=%i",i);
        // break 跳出循环,continue 中断本次循环
```
### 预处理程序
```objective-c
        // 预处理程序
      
        // 文件包含(import)
        // #import <Foundation/Foundation.h>
        
        // 宏定义(define)(可以定义在问题头部，也可以定义在方法中)
        # define PI 3.141592
        # define PLUS(x,y) x+y
        NSLog(@"PI=%f",PI);
        NSLog(@"PLUSE=%d",PLUS(1,2));
        
        // 条件编译(#if)
        #if RELEASE == 1
            NSLog(@"Release Version") //该代码缺乏分号;,由于不满足编译条件，不会编译，不会报错
        #elif RELEASE == 0
            NSLog(@"Debug Version");
        #else
            NSLog(@"Develop Version");
        #endif
        
        // 错误、警告处理
        //编译不能通过
        //#error "错误信息"
        
        //编译可以通过，但是会提示警告
        #warning "这是一个警告"
        
        // 编译器控制
        #pragma mark - 添加mark,方便快速定位
```

### 枚举类型
```objective-c
//C枚举定义
enum Gender{
    Man,
    Woman,
};

//OC枚举定义,单选
typedef NS_ENUM(int, Child) {
    Boy,
    Girl,
};

//OC枚举定义,多选
typedef NS_OPTIONS(int, Date){
    Day=0,//1
    Week=1<<0,//2
    Month=1<<2,//4
};
```

### 面向对象

#### 类和对象
`Person.h`
```objective-c
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

//@interface类定义，可以理解为接口
@interface Person : NSObject{
    NSString *_name; //* 理解为指针类型，因为NSString不是基础类型
    int _age;
}

-(void)setName:(NSString*)name;
-(NSString*)getName;
-(void)setAge:(int)age;
-(int)getAge;
-(void)print;
@end

NS_ASSUME_NONNULL_END

```
`Person.m`
```objective-c
#import "Person.h"

//@implementation 类的实现，可以理解为java的类
@implementation Person
-(void)setName:(NSString*)name{
    _name=name;
}
-(NSString*)getName{
    return _name;
}
-(void)setAge:(int)age{
    _age=age;
}
-(int)getAge{
    return _age;
}
-(void)print{
    NSLog(@"name=%@,age=%d",_name,_age);
}
@end
```
`main.m`
```objective-c
#import <Foundation/Foundation.h>
#import "Person.h"

//面向对象
//类，对象
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 对象创建
        Person *person=[[Person alloc]init];
        //方法调用
        [person setName:@"Rock"];
        [person setAge:20];
        [person print];
    }
    
    return 0;
}

```
### 类的属性和方法
`Person.h`
```objective-c
NS_ASSUME_NONNULL_BEGIN

//@interface类定义，可以理解为接口
@interface Person : NSObject{
    NSString *_name; //* 理解为指针类型，因为NSString不是基础类型
    int _age;
    NSString *_address;
}
//@property 自动生成实例变量的set和get方法供外界使用
//nonatomic 非原子性
//strong 强引用，适用于OC对象，一般使用string weak：适用于OC对象，特殊情况(循环引用)assign
//readwrite对外可读写，readonly 外界只可读
@property (nonatomic,strong,readwrite) NSString *name;
@property (nonatomic,assign,readwrite) int age;

-(void)print;
-(void)setAddress:(NSString*)address;
//- 符号：代表实例方法。实例方法是属于类的实例的，只能通过类的实例来调用。
-(NSString*)getAddress;
//+ 符号：代表类方法（类方法也被称为静态方法）
+(NSString*)getDescription;


@end

NS_ASSUME_NONNULL_END
```
`Person.m`
```objective-c
NS_ASSUME_NONNULL_BEGIN

//@interface类定义，可以理解为接口
@interface Person : NSObject{
    NSString *_name; //* 理解为指针类型，因为NSString不是基础类型
    int _age;
    NSString *_address;
}
//@property 自动生成实例变量的set和get方法供外界使用
//nonatomic 非原子性
//strong 强引用，适用于OC对象，一般使用string weak：适用于OC对象，特殊情况(循环引用)assign
//readwrite对外可读写，readonly 外界只可读
@property (nonatomic,strong,readwrite) NSString *name;
@property (nonatomic,assign,readwrite) int age;

-(void)print;
-(void)setAddress:(NSString*)address;
//- 符号：代表实例方法。实例方法是属于类的实例的，只能通过类的实例来调用。
-(NSString*)getAddress;
//+ 符号：代表类方法（类方法也被称为静态方法）
+(NSString*)getDescription;


@end

NS_ASSUME_NONNULL_END

```
`main.m`
```objective-c
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

//@interface类定义，可以理解为接口
@interface Person : NSObject{
    NSString *_name; //* 理解为指针类型，因为NSString不是基础类型
    int _age;
    NSString *_address;
}
//@property 自动生成实例变量的set和get方法供外界使用
//nonatomic 非原子性
//strong 强引用，适用于OC对象，一般使用string weak：适用于OC对象，特殊情况(循环引用)assign
//readwrite对外可读写，readonly 外界只可读
@property (nonatomic,strong,readwrite) NSString *name;
@property (nonatomic,assign,readwrite) int age;

-(void)print;
-(void)setAddress:(NSString*)address;
//- 符号：代表实例方法。实例方法是属于类的实例的，只能通过类的实例来调用。
-(NSString*)getAddress;
//+ 符号：代表类方法（类方法也被称为静态方法）
+(NSString*)getDescription;


@end

NS_ASSUME_NONNULL_END
```
### 继承
`Student.h`
```objective-c
#import <Foundation/Foundation.h>
#import "Person.h"

NS_ASSUME_NONNULL_BEGIN

@interface Student : Person

@property (nonatomic,assign)int score;

//拓展新方法
-(void)study;

//暂时比较模糊，后续再理解
-(void)setAddress:(NSString *)address name:(NSString *)name;

//重载父类方法
-(void)print;
@end

NS_ASSUME_NONNULL_END

```
`Student.m`
```objective-c
#import <Foundation/Foundation.h>
#import "Person.h"

NS_ASSUME_NONNULL_BEGIN

@interface Student : Person

@property (nonatomic,assign)int score;

//拓展新方法
-(void)study;

//暂时比较模糊，后续再理解
-(void)setAddress:(NSString *)address name:(NSString *)name;

//重载父类方法
-(void)print;
@end

NS_ASSUME_NONNULL_END

```
`main.m`
```objective-c
#import <Foundation/Foundation.h>
#import "Student.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 对象创建
        Student *student=[[Student alloc]init];
        //方法调用
        [student setAddress:@"3st" name:@"Rock"];
        student.score=85;
        [student print];
    }
    
    return 0;
}
```
### 多态
父类的方法会更具运行时的情况自动调用对应子类的方法。
代码和上相同，为了简单，不再创建继承Person的子类
`main.m`
```objective-c
#import <Foundation/Foundation.h>
#import "Student.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 对象创建
        Person *person=[[Student alloc]init];
        //方法调用
        [person print];
        //输出>(null)得分0
    }
    
    return 0;
}
```
### 动态类型和动态绑定

  + 动态类型：程序直到执行时才知道所属的类是什么。
  + 动态绑定：程序直到执行时才知道实际调用的方法是什么。

`main.h`
```objective-c
#import <Foundation/Foundation.h>
#import "Student.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 静态类型
        Person *person=[[Student alloc]init];
        // 方法调用
        [person print];
        
        // 动态类型
        id obj=person;
        [obj print];
        
        // 静态类型与动态类型比较
        // 静态类型安全性高，可读性强，大部分情况下我们都使用静态类型；
        
        // 动态绑定
        // isKindOfClass 判断对象是否属于哪个类,类是Java instanceof
        NSLog(@"persion isKindOfClass %i",[person isKindOfClass:[Person class]]);
        // isMemberOfClass 判断对象是否为哪个类的实例，必须是具体类的实例，父类不是该对象实例
        NSLog(@"persion isMemberOfClass %i",[person isMemberOfClass:[Student class]]);
        
        // SEL 类是java的反射
        SEL sel1=@selector(setName:);
        // respondsToSelector 判断对象是否实现了某个方法，如果persion未实现setName方法，会异常
        if([person respondsToSelector:sel1]){
            [person performSelector:sel1 withObject:@"Rock"];
        }
        [person print];
        
    }
    
    return 0;
}
```

### 对象初始化

  + 初始化：从系统获取一块内存，准备用于存储对象；

```objective-c
// 自定义构造方法
-(Person*)initWithName:(NSString*)name age:(int)age;

// 自定义构造方法实现
-(Person*)initWithName:(NSString*)name age:(int)age{
    self=[super init];
    if (self){
        self.name=name;
        self.age=age;
    }
    return self;
}

// 初始化
 Person *person=[[Person alloc]initWithName:@"Rock" age:18];
[person print];

```

### 属性作用域
```objective-c
// 外部变量：外部变量是在全局范围内声明的变量，可以被程序中的所有函数访问。
extern NSString* Desc=@"这是Person描述";

// 静态变量：静态变量是在函数内部声明的变量，但是它们在程序的整个执行过程中都存在，并且只初始化一次。
static int count=0;

// 长量：常量是值不可变的变量，在定义时就必须进行初始化，并且不能再次赋值。
const int MAN=0;

@interface Person : NSObject{
    // 在Objective-C中，@private、@protected、@public等关键字用于控制类中实例变量的访问权限。
    // @protected关键字用于声明实例变量为私有的。私有变量只能在声明它们的类的内部访问，无法被类的子类或其他类直接访问。
    // @private关键字用于声明实例变量为受保护的。受保护的变量可以在声明它们的类及其子类中访问，但不能被其他类直接访问。
    // @public关键字用于声明实例变量为公开的。公开变量可以在声明它们的类以及其他类中直接访问。
    // 调用变量 object-> 变量名，如：person->_address;
    @protected
    NSString *_name; //* 理解为指针类型，因为NSString不是基础类型
    @private
    int _age;
    @public
    NSString *_address;
}
```
### 分类和协议


### Foundation
+ 数字对象NSNumber
  ```objective-c
  // 创建对象方式创建
  NSNumber *nsNumber=[NSNumber numberWithInt:123];
  // 便捷创建
  NSNumber *nsNumber2=@(1234);
  
  //比较是否相同
  NSLog(@"%d",[nsNumber  isEqual: @(123)]);
  //比较大小
  NSLog(@"%d",[nsNumber  compare: @(123)]);
  ```
+ 

  