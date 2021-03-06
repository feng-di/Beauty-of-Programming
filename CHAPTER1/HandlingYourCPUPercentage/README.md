# 控制你的CPU占有率
`CPU占有率：`在任务管理器的一个刷新周期内，CPU忙（执行应用程序）的时间和刷新周期总时间的比率， 就是CPU的占用率，也就是说，任务管理器中显示的是每个刷新周期内CPU占用率的统计平均值。 
因此，我们可以写一个程序，让它在任务管理器的刷新期间内一会儿忙， 一会儿闲， 然后通过调节忙／ 闲的比例， 就可以控制任务管理器中显示的CPU占用率。

可以同时参考操作系统原理中，描述进程管理的时候说道：操作系统在运行的时候有一个个的进程，这些进程或忙，或闲。很多人都只关注了进程忙的时候做了什么而忽略了进程闲的手
在干什么。实际上，进程闲的时候通常有以下状态：

* 等待用户输入
* 监听时间发生
* 处于休眠状态

也就是说：可以通过实现以上三种状态的任意一种，通过代码级别的控制在实现。

占用cpu最简单的方式就是一个循环
```java
while(true){} //考虑执行的时候程序在占用CPU
```
不占用cpu最简单的方式就是让CPU休眠
```java
 Thread.sleep(TIME)
```
最终需要的是一会儿占用一会儿不占用，同时这个控制是人为通过程序干预的
```java
public class HandleCPU{
    public static final double TIME = 1000;  //TIME=周期
    
    private static void handle(double rate) throws InterruptedException{
        while (true){
            runCPU(rate * TIME);
            Thread.sleep((long) (TIME - rate * TIME));
        }
    }
   
    private static void runCPU(double time) {
        long startTime = System.currentTimeMillis();
        while ((System.currentTimeMillis() - startTime) < time) {
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
        handle(0.5);
    }
}
```
以上读者在MacOS 10运行中，CPU占有率维持在了50%左右。但实际上，CPU负载的折线图依旧没有成为直线，而是类似于锯齿形一般。

