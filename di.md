# 理解DIP、IoC、DI以及DI框架

## 摘要

面向对象设计（OOD）有助于我们开发出高性能、易扩展以及易复用的程序。其中，OOD有一个重要的思想那就是依赖倒置原则（DIP），并由此引申出IoC、DI以及DI框架等概念。通过本文我们将一起学习这些概念，并理清他们之间微妙的关系。

## 前言

对于大部分小菜来说，当听到大牛们高谈DIP、IoC、DI以及DI框架等名词时，有没有瞬间石化的感觉？其实，这些“高大上”的名词，理解起来也并不是那么的难，关键在于入门。只要我们入门了，然后循序渐进，假以时日，自然水到渠成。

好吧，我们先初略了解一下这些概念。

**依赖倒置原则（DIP）：**一种软件架构设计的原则（抽象概念）。

**控制反转（IoC）：**一种反转流、依赖和接口的方式（DIP的具体实现方式）。

**依赖注入（DI）：**IoC的一种实现方式，用来反转依赖（IoC的具体实现方式）。

**DI框架：**依赖注入的**框架**，用来映射依赖，管理对象创建和生存周期（DI框架）。

哦！也许你正为这些陌生的概念而伤透脑筋。不过没关系，接下来我将为你一一道破这其中的玄机。

## 依赖倒置原则（DIP）

在讲概念之前，我们先看生活中的一个例子。

