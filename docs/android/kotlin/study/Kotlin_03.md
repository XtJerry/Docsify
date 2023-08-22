### 学习内容
1、协程

### 协程
1、背景
``` kotlin
在程序开发中，我们一直在防止应用被柱塞。为了解决这个问题，我们使用了不少方式：
 - 线程
 - 回调
 - Futures
 - 响应式扩展
 - 协程
 
1、开启线程有代价，故才有线程池；
2、协程开启也是有代价，只是极小
```
1、协程是什么？
``` kotlin
Kotlin 编写异步代码的方式是使用协程，这是一种计算可被挂起的想法。即一种函数可以在某个时刻暂停执行并稍后恢复的想法。

// 以下代码就是开启协程，就像java开启了一个线程
GlobalScope.launch {
        println("coroutine start")
        delay(1000)
        println("coroutine finish")
    }
```
2、协程开发
*协程使用需要导入coroutine包*
Gradle
``` kotlin
dependencies {
    ...
	implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.4.2")
}
```
Mevean
``` kotlin
<dependencies>
    ...
    <dependency>
        <groupId>org.jetbrains.kotlinx</groupId>
        <artifactId>kotlinx-coroutines-core</artifactId>
        <version>1.4.2</version>
    </dependency>
</dependencies>
```
3、协程与线程比较
``` kotlin
    val c = AtomicLong()
    // 执行完毕慢(60000+ms)
    for (i in 1..1_000_000L)
        thread(start = true) {//创建线程并+i
            c.addAndGet(i)
        }

    // 执行完毕极快(1600ms)
    for (i in 1..1_000_000L)
        GlobalScope.launch {//创建协程并+i
            c.addAndGet(i)
        }


    // 执行完毕最快(15ms)
    for (i in 1..1_000_000L)
        c.addAndGet(i)

    // 总结，开启线程是需要消耗资源的，开启协程也是要消耗资源，但极小

```
3、协程简单操作
``` kotlin
	// context 设置(作用：线程切换)
	// Dispatchers.Default			线程池执行(数据解析，密集型运算)
	// Dispatchers.IO				线程池执行(文件读写)
	// Dispatchers.Main 			主线程UI
	// Dispatchers.Unconfined		调用线程执行

	// start 设置(协程启动模式)
	// CoroutineStart.DEFAULT		立即等待被调度执行
	// CoroutineStart.ATOMIC		立即等待被调度执行，且开始执行器不能被取消，直到完毕或第一个挂起suspend
	// CoroutineStart.UNDISPATCHED	立即在当前线程执行协程体内容
	// CoroutineStart.LAZY			需要手动触发才可开启等待被调度执行

	val job = GlobalScope.launch(
        context = Dispatchers.IO,
        start = CoroutineStart.DEFAULT
    ) {
        println("coroutine start")
        delay(1000)
		doSomething() // 挂起函数
        println("coroutine finish")
    }

    //job.start()//启动协程(start=CoroutineStart.LAZY才需要手动启动)
    job.cancel("cancel")//取消协程
    //job.join() //等待协程结束
```
4、协程体(suspend)
``` kotlin
// 协程体是一个用suspend关键字修饰的一个无参，无返回值的函数类型;
// 被suspend修饰的函数称为挂起函数(挂起函数只能在协程中和其他挂起函数中调用，不能在其他地方使用)
suspend fun doSomething(){
	printlen("doSomething")
}
```
5、withContext/sync
``` kotlin
    GlobalScope.launch {
        println("coroutine2 start")
        doSomething()
        println("coroutine2 finish")
        withContext(context = Dispatchers.IO) {
            //不创建新的协程，在指定协程上运行代码块，且如果有多个withContext是串行执行的
            println("withContext")
        }
        async(context = Dispatchers.IO){
            println("async")//不创建新的协程，在指定协程上运行代码块，且如果有多个withContext是并行执行的
        }

    }
```
6、runBlocking
*runBlocking与launch都可以开启一个协程*
*区别：launch后不会柱塞线程，runBlocking会柱塞线程*
``` kotlin
runBlocking{
	//协程执行体
	delay(100)
	println("runBlocking")
}
println("finish")
```
	# 输出
	> runBlocking
	> finish


