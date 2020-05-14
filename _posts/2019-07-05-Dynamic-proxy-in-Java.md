---
layout: post
title: 'Java中的动态代理'
description: 'Java中的动态代理'
date: '2019-07-05 16:54:00'
author: Jast
---

- 在许多情况下，代理对象在客户端对象和目标对象之间充当中介

- 通常，代理类已经可用作Java字节码，已从代理类的Java源文件编译

- 需要时，代理类的字节码将加载到Java虚拟机中，然后可以实例化代理对象

- 但是，在某些情况下，在运行时动态生成代理类的字节码很有用

- 本单元将介绍在Java中动态生成代理的技术以及这样做的好处

- 首先，让我们展示一个客户端直接与目标对象进行交互

- 假设我们有一个IVehicle接口，如下所示：

```
/**
* Interface IVehicle.
*/
public interface IVehicle {
    public void start();
    public void stop();
    public void forward();
    public void reverse();
    public String getName();
}
```
- 这是一个实现IVehicle接口的Car类：
```
/**
* Class Car
*/
public class Car implements IVehicle {
    private String name;
    public Car(String name) {this.name = name;}
    public void start() {
        System.out.println("Car " + name + " started");
    }
    // stop(), forward(), reverse() implemented similarly.
    // getName() not shown.
}
```

```
/**
* Class Client1.
* Interacts with a Car Vehicle directly.
*/
public class Client1 {
    public static void main(String[] args) {
        IVehicle v = new Car("Botar");
        v.start();
        v.forward();
        v.stop();
    }
}
```
- 没有代理的车辆示例的输出：
```
Car Botar started
Car Botar going forward
Car Botar stopped
```

- 现在让我们让客户端通过代理与目标对象进行交互

- 请记住，代理的主要目的是控制对目标对象的访问，而不是增强目标对象的功能

- 代理可以提供访问控制的方式包括：
    - 同步
    - 认证
    - 远程访问
    - 懒惰的实例化

- 这是我们的VehicleProxy类：
```
/**
* Class VehicleProxy.
*/
public class VehicleProxy implements IVehicle {
    private IVehicle v;
    public VehicleProxy(IVehicle v) {this.v = v;}
    public void start() {
        System.out.println("VehicleProxy: Begin of start()");
        v.start();
        System.out.println("VehicleProxy: End of start()");
    }
    // stop(), forward(), reverse() implemented similarly.
    // getName() not shown.
}
```

```
/**
* Class Client2.
* Interacts with a Car Vehicle through a VehicleProxy.
*/
public class Client2 {
    public static void main(String[] args) {
        IVehicle c = new Car("Botar");
        IVehicle v = new VehicleProxy(c);
        v.start();
        v.forward();
        v.stop();
    }
}
```
- 带代理的车辆示例的输出：
```
VehicleProxy: Begin of start()
Car Botar started
VehicleProxy: End of start()
VehicleProxy: Begin of forward()
Car Botar going forward
VehicleProxy: End of forward()
VehicleProxy: Begin of stop()
Car Botar stopped
VehicleProxy: End of stop()
```

- Java 1.3支持创建动态代理类和实例

- 动态代理类是一个实现创建类时在运行时指定的接口列表的类

- 代理接口是由代理类实现的接口

- 代理实例是代理类的实例

- 每个代理实例都有一个关联的调用处理程序对象，该对象实现了InvocationHandler接口

- 通过其代理接口之一对代理实例进行方法调用将被调度到实例调用处理程序的invoke（）方法

- 使用新的java.lang.reflect.Proxy类创建代理类

- 代理类是java.lang.reflect.Proxy的公共，最终，非抽象子类

- 未指定代理类的非限定名称。但是，以字符串“$Proxy”开头的类名空间应该保留给代理类。

- 代理类完全实现其创建时指定的接口

- 由于代理类实现了在创建时指定的所有接口，因此在其Class对象上调用getInterfaces（）将返回一个包含相同接口列表的数组（按照创建时指定的顺序）