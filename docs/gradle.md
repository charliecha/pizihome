# Gradle

## 概念

Gradle是一种构建工具，它抛弃了基于XML的构建脚本，取而代之的是采用一种基于Groovy的内部领域特定语言。

### Gradle构建简介

在Gradle中，有两个基本概念：**项目**和**任务**。请看以下详解：

- **项目**是指我们的构建产物（比如Jar包）或实施产物（将应用程序部署到生产环境）。**一个项目包含一个或多个任务。**
- **任务**是指不可分的最小工作单元，执行构建工作（比如编译项目或执行测试）。

那么，这些概念和Gradle的构建又有什么联系呢？好，**每一次Gradle的构建都包含一个或多个项目**。

下面这张图展示了上面所谈到的这些概念的关系。

![Gradle](http://ww1.sinaimg.cn/mw690/63918611gw1ehqthjdvaoj20eb09v3yv.jpg)

我们能够使用以下配置文件对Gradle的构建进行配置：

- Gradle构建脚本（build.gradle）指定了一个项目和它的任务。
- [Gradle属性文件（gradle.properties）](http://www.gradle.org/docs/current/userguide/build_environment.html#sec:gradle_configuration_properties)用来配置构建属性。
- [Gradle设置文件（gradle.settings）](http://www.gradle.org/docs/current/userguide/build_lifecycle.html#sec:settings_file)对于只有一个项目的构建而言是可选的，如果我们的构建中包含多于一个项目，那么它就是必须的，因为它描述了哪一个项目参与构建。每一个多项目的构建都必须在项目结构的根目录中加入一个设置文件。

你可以在[这篇文章](http://www.gradle.org/docs/current/userguide/tutorial_using_tasks.html)中获得更多关于Gradle构建脚本的信息。

我们继续，下面我们看一下如果使用Gradle插件为构建工作加入新功能。

### 更简短的Gradle插件简介

Gradle的设计理念是，所有有用的特性都由[Gradle插件](http://www.gradle.org/docs/current/userguide/plugins.html)提供，一个Gradle插件能够：

- 在项目中添加新任务
- 为新加入的任务提供默认配置，这个默认配置会在项目中注入新的约定（如源文件位置）。
- 加入新的属性，可以覆盖插件的默认配置属性。
- 为项目加入新的依赖。

Gradle用户手册提供了[一系列标准Gradle插件](http://www.gradle.org/docs/current/userguide/standard_plugins.html)。

在我们为项目加入Gradle插件时，我们可以根据名称或类型来指定Gradle插件。

我们可以将下面这行代码加入到build.gradle文件中，它通过名称指定Gradle插件（这里的名称是foo）：

```
apply plugin: 'foo'
```

另一方面，我们也可以通过类型指定Gradle插件，将下面这行代码加入到build.gradle文件中（这里的类型是com.bar.foo）：

```
apply plugin: 'com.bar.foo'
```

## 创建一个Java项目

我们可以使用Java插件（译注：关于Gradle插件的定义，请查看[第一篇教程](http://blog.jobbole.com/71999/)）来创建一个Java项目，为了做到这点，我们需要把下面这段语句加入到*build.gradle*文件中：

```
apply plugin: 'java'
```

就是这样，现在我们已经创建了一个Java项目。Java插件会在我们的构建中添加一些新的约定（如默认的项目结构），新的任务，和新的属性。
让我们来快速地看一下默认的项目结构。

### Java项目结构

[默认的项目结构](http://www.gradle.org/docs/current/userguide/java_plugin.html#N12119)如下：

- *src/main/java*目录包含了项目的源代码。
- *src/main/resources*目录包含了项目的资源（如属性文件）。
- *src/test/java*目录包含了测试类。
- *src/test/resources*目录包含了测试资源。所有我们构建生成的文件都会在*build*目录下被创建，这个目录涵盖了以下的子目录，这些子目录我们会在这篇教程中提到，另外还有一些子目录我们会放在以后讲解。

- *classes*目录包含编译过的*.class*文件。

- *libs*目录包含构建生成的*jar*或*war*文件。

  ### Java工程中的任务

[Java插件在我们的构建中加入了很多任务](http://www.gradle.org/docs/current/userguide/java_plugin.html#N11EA7)，我们这篇教程涉及到的任务如下：

- *assemble*任务会编译程序中的源代码，并打包生成Jar文件，这个任务不执行单元测试。
- *build*任务会执行一个完整的项目构建。
- *clean*任务会删除构建目录。
- *compileJava*任务会编译程序中的源代码。

我们还可以执行以下命令得到一个可运行任务及其描述的完整列表

```
gradle tasks
```

### 打包我们的项目

我们可以通过使用两个不同的任务来打包项目。
如果我们在命令提示符中执行命令*gradle assemble*，我们可以看到以下输出：

```
> gradle assemble
:compileJava
:processResources 
:classes
:jar
:assemble
 
BUILD SUCCESSFUL
 
Total time: 3.163 secs
```

如果我们在命令提示符中执行命令*gradle build*，我们可以看到以下输出：

```
> gradle build
:compileJava
:processResources 
:classes
:jar
:assemble
:compileTestJava 
:processTestResources 
:testClasses 
:test 
:check 
:build
 
BUILD SUCCESSFUL
 
Total time: 3.01 secs
```

这些命令的输出表明了它们的区别：

- *assemble*任务仅仅执行项目打包所必须的任务集。

- *build*任务执行项目打包所必须的任务集，以及执行自动化测试。这两个命令都会在*build/libs*目录中创建一个*file-java-project.jar*文件。默认创建的Jar文件名称是由这个模版决定的：*[projectname].jar*，此外，项目的默认名称和其所处的目录名称是一致的。因此如果你的项目目录名称是*first-java-project*，那么创建的Jar文件名称就是*first-java-project.jar。*

  #### 配置Jar文件的主类

我们可以对生成的Jar文件的主类进行配置，[使用*Manifest*接口的*attributes()*方法](http://www.gradle.org/docs/current/javadoc/org/gradle/api/java/archives/Manifest.html#attributes%28java.util.Map%29)。换句话说，我们可以使用一个包含键值对的map结构指定加入到*manifest*文件的属性集。

我们能够通过设置*Main-Class*属性的值，指定我们程序的入口点。在我们对*build.gradle*文件进行必要的改动后，代码如下：

```
apply plugin: 'java'
 
jar {
    manifest {
        attributes 'Main-Class': 'net.petrikainulainen.gradle.HelloWorld'
    }
}
```

## 依赖管理

在现实生活中，要创造一个没有任何外部依赖的应用程序并非不可能，但也是极具挑战的。这也是为什么依赖管理对于每个软件项目都是至关重要的一部分。

### 仓库管理简介

本质上说，仓库是一种存放依赖的容器，每一个项目都具备一个或多个仓库。

Gradle支持以下仓库格式：

- Ivy仓库
- [Maven仓库](http://maven.apache.org/guides/introduction/introduction-to-repositories.html)
- [Flat directory仓库](http://gradle.1045684.n5.nabble.com/Flat-Dir-repositories-td1431519.html)

我们来看一下，对于每一种仓库类型，我们在构建中应该如何配置。

### 在构建中加入Ivy仓库

我们可以通过URL地址或本地文件系统地址，将Ivy仓库加入到我们的构建中。

如果想通过URL地址添加一个Ivy仓库，我们可以将以下代码片段加入到*build.gradle*文件中：

```
repositories {
    ivy {
        url "http://ivy.petrikainulainen.net/repo"
    }
}
```

如果想通过本地文件系统地址添加一个Ivy仓库，我们可以将以下代码片段加入到*build.gradle*文件中：

```
repositories {
    ivy {       
        url "../ivy-repo"
    }
}
```

### 在构建中加入Maven仓库

与Ivy仓库很类似，我们可以通过URL地址或本地文件系统地址，将Maven仓库加入到我们的构建中。

如果想通过URL地址添加一个Maven仓库，我们可以将以下代码片段加入到*build.gradle*文件中：

```
repositories {
    maven {
        url "http://maven.petrikainulainen.net/repo"
    }
}
```

如果想通过本地文件系统地址添加一个Maven仓库，我们可以将以下代码片段加入到*build.gradle*文件中：

```
repositories {
    maven {       
        url "../maven-repo"
    }
}
```

在加入Maven仓库时，Gradle提供了三种“别名”供我们使用，它们分别是：

- *mavenCentral()*别名，表示依赖是从*Central Maven 2* 仓库中获取的。
- *jcenter()*别名，表示依赖是从*Bintary’s JCenter Maven* 仓库中获取的。
- *mavenLocal()*别名，表示依赖是从本地的Maven仓库中获取的。

如果我们想要将Central Maven 2 仓库加入到构建中，我们必须在*build.gradle*文件中加入以下代码片段：

```
repositories {
    mavenCentral()
}
```

### 在构建中加入Flat Directory仓库

如果我们想要使用Flat Directory仓库，我们需要将以下代码片段加入到*build.gradle*文件中：

```
repositories {
    flatDir {
        dirs 'lib'
    }
}
```

这意味着系统将在*lib*目录下搜索依赖，同样的，如果你愿意的话可以加入多个目录，代码片段如下：

```
repositories {
    flatDir {
        dirs 'libA', 'libB'
    }
}
```

## **创建二进制发布文件**

[Application插件](http://www.gradle.org/docs/2.0/userguide/application_plugin.html)是一种Gradle插件，让我们可以运行、安装应用程序并用非“fat jar”方式创建二进制发布版本。

还记得我们[在上篇教程中](http://www.petrikainulainen.net/programming/gradle/getting-started-with-gradle-dependency-management/)提到的一个例子吗？在它的build.gradle文件中做一些相应的更改，就可以进行二进制发布了。

1. 移除jar任务的配置。
2. 为项目应用application插件。
3. 对应用程序的主类进行配置，设置mainClassName属性。

在build.gradle文件中作出以上更改后，结果如下（相关部分已经高亮）：

```
apply plugin: 'application'
apply plugin: 'java'
 
repositories {
    mavenCentral()
}
 
dependencies {
    compile 'log4j:log4j:1.2.17'
    testCompile 'junit:junit:4.11'
}
 
mainClassName = 'net.petrikainulainen.gradle.HelloWorld'
```

Application插件在项目中添加了5个任务：

- run任务用以启动应用程序。
- startScripts任务会在build/scripts目录中创建启动脚本，这个任务所创建的启动脚本适用于Windows和*nix操作系统。
- installApp任务会在build/install/[project name]目录中安装应用程序。
- distZip任务用以创建二进制发布并将其打包为一个zip文件。可以在build/distributions目录下找到。
- distTar任务用以创建二进制发布并将其打包为一个tar文件。可以在build/distributions目录下找到。

我们可以通过在项目根目录下运行以下命令：gradle distZip或gradle distTar 创建二进制文件。假设我们创建了一个打包为zip文件的二进制文件，输出如下：

```
> gradle distZip
:compileJava
:processResources
:classes
:jar
:startScripts
:distZip
 
BUILD SUCCESSFUL
 
Total time: 4.679 secs
```

如果将application插件创建的二进制文件解压缩，可以得到以下目录结构：

- bin目录：包括启动脚本。
- lib目录：包括应用程序的jar文件以及它的依赖。

你可以阅读[Gradle Application插件用户指南（](http://www.gradle.org/docs/2.0/userguide/application_plugin.html)[第45章](http://www.gradle.org/docs/2.0/userguide/application_plugin.html)）了解更多关于Application插件信息。

现在，我们可以创建一个几乎能满足所有需求的二进制发布了。

## 创建多项目构建

### 创建一个多项目构建

下一步，我们将创建一个多项目的Gradle构建，包括两个子项目：`app` 和 `core。`初始阶段，先要建立Gradle构建的目录结构。

### 建立目录结构

由于core和app模块都使用Java语言，而且它们都使用Java项目的默认项目布局，我们根据以下步骤建立正确的目录结构：

1. 建立core模块的根目录(core)，并建立以下子目录：
   - `src/main/java`目录包含core模块的源代码。
   - `src/test/java`目录包含core模块的单元测试。
2. 建立app模块的根目录(app)，并建立以下子目录：
   - `src/main/java`目录包含app模块的源代码。
   - `src/main/resources`目录包含app模块的资源文件。

现在，我们已经创建了所需的目录，下一步是配置Gradle构建，先对包含在多项目构建中的项目进行配置。

### 对包含在多项目构建中的项目进行配置

我们可以通过以下步骤，对包含在多项目构建中的项目进行配置：

1. 在根项目的根目录下创建`settings.gradle`文件，一个多项目Gradle构建必须含有这个文件，因为它指明了那些包含在多项目构建中的项目。
2. 确保app和core项目包含在我们的多项目构建中。

我们的`settings.gradle`文件如下：

```
include 'app'
include 'core'
```

### 配置 core 项目

我们可以通过以下步骤对core项目进行配置：

- 1. 在core项目的根目录下创建build.gradle文件。
- 2. 使用Java插件创建一个Java项目。
- 3. 确保core项目从Maven2中央仓库(central Maven2 repository)中获取依赖。
- 4. 声明JUnit依赖(版本4.11)，并使用testCompile配置项，该配置项表明：core项目在它的单元测试被编译前，需要JUnit库。

core项目的build.gradle文件如下：

```
apply plugin: "java"
 
repositories {
	mavenCentral()
}
 
dependencies {
    testCompile "junit:junit:4.11"
}
```

我们继续对app项目进行配置。

### 配置App项目

在配置app项目之前，我们先来快速浏览一下对一些特殊依赖的依赖管理，这些依赖都是同一个多项目构建的一部分，我们将其称之为”项目依赖“。

如果多项目构建拥有项目A和B，同时，项目B的编译需要项目A，我们可以通过在项目B的build.gradle文件中添加以下依赖声明来进行依赖配置。

```
dependencies {
    compile project(":A")
}
```

现在，我们可以按照以下步骤配置app项目：

1. 在app项目的根目录下创建build.gradle文件。
2. 用Java插件创建一个Java项目。
3. 确保app项目从Maven2中央仓库(central Maven2 repository)中获取依赖。
4. 配置所需的依赖，app项目在编译时需要两个依赖：
   - Log4j (version 1.2.17)
   - `core` 模块
5. [创建二进制发布版本](http://www.petrikainulainen.net/programming/gradle/getting-started-with-gradle-creating-a-binary-distribution/)

app项目的build.gradle文件如下：

```
apply plugin: "application"
apply plugin: "java"
 
repositories {
	mavenCentral()
}
 
dependencies {
    compile "log4j:log4j:1.2.17"
    compile project(":core")
}
 
mainClassName = "net.petrikainulainen.gradle.client.HelloWorld"
 
task copyLicense {
    outputs.file new File("$buildDir/LICENSE")
    doLast {
        copy {
            from "LICENSE"
            into "$buildDir"
        }
    }
}
 
applicationDistribution.from(copyLicense) {
    into ""
}
```

### 总结

这篇教程教会了我们三部分内容：

1. 一个多项目构建必须在根项目的根目录下包含`settings.gradle`文件，因为它指明了那些包含在多项目构建中的项目。

2. 如果需要在多项目构建的所有项目中加入公用的配置或行为，我们可以将这项配置加入到根项目的build.gradle文件中(使用`allprojects`)。

3. 如果需要在根项目的子项目中加入公用的配置或行为，我们可以将这项配置加入到根项目的build.gradle文件中(使用`subprojects`)。

   [参考](http://blog.jobbole.com/84471/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)

   