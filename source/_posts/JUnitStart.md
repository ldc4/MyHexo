title: JUnit初窥
date: 2016-05-16 21:42:00
categories:
- JAVA
tags:
- junit
---

现在的发行版本是 4.12

junit的使用真的真的非常简单。。。

<!--more-->


## 基础用法
创建 Test Case 类
	* 创建一个名为 TestJunit.java 的测试类。
	* 向测试类中添加名为 testPrintMessage() 的方法。
	* 向方法中添加 Annotaion @Test。
	* 执行测试条件并且应用 Junit 的 assertEquals API 来检查。

创建 Test Runner 类
	* 创建一个 TestRunner 类
	* 运用 JUnit 的 JUnitCore 类的 runClasses 方法来运行上述测试类的测试案例
	* 获取在 Result Object 中运行的测试案例的结果
	* 获取 Result Object 的 getFailures() 方法中的失败结果
	* 获取 Result object 的 wasSuccessful() 方法中的成功结果



在MyEclipse下
只需要建立一个XXTest类
@Test 以及 assertEquals( , )

如果不是在MyEclipse这类集成JUnit工具的IDE下，就需要自己来运行测试用例
java -cp .;junit-4.XX.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore CalculatorTest
org.junit.runner.JUnitCore

也可以用JUnit.runClasses(xx.class);来得到一个Result类型的结果，调用result的方法来查看输出结果



JUnit重要的API：
Assert
TestCase
TestResult
TestSuite


## 高级用法

1.继承TestCase
复写setUp()和tearDown()，
分别表示在测试之前执行和测试之后执行的方法

也就是
@AfterClass
@BeforeClass
@After
@Before

2.时间测试
@Test(timeout=1000)
3.异常测试
@Test(expected=ArithmeticException.class)

4.参数化测试
a.RunWith(Parameterized.class)标注测试类
b.Parameterized.Parameters标注公共静态方法，返回测试数据数组或集合
c.为每个测试数据创建一个实例变量
d.需要一个公共构造函数来接收这些测试数据集合
e.然后用这些实例变量来测试

5.套件测试
a.RunWith(Suit.class)和Suit.SuitClasses({a.class,b.class,c.class})标注测试类
b.创建a,b,c三个测试类，这三个测试类会被同时执行

6.忽略测试
@Ignore


## JUnit的拓展框架
-Cactus: 是一个简单框架用来测试服务器端的 Java 代码（Servlets, EJBs, Tag Libs, Filters）
-JWebUnit: 一个基于 Java 的用于 web 应用的测试框架
-XMLUnit: 提供了一个单一的 JUnit 扩展类，即 XMLTestCase，还有一些允许断言的支持类
-MockObject: 在一个单元测试中，虚拟对象可以模拟复杂的，真实的（非虚拟）对象的行为，因此当一个真实对象不现实或不可能包含进一个单元测试的时候非常有用。

拓展框架等以后有用的时候再学。

## 参考资料
http://wiki.jikexueyuan.com/project/junit/extensions.html









