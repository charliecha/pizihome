# AOP

在软件业，AOP为Aspect Oriented Programming的缩写，意为：[面向切面编程](https://baike.baidu.com/item/%E9%9D%A2%E5%90%91%E5%88%87%E9%9D%A2%E7%BC%96%E7%A8%8B/6016335)，通过[预编译](https://baike.baidu.com/item/%E9%A2%84%E7%BC%96%E8%AF%91/3191547)方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是[OOP](https://baike.baidu.com/item/OOP)的延续，是软件开发中的一个热点，也是[Spring](https://baike.baidu.com/item/Spring)框架中的一个重要内容，是[函数式编程](https://baike.baidu.com/item/%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B/4035031)的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的[耦合度](https://baike.baidu.com/item/%E8%80%A6%E5%90%88%E5%BA%A6/2603938)降低，提高程序的可重用性，同时提高了开发的效率。

AOP 三剑客

| 方法           | 说明              | 原理                                           | 实现                          | 特点                                                         |
| -------------- | ----------------- | ---------------------------------------------- | ----------------------------- | ------------------------------------------------------------ |
| APT            | 注解处理器        | 在编译器通过注解采集信息，生成 Java/Class 文件 | ButterKnife、Glide、EventBus3 | 基于 javac，但是如果需要修改已经存在的代码太麻烦，代码侵入比较强 |
| Aspectj        | 面向切面的框架    | 在编译后修改/生成指定 Class                    | Hugo                          | 功能强大，底层原理利用了 ASM，不够轻量，代码侵入比较少       |
| Javaassist/ASM | 操作 Class 的框架 | 按照 Class文件的格式，解析、修改、生成 Class   | QZone 超级不定                | Javaassist 的 Java API更易于使用；ASM 性能更好               |

静态织入：在编译器、运行前修改 Class，对系统无性能影响。

## APT