![img](https://images.cnblogs.com/cnblogs_com/liuhaorain/355410/o_O_%e6%9c%aa%e6%a0%87~1_%e5%89%af%e6%9c%ac.jpg)

​                                                                             图1  ATM与银行卡

相信大部分取过钱的朋友都深有感触，只要有一张卡，随便到哪一家银行的ATM都能取钱。在这个场景中，ATM相当于高层模块，而银行卡相当于低层模块。ATM定义了一个插口（接口），供所有的银行卡插入使用。也就是说，ATM不依赖于具体的哪种银行卡。它只需定义好银行卡的规格参数（接口），所有实现了这种规格参数的银行卡都能在ATM上使用。现实生活如此，软件开发更是如此。**依赖倒置原则，它转换了依赖，高层模块不依赖于低层模块的实现，而低层模块依赖于高层模块定义的接口**。通俗的讲，就是高层模块定义接口，低层模块负责实现。

> Bob Martins对DIP的定义：
>
> *高层模块不应依赖于低层模块，两者应该依赖于抽象。*
>
> *抽象不不应该依赖于实现，实现应该依赖于抽象。*

如果生活中的实例不足以说明依赖倒置原则的重要性，那下面我们将通过软件开发的场景来理解为什么要使用依赖倒置原则。

**场景一 依赖无倒置（低层模块定义接口，高层模块负责实现）**

![image-20200511180112518](upload\image-20200511180112518.png)

从上图中，我们发现高层模块的类依赖于低层模块的接口。因此，低层模块需要考虑到所有的接口。如果有新的低层模块类出现时，高层模块需要修改代码，来实现新的低层模块的接口。这样，就破坏了开放封闭原则。

**场景二 依赖倒置（高层模块定义接口，低层模块负责实现）**

![image-20200511180204255](upload\image-20200511180204255.png)

在这个图中，我们发现高层模块定义了接口，将不再直接依赖于低层模块，低层模块负责实现高层模块定义的接口。这样，当有新的低层模块实现时，不需要修改高层模块的代码。

由此，我们可以总结出使用DIP的优点：

**系统更柔韧：**可以修改一部分代码而不影响其他模块。

**系统更健壮：**可以修改一部分代码而不会让系统崩溃。

**系统更高效：**组件松耦合，且可复用，提高开发效率。

## 控制反转（IoC）

DIP是一种 **软件设计原则**，它仅仅告诉你两个模块之间应该如何依赖，但是它并没有告诉如何做。IoC则是一种 **软件设计模式**，它告诉你应该如何做，来解除相互依赖模块的耦合。**控制反转（IoC），它为相互依赖的组件提供抽象，将依赖（低层模块）对象的获得交给第三方（系统）来控制**，**即依赖对象不在被依赖模块的类中直接通过new来获取**。在图1的例子我们可以看到，ATM它自身并没有插入具体的银行卡（工行卡、农行卡等等），而是将插卡工作交给人来控制，即我们来决定将插入什么样的银行卡来取钱。同样我们也通过软件开发过程中场景来加深理解。

> **软件设计原则：**原则为我们提供指南，它告诉我们什么是对的，什么是错的。它不会告诉我们如何解决问题。它仅仅给出一些准则，以便我们可以设计好的软件，避免不良的设计。一些常见的原则，比如DRY、OCP、DIP等。
>
> **软件设计模式：**模式是在软件开发过程中总结得出的一些可重用的解决方案，它能解决一些实际的问题。一些常见的模式，比如工厂模式、单例模式等等。
>
> 先看一个违反依赖导致的例子：

```java
class Car {

    private Engine engine = new Engine();

    public void start() {
        engine.start();
    }
}


class MyApp {
    public static void main(String[] args) {
        Car car = new Car();
        car.start();
    }
}
```

高层模块的类直接负责创建低层次类Engine的实例，考虑一下，汽车使用的引擎可以是2驱、4驱的。那每次更换引擎，我们是否都要更新Car的代码？

显然，这不是一个良好的设计，组件之间高度耦合，可扩展性较差，它违背了DIP原则。高层模块Car类不应该依赖于低层模块。

那么我们是否可以通过IoC来优化代码呢？答案是肯定的。IoC有2种常见的实现方式：依赖注入和服务定位。其中，依赖注入使用最为广泛。

## 服务定位

An alternative to dependency injection is using a [service locator](https://en.wikipedia.org/wiki/Service_locator_pattern). The service locator design pattern also improves decoupling of classes from concrete dependencies. You create a class known as the *service locator* that creates and stores dependencies and then provides those dependencies on demand.

```java
class ServiceLocator {

    private static ServiceLocator instance = null;

    private ServiceLocator() {}

    public static ServiceLocator getInstance() {
        if (instance == null) {
            synchronized(ServiceLocator.class) {
                instance = new ServiceLocator();
            }
        }
        return instance;
    }

    public Engine getEngine() {
        return new Engine();
    }
}

class Car {

    private Engine engine = ServiceLocator.getInstance().getEngine();

    public void start() {
        engine.start();
    }
}

class MyApp {
    public static void main(String[] args) {
        Car car = new Car();
        car.start();
    }
}
```

Compared to dependency injection:

- The collection of dependencies required by a service locator makes code harder to test because all the tests have to interact with the same global service locator.
- Dependencies are encoded in the class implementation, not in the API surface. As a result, it's harder to know what a class needs from the outside. As a result, changes to `Car` or the dependencies available in the service locator might result in runtime or test failures by causing references to fail.
- Managing lifetimes of objects is more difficult if you want to scope to anything other than the lifetime of the entire app. 

下面我们将深入理解依赖注入（DI），并学会使用。

## 依赖注入（DI）

控制反转（IoC）一种重要的方式，**就是将依赖对象的创建和绑定转移到被依赖对象类的外部来实现**。在上述的实例中，Car类所依赖的对象Engine的创建和绑定是在Car类内部进行的。事实证明，这种方法并不可取。既然，不能在Car类内部直接绑定依赖关系，那么如何将Engine对象的引用传递给Car类使用呢？

依赖注入（DI），**它提供一种机制，将需要依赖（低层模块）对象的引用传递给被依赖（高层模块）对象。**通过DI，我们可以在Car类的外部将Engine对象的引用传递给Car类对象。那么具体是如何实现呢？

### **方法一 构造函数注入**

构造函数函数注入，毫无疑问通过构造函数传递依赖。因此，构造函数的参数必然用来接收一个依赖对象。那么参数的类型是什么呢？具体依赖对象的类型？还是一个抽象类型？根据DIP原则，我们知道高层模块不应该依赖于低层模块，两者应该依赖于抽象。那么构造函数的参数应该是一个抽象类型。我们再回到上面那个问题，**如何将Engine对象的引用传递给Car类使用呢**？

```java
class Car {

    private final Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.start();
    }
}


class MyApp {
    public static void main(String[] args) {
        Engine engine = new Engine();
        Car car = new Car(engine);
        car.start();
    }
}

```

### **方法二 属性注入**

顾名思义，属性注入是通过属性来传递依赖。

```java
class Car {

    private Engine engine;

    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.start();
    }
}

class MyApp {
    public static void main(String[] args) {
        Car car = new Car();
        car.setEngine(new Engine());
        car.start();
    }
}
```

###  **方法三 接口注入**

相比构造函数注入和属性注入，接口注入显得有些复杂，使用也不常见。具体思路是先定义一个接口，包含一个设置依赖的方法。然后依赖类，继承并实现这个接口。

## DI框架

前面所有的例子中，我们都是通过**手动**的方式来创建依赖对象，并将引用传递给被依赖模块。比如：

```
Car car = new Car();
car.setEngine(new Engine());
```

 对于大型项目来说，相互依赖的组件比较多。如果还用手动的方式，自己来创建和注入依赖的话，显然效率很低，而且往往还会出现不可控的场面。正因如此，IoC容器诞生了。IoC容器实际上是一个DI框架，它能简化我们的工作量。它包含以下几个功能：

- 动态创建、注入依赖对象。
- 管理对象生命周期。
- 映射依赖关系。

主要DI框架：

Dagger2

