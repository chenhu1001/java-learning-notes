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
