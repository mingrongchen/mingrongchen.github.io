---
title: 深入理解Java抽象类和接口
categories: 
  - Java
tags:
  - Java基础
  - Java面向对象
about: 无
date: 2021-02-08 11:17:36
---

# 深入理解Java抽象类和接口

<!--more-->

对于面向对象编程来说，抽象是它的一大特征之一。在Java中，可以通过两种形式来体现OOP的抽象：接口和抽象类。

## 一、抽象类

抽象类和抽象方法必须使用abstract关键字进行修饰。

使用方法：使用extends关键字。

```java
public abstract class TestAbstractDoor {

    public abstract void open();

    public abstract void close();

}
```

包含抽象方法的类称为抽象类，除了定义抽象方法，其它和普通类一样，同样可以拥有成员变量和普通的成员方法。注意，抽象类和普通类的主要有三点区别：

  1）抽象方法必须为public或者protected（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public。

  2）抽象类可以理解成在普通类的基础上拓展定义抽象方法，提供一种模板，由于抽象类是“半成品”，所以不能用来创建对象，即不能被实例化；但可以通过向上转型实例化，参考代理模式。

  3）如果一个类继承于一个抽象类，则子类必须实现父类的抽象方法。如果子类没有实现父类的抽象方法，则必须将子类也定义为为abstract类。若子类没有完全实现父类所有抽象方法，则该子类也必须定为抽象类。

  在其他方面，抽象类和普通的类并没有区别。

## 二、接口

接口泛指指供别人调用的方法或者函数，它是对行为的抽象。

使用方法：使用implements关键字。

```java
public interface TestInterface {

    public void beforeHandle();
    public void dealHandle();
    public void afterHandle();

}
```

接口可以含有变量和方法（且方法不能有具体实现），变量会被隐式地指定为public static final变量（只能是public static final变量，否则编译报错），方法会被隐式地指定为public abstract方法且只能是public abstract方法，即接口中的方法必须全为抽象方法。允许一个类遵循多个特定的接口。

- 非抽象类遵循了某个接口，就必须实现该接口中的所有方法。
- 抽象类遵循了某个接口，可以不实现该接口中的抽象方法。

## 三、抽象类和接口的区别

### 1、语法层面的区别

1）方法：抽象类可以提供成员方法的实现细节，而接口中只能存在public abstract 方法
2）成员变量：抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是public static final类型的
3）静态代码块和静态方法：接口中不能含有静态代码块以及静态方法，而抽象类可以有静态代码块和静态方法
4）一个类只能继承一个抽象类，而一个类却可以实现多个接口。

### 2、设计层面的区别

1）抽象类是对一种事物的抽象、对类抽象，而接口是对行为的抽象。**抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象**。

eg：例如，飞机和鸟都具有飞的属性，可以设计一个接口Fly，包含Fly()方法。如果有不同类别的战斗机，可以继承飞机抽象类。

总结：**继承是一个“是不是”的关系，而接口是“有没有”的关系**。如果一个类继承了一个抽象类，那么子类也是属于这个抽象类的一种。而接口只是约束行为（方法）。

2）抽象类可以作为很多子类的父类，它是一种模板式设计；而接口是一种行为规范，它是一种辐射式设计。

模板式设计：如果两个B、C子类继承C抽象类，B、C子类公共部分就是模板A，如果公共部分需要改动，只需要改动模板A了，不需要重新改动B、C子类。

辐射式设计：比如某个电梯都装了某种报警器，一旦要更新报警器，就必须全部更新。也就是说对于抽象类，如果需要添加新的方法，可以直接在抽象类中添加具体的实现，子类可以不进行变更；而对于接口则不行，如果接口进行了变更，则所有实现这个接口的类都必须进行相应的改动。

eg：

```java
// 定义抽象类
public abstract class TestAbstractDoor {

    public abstract void open();

    public abstract void close();

}

// 定义接口
public interface TestInterfaceDoor {
    public abstract void open();

    public abstract void close();

}

```

但是现在如果我们需要门具有报警alarm( )的功能，那么该如何实现？下面提供两种思路：

　　1）将这三个功能都放在抽象类里面，但是这样一来所有继承于这个抽象类的子类都具备了报警功能，但是有的门并不一定具备报警功能；

　　2）将这三个功能都放在接口里面，需要用到报警功能的类就需要实现这个接口中的open( )和close( )，也许这个类根本就不具备open( )和close( )这两个功能，比如火灾报警器。

​    从这里可以看出， Door的open() 、close()和alarm()根本就属于两个不同范畴内的行为，open()和close()属于门本身固有的行为特性，而alarm()属于延伸的附加行为。因此最好的解决办法是单独将报警设计为一个接口，包含alarm()行为,Door设计为单独的一个抽象类，包含open和close两种行为。再设计一个报警门继承Door类和实现Alarm接口。

```java
// 抽象类衍生接口
public interface TestInterfaceAlarm {
    public void alarm();
}

// 具体类的具体实现
public class TestAlarmDoor extends TestAbstractDoor implements TestInterfaceAlarm{

    @Override
    public void alarm() {
        // TODO Auto-generated method stub
        
    }

    @Override
    public void open() {
        // TODO Auto-generated method stub
        
    }

    @Override
    public void close() {
        // TODO Auto-generated method stub
        
    }

}

```

