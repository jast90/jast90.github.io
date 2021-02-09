---
layout: post
title: 'Java 动态代理源码分析'
description: 'Java 动态代理源码分析'
date: '2019-8-1 15:26:00'
author: Jast
---

## 接口及代理类

### 接口

```java
package cn.jastz.java.reflect.dynamic.proxy;

/**
 * @author zhiwen
 */
public interface Foo {
    void bar();
    void rtest();
    void rtest1();
    void rtest2();
}
```

### 代理类

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package com.sun.proxy;

import cn.jastz.java.reflect.dynamic.proxy.Foo;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.lang.reflect.UndeclaredThrowableException;

public final class $Proxy0 extends Proxy implements Foo {
    private static Method m1;
    private static Method m4;
    private static Method m5;
    private static Method m2;
    private static Method m6;
    private static Method m3;
    private static Method m0;

    public $Proxy0(InvocationHandler var1) throws  {
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

    public final void rtest1() throws  {
        try {
            super.h.invoke(this, m4, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final void rtest2() throws  {
        try {
            super.h.invoke(this, m5, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
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

    public final void bar() throws  {
        try {
            super.h.invoke(this, m6, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final void rtest() throws  {
        try {
            super.h.invoke(this, m3, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
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
            m4 = Class.forName("cn.jastz.java.reflect.dynamic.proxy.Foo").getMethod("rtest1");
            m5 = Class.forName("cn.jastz.java.reflect.dynamic.proxy.Foo").getMethod("rtest2");
            m2 = Class.forName("java.lang.Object").getMethod("toString");
            m6 = Class.forName("cn.jastz.java.reflect.dynamic.proxy.Foo").getMethod("bar");
            m3 = Class.forName("cn.jastz.java.reflect.dynamic.proxy.Foo").getMethod("rtest");
            m0 = Class.forName("java.lang.Object").getMethod("hashCode");
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}

```

### 为什么不能代理类？

因为生成的代理类是继承java.lang.reflect.Proxy类、实现被代理接口，因为java是单继承，所以可想而知是不能代理类的，并且运行的时候有做相关的校验的。

## 用途

- [动态获取配置文件](https://www.cnblogs.com/techyc/p/3455950.html)
- mybatis动态代理使用：通过接口就可以返回数据

## 编码步骤

- 定义被代理接口、被代理接口实现类[可选的]
- 定义 InvocationHandler
- 创建代理对象：Proxy.newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)

## 源码分析

TODO

## 动态代理类class文件生成

```java
sun.misc.ProxyGenerator.generateProxyClass(java.lang.String, java.lang.Class<?>[], int)  
sun.misc.ProxyGenerator.generateClassFile() 
```

需要生成代理类class文件需要添加如下配置：

```
System.getProperties().put(
                "sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
```

## 和设计模式——代理模式的关系

TODO
## 参考

1.[JDK动态代理源码分析](https://zhuanlan.zhihu.com/p/29188162)