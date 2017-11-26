# java-learning-notes
## 1、java命令
* javac：用于将java源文件编译为class字节码文件
* java：可以运行class字节码文件，如：java HelloWorld
## 2、java分为三个体系
* JavaSE（J2SE）java平台标准版
* JavaEE（J2EE）java平台企业版
* JavaME（J2ME）java平台微型版
## 3、java语言特性
java语言支持动态绑定，而C++只对虚函数使用动态绑定。
## 4、类中包含的变量
* 局部变量
* 成员变量
* 类变量（必须声明为static）
## 5、源文件声明规则
* 一个源文件中只能有一个public类；
* 一个源文件可以有多个非public类；
* 源文件的名称应该和public类的类名保持一致；
* 如果一个类定义在某个包中，那么package语句应该在源文件的首行；
* 如果源文件中包含import语句，那么应该放在package语句和类定义之间。如果没有package语句，那么import语句应该在源文件中最前面；
* import语句和package语句对源文件中定义的所有类都有效，在同一源文件中，不能给不同的类不同的包声明。
## 6、java数据类型
* byte（有符号8位整数）
* short
* int
* long
* float
* double
* char
## 7、java常量

```
final double PI = 3.1415927
```

## 8、局部变量和实例变量区别
|局部变量|实例变量|
|:-------------:|:-------------:|
|访问修饰符不能用于局部变量|可用于实例变量|
|局部变量无默认值|有默认值|
## 9、类变量（静态变量）
类变量被声明为public static final类型时，类变量名称必须使用大写字母。
## 10、访问控制
|修饰符|当前类|同一包内|子孙类|其他包|
|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|
|pulic|√|√|√|√|
|protected|√|√|√|×|
|default|√|√|×|×|
|private|√|×|×|×|
protected访问修饰符不能修饰类和接口，接口中的成员变量和成员方法不能声明为protected。
## 11、访问控制和继承
|父类|子类|
|:-------------:|:-------------:|
|public|必须public|
|protected|protected或public，不能为private|
|private|不能被继承|
## 12、非访问修饰符
* static->类变量和类方法
* final->修饰类不能被继承，方法不能被重写，修饰变量不可修改（接近const）
* Abstract->创建抽象类和抽象方法


## @Component, @Service, @Controller, @Repository注解的区别
* @Component, @Service, @Controller, @Repository是spring注解，注解后可以被spring框架所扫描并注入到spring容器来进行管理
* @Component是通用注解，其他三个注解是这个注解的拓展，并且具有了特定的功能
* @Repository注解在持久层中，具有将数据库操作抛出的原生异常翻译转化为spring的持久层异常的功能。
* @Controller层是spring-mvc的注解，具有将请求进行转发，重定向的功能。
* @Service层是业务逻辑层注解，这个注解只是标注该类处于业务逻辑层。
* 用这些注解对应用进行分层之后，就能将请求处理，义务逻辑处理，数据库操作处理分离出来，为代码解耦，也方便了以后项目的维护和开发。

## java只使用try和finally不使用catch的原因和场景
JDK并发工具包中，很多异常处理都使用了如下的结构，如AbstractExecutorService，即只有try和finally没有catch。

```
class X   
{  
    private final ReentrantLock lock = new ReentrantLock();  
    // ...  
   
    public void m()  
    {  
    lock.lock();  // block until condition holds  
    try   
    {  
        // ... method body  
    } finally  
    {  
        lock.unlock()  
    }  
     }  
} 
```

为什么要使用这种结构？有什么好处呢？先看下面的代码

```
public void testTryAndFinally(String name)  
{  
       try  
       {  
           name.length();// NullPointerException  
       }  
       finally  
       {  
           System.out.println("aa");  
       }  
} 
```

* 传递null调用该方法的执行结果是：在控制台打印aa，并抛出NullPointerException。即程序的执行流程是先执行try块，出现异常后执行finally块，最后向调用者抛出try中的异常。这种执行结果是很正常的，因为没有catch异常处理器，所有该方法只能将产生的异常向外抛；因为有finally，所以会在方法返回抛出异常之前，先执行finally代码块中的清理工作。  
* 这种做法的好处是什么呢？对于testTryAndFinally来说，它做了自己必须要做的事(finally)，并向外抛出自己无法处理的异常；对于调用者来说，能够感知出现的异常，并可以按照需要进行处理。也就是说这种结构实现了职责的分离，实现了异常处理(throw)与异常清理(finally)的解耦，让不同的方法专注于自己应该做的事。  
* 那什么时候使用try-finally，什么时候使用try-catch-finally呢？很显然这取决于方法本身是否能够处理try中出现的异常。如果自己可以处理，那么直接catch住，不用抛给方法的调用者；如果自己不知道怎么处理，就应该将异常向外抛，能够让调用者知道发生了异常。即在方法的签名中声明throws可能出现而自己又无法处理的异常，但是在方法内部做自己应该的事情。

java 的异常处理中，
* 在不抛出异常的情况下，程序执行完 try 里面的代码块之后，该方法并不会立即结束，而是继续试图去寻找该方法有没有 finally 的代码块;
* 如果没有finally代码块，整个方法在执行完try代码块后返回相应的值来结束整个方法;
* 如果有finally代码块，此时程序执行到try代码块里的return语句之时并不会立即执行return，而是先去执行finally代码块里的代码;
* 若finally代码块里没有return或没有能够终止程序的代码，程序将在执行完finally代码块代码之后再返回try代码块执行return语句来结束整个方法;
* 若finally代码块里有return或含有能够终止程序的代码，方法将在执行完finally之后被结束，不再跳回try代码块执行return;
* 在抛出异常的情况下，原理也是和上面的一样的，你把上面说到的try换成catch去理解就OK了 *_*

## spring4.x定时任务执行

```
package com.yzhotel.task;

import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
@EnableScheduling
public class WxTask {

    @Scheduled(cron = "0/5 * * * * ?")
    public void sendMessage(){
    	System.out.println("定时任务执行了！");
    }
}
```

## Mac环境中Jenkins的停止和启动命令
启动  
sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist  
停止  
sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist

## web.xml 配置中classpath: 与classpath*:的区别
classpath：只会到你的class路径中查找找文件; 
classpath\*：不仅包含class路径，还包括jar文件中(class路径)进行查找. 
