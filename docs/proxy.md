---

---

# 动态代理

Java代码中，**动态代理**是通过调用 Proxy 类中下面方法实现的，

```
public static Object newProxyInstance(ClassLoader loader,
                                      Class<?>[] interfaces,
                                      InvocationHandler h)
```

## 原理

其原理是通过传入的接口参数，动态生成一个继承自 Proxy类，并实现所有传入接口的子类。

newProxyInstance方法的执行流程如下，省略以下非关键步骤：

![proxy](https://ws3.sinaimg.cn/large/006tKfTcly1g1lclvnbegj311p0gpwgi.jpg)

其中：

ProxyGenerator.apply()方法负责根据传入包名，接口集合以及访问修饰符生成集成自 Proxy 并实现所有接口的之类的字节码；

ProxyClassFactory .generateProxyClass()方法负责计算生成之类包名，类名等信息，并负责加载ProxyGenerator生成的字节码，获得 Class 对象；

Proxy.newProxyInstance()方法负责调用ProxyGenerator.apply()获得代理类，并反射调用代理类的构造方法，来构造一个代理类的实例。

## 一个例子：

### 接口类：

```
public interface UserMapper {
    int deleteByPrimaryKey(Integer id);

    int insert(User record);

    int insertSelective(User record);

    User selectByPrimaryKey(Integer id);

    int updateByPrimaryKeySelective(User record);

    int updateByPrimaryKey(User record);
}
```

### 调用ProxyGenerator.generateProxyClass()方法生成字节码的代码：

```
byte[] classFile = ProxyGenerator.generateProxyClass(DEFAULT_CLASS_NAME, interfaces);
```

### 生成的代理类：

```
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.lang.reflect.UndeclaredThrowableException;
import org.charle.proxy.User;
import org.charle.proxy.UserMapper;

public final class $Proxy extends Proxy implements UserMapper {
    private static Method m1;
    private static Method m6;
    private static Method m4;
    private static Method m2;
    private static Method m3;
    private static Method m5;
    private static Method m7;
    private static Method m8;
    private static Method m0;

    public $Proxy(InvocationHandler var1) throws  {
        super(var1);
    }

    public final boolean equals(Object var1) throws  {
        try {
            return (Boolean)super.h.invoke(this, m1, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final User selectByPrimaryKey(Integer var1) throws  {
        try {
            return (User)super.h.invoke(this, m6, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final int insertSelective(User var1) throws  {
        try {
            return (Integer)super.h.invoke(this, m4, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final String toString() throws  {
        try {
            return (String)super.h.invoke(this, m2, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final int insert(User var1) throws  {
        try {
            return (Integer)super.h.invoke(this, m3, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final int deleteByPrimaryKey(Integer var1) throws  {
        try {
            return (Integer)super.h.invoke(this, m5, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final int updateByPrimaryKeySelective(User var1) throws  {
        try {
            return (Integer)super.h.invoke(this, m7, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final int updateByPrimaryKey(User var1) throws  {
        try {
            return (Integer)super.h.invoke(this, m8, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final int hashCode() throws  {
        try {
            return (Integer)super.h.invoke(this, m0, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    static {
        try {
            m1 = Class.forName("java.lang.Object").getMethod("equals", Class.forName("java.lang.Object"));
            m6 = Class.forName("org.charle.proxy.UserMapper").getMethod("selectByPrimaryKey", Class.forName("java.lang.Integer"));
            m4 = Class.forName("org.charle.proxy.UserMapper").getMethod("insertSelective", Class.forName("org.charle.proxy.User"));
            m2 = Class.forName("java.lang.Object").getMethod("toString");
            m3 = Class.forName("org.charle.proxy.UserMapper").getMethod("insert", Class.forName("org.charle.proxy.User"));
            m5 = Class.forName("org.charle.proxy.UserMapper").getMethod("deleteByPrimaryKey", Class.forName("java.lang.Integer"));
            m7 = Class.forName("org.charle.proxy.UserMapper").getMethod("updateByPrimaryKeySelective", Class.forName("org.charle.proxy.User"));
            m8 = Class.forName("org.charle.proxy.UserMapper").getMethod("updateByPrimaryKey", Class.forName("org.charle.proxy.User"));
            m0 = Class.forName("java.lang.Object").getMethod("hashCode");
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}
```

## 生成代理类的原理

说白了就是按照Class 类文件的格式，直接创建一个 Class 文件。

详细原理解析的话，可以参考另一篇文章。

[JDK动态代理之ProxyGenerator生成代理类的字节码文件解析](https://www.jb51.net/article/135597.htm)

Class 文件格式参考

[《Java虚拟机规范》阅读（三）:Class文件格式](<http://www.cnblogs.com/zhuYears/archive/2012/02/07/2340347.html>)

[源代码获取](https://github.com/charliecha/AopDemo)

