---

---

# AOP

在软件业，AOP为Aspect Oriented Programming的缩写，意为：[面向切面编程](https://baike.baidu.com/item/%E9%9D%A2%E5%90%91%E5%88%87%E9%9D%A2%E7%BC%96%E7%A8%8B/6016335)，通过[预编译](https://baike.baidu.com/item/%E9%A2%84%E7%BC%96%E8%AF%91/3191547)方式和运行期动态代理实现程序功能的统一维护的一种技术。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的[耦合度](https://baike.baidu.com/item/%E8%80%A6%E5%90%88%E5%BA%A6/2603938)降低，提高程序的可重用性，同时提高了开发的效率。

AOP 三剑客

| 方法           | 说明              | 原理                                           | 实现                          | 特点                                                         |
| -------------- | ----------------- | ---------------------------------------------- | ----------------------------- | ------------------------------------------------------------ |
| APT            | 注解处理器        | 在编译器通过注解采集信息，生成 Java/Class 文件 | ButterKnife、Glide、EventBus3 | 基于 javac，但是如果需要修改已经存在的代码太麻烦，代码侵入比较强 |
| Aspectj        | 面向切面的框架    | 在编译后修改/生成指定 Class                    | Hugo                          | 功能强大，底层原理利用了 ASM，不够轻量，代码侵入比较少       |
| Javaassist/ASM | 操作 Class 的框架 | 按照 Class文件的格式，解析、修改、生成 Class   | QZone 超级不定                | Javaassist 的 Java API更易于使用；ASM 性能更好               |

静态织入：在编译器、运行前修改 Class，对系统无性能影响。

## APT

APT英文全称：Android annotation process tool是一种处理注解的工具，它对源代码文件进行检测找出其中的Annotation，使用Annotation进行额外的处理。
Annotation处理器在处理Annotation时可以根据源文件中的Annotation生成额外的源文件和其它的文件（文件具体内容由Annotation处理器的编写者决定），APT还会编译生成源文件和原来的源文件，将它们一起生成class文件。简言之：APT可以把注解，在编译时生成代码。

### Transform 

则是 class 生成 dex 之前，gradle 提供的处理 class的接口。

## AspectJ

AspectJ是一个面向切面的框架，它扩展了Java语言。AspectJ定义了AOP语法，所以它有一个专门的[编译器](https://baike.baidu.com/item/%E7%BC%96%E8%AF%91%E5%99%A8/8853067)用来生成遵守Java字节编码规范的Class文件。

代表框架： Hugo(Jake Wharton)

AspectJ支持编译期和加载时代码注入，在开始之前，我们先看看需要了解的词汇：
 **Advice（通知）:** 典型的 Advice 类型有 before、after 和 around，分别表示在目标方法执行之前、执行后和完全替代目标方法执行的代码。

**Joint point（连接点）:** 程序中可能作为代码注入目标的特定的点和入口。

**Pointcut（切入点）:** 告诉代码注入工具，在何处注入一段特定代码的表达式。

**Aspect（切面）:** Pointcut 和 Advice 的组合看做切面。

**Weaving（织入）:** 注入代码（advices）到目标位置（joint points）的过程。

下面这张图简要总结了一下上述这些概念。

![image-20190319232242121](https://ws3.sinaimg.cn/large/006tKfTcly1g18irb9tvxj313e0os40f.jpg)



## JavaAssist/ASM

字节码操作工具(待补充)

# Javapoet

java 源文件生成工具(待补充)

[Demo](https://github.com/charliecha/AopDemo)

