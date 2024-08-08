# 设计模式

[TOC]

## 一、软件设计原则

+ 开闭原则：对扩展开放，对修改关闭
+ 里氏代换原则：任何基类（父类）可以出现的地方，子类一定可以出现。（子类可以扩展父类，但是不要重写父类的方法）
+ 依赖倒转原则：依赖于抽象，不能依赖于具体实现
+ 接口隔离原则：客户端不应该被迫依赖于它不使用的方法；一个类对另一个类的依赖应该建立在最小的接口上。
+ 迪米特法则：如果两个软件实体无须直接通信，那么就不应当发生直接的相互调用，可以通过第三方转发该调用。其目的是降低类之间的耦合度，提高模块的相对独立性。
+ 合成复用原则：尽量先使用组合或者聚合等关联关系来实现，其次才考虑使用继承关系来实现。

> 继承复用虽然有简单和易实现的优点，但它也存在以下缺点：
>
> 1. 继承复用破坏了类的封装性。因为继承会将父类的实现细节暴露给子类，父类对子类是透明的，所以这种复用又称为“白箱”复用。
> 2. 子类与父类的耦合度高。父类的实现的任何改变都会导致子类的实现发生变化，这不利于类的扩展与维护。
> 3. 它限制了复用的灵活性。从父类继承而来的实现是静态的，在编译时已经定义，所以在运行时不可能发生变化。
>
>
> 采用组合或聚合复用时，可以将已有对象纳入新对象中，使之成为新对象的一部分，新对象可以调用已有对象的功能，它有以下优点：
>
> 1. 它维持了类的封装性。因为成分对象的内部细节是新对象看不见的，所以这种复用又称为“黑箱”复用。
> 2. 对象间的耦合度低。可以在类的成员位置声明抽象。
> 3. 复用的灵活性高。这种复用可以在运行时动态进行，新对象可以动态地引用与成分对象类型相同的对象。

+ 单一职责原则：⼀个类只负责⼀个功能领域中的相应职责



## 二、单例模式

### 2.1 什么是单例模式

单例模式属于创建型模式，一个单例类在任何情况下都只存在一个实例， 构造方法必须是私有的、由自己创建一个静态变量存储实例，对外提供一个静态公有方法获取实例。 优点是内存中只有⼀个实例，减少了开销，尤其是频繁创建和销毁实例的 情况下并且可以避免对资源的多重占用。缺点是没有抽象层，难以扩展， 与单一职责原则冲突（单例类既负责自己的实例管理又负责了业务逻辑）。



### 2.2 单例模式的几种写法

#### 2.2.1 饿汉式，线程安全

饿汉式单例模式，类⼀加载就创建对象，这种方式比较常用， 但容易产生垃圾对象，浪费内存空间。 

+ 优点：线程安全，没有加锁，执行效率较高 
+ 缺点：不是懒加载，类加载时就初始化，浪费内存空间

饿汉式单例是如何保证线程安全的呢？它是基于类加载机制避免了多线程 的同步问题，但是如果类被不同的类加载器加载就会创建不同的实例。

```java
public class Singleton {
	// 1、私有化构造⽅法
	private Singleton(){}
 	// 2、定义⼀个静态变量指向⾃⼰类型
 	private final static Singleton instance = new Singleton();
 	// 3、对外提供⼀个公共的⽅法获取实例
 	public static Singleton getInstance() {
 		return instance;
 	}
}
```

#### 2.2.2 懒汉式，线程不安全 -> DCL

**线程不安全**

这种方式在单线程下使用没有问题，对于多线程是无法保证单例的，这里列出来是为了和后面使用锁保证线程安全的单例做对比。 

+ 优点：懒加载 

+ 缺点：线程不安全

```java
public class Singleton {
	// 1、私有化构造⽅法
	private Singleton(){ }
 	// 2、定义⼀个静态变量指向⾃⼰类型
 	private static Singleton instance;
 	// 3、对外提供⼀个公共的⽅法获取实例
 	public static Singleton getInstance() {
 		// 判断为 null 的时候再创建对象
 		if (instance == null) {
 			instance = new Singleton();
 		}
 		return instance;
 	}
}

```

**线程安全**

通过 synchronized 关键字加锁保证线程 安全， synchronized 可以添加在方法上面，也可以添加在代码块上面，这里演示添加在方法上面，存在的问题是 每一次调用 getInstance 获取实例时 都需要加锁和释放锁，这样是非常影响性能的。 

+ 优点：懒加载，线程安全 
+ 缺点：效率较低

```java
public class Singleton {
	// 1、私有化构造⽅法
 	private Singleton(){ }
 	// 2、定义⼀个静态变量指向⾃⼰类型
 	private static Singleton instance;
 	// 3、对外提供⼀个公共的⽅法获取实例
 	public synchronized static Singleton getInstance() {
 		if (instance == null) {
 			instance = new Singleton();
 		}
 		return instance;
 	}
}
```

**双重检查锁（DCL）**

+ 优点：懒加载，线程安全，效率较高 
+ 缺点：实现较复杂

```java
public class Singleton {
 	// 1、私有化构造⽅法
 	private Singleton() {}
 	// 2、定义⼀个静态变量指向⾃⼰类型
 	private volatile static Singleton instance;
 	// 3、对外提供⼀个公共的⽅法获取实例
 	public static Singleton getInstance() {
 		// 第⼀重检查是否为 null
 		if (instance == null) {
 			// 使⽤ synchronized 加锁
 			synchronized (Singleton.class) {
 				// 第⼆重检查是否为 null
				if (instance == null) {
 					// new 关键字创建对象不是原⼦操作
 					instance = new Singleton();
 				}
 			}
 		}
 		return instance;
 	}
}
```

这里使用双重锁判断，主要是出于性能上的考虑，之前对于所有调用 getInstance() 方法的情况都使用了 synchronized 加锁，无论是否是第一次创建实例，对性能影响很大。使用双重锁，可以让除了第一次创建锁时，并发通过第一个判断的线程之外的所有线程都不需要获取锁，性能有很大提升。

第二次判断的主要目的就是为了防止第一次创建时，同时通过第一次判断的多个线程，创建多个实例的请款。

双重检查锁中使用 volatile ，主要是为了防止指令重排：

若不加volatile则多线程情况下，对象初始化和引用指向地址这步骤可能发生指令重排，导致某个线程得到了单例，但是这个单例是没有完全初始化的。当我们在引用变量上面添加 volatile 关键字以后，会通过在创建对象指令的前后添加内存屏障来禁止指令重排序，就可以避免这个问题。



#### 2.2.3 静态内部类实现

+ 优点：懒加载，线程安全，效率较高，实现简单

```java
public class Singleton {
 	// 1、私有化构造⽅法
 	private Singleton() {}
 	// 2、对外提供获取实例的公共⽅法
 	public static Singleton getInstance() {
 		return InnerClass.INSTANCE;
	}
 	// 定义静态内部类
 	private static class InnerClass{
 		private final static Singleton INSTANCE = new Singleton();
 	}
}
```

**为什么是懒加载**

在《虚拟机规范》只有主动引用的情况需要对类立即进行初始化，静态内部类属于被动引用的情况，当 getInstance()方法被调用时，内部类才在 Singleton 的运行时常量池里，把符号引用替换为直接引用，这时静态对象 INSTANCE 也真正被创 建，然后再被 getInstance()方法返回出去。

**为什么线程安全**

虚拟机会保证一个类的初始化方法在多线程环境中被正确地加锁。

#### 2.2.4 枚举单例

+ 优点：简单，高效，线程安全，可以避免通过反射破坏枚举单例

**枚举类无法通过反射创建实例**

```java
public enum Singleton {
 	INSTANCE;
 	public void doSomething(String str) {
 		System.out.println(str);
 	}
}
```

从枚举的反编译结果可以看到，INSTANCE 被 **static final 修饰**，所以可以 通过类名直接调用，并且创建对象的实例是在**静态代码块**中创建的，因为 static 类型的属性会在类被加载之后被初始化，当⼀个 Java 类第一次被真正使用到的时候静态资源被初始化、Java 类的加载和初始化过程都是线程安全的，所以创建⼀个 enum 类型是线程安全的。



## 三、策略模式

### 3.1 什么是策略模式

该模式定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的变化不会影响使用算法的客户。策略模式属于对象行为模式，它通过对算法进行封装，**把使用算法的责任和算法的实现分割开来**，并委派给不同的对象对这些算法进行管理。

### 3.2 策略模式的角色

* 抽象策略（Strategy）类：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口。
* 具体策略（Concrete Strategy）类：实现了抽象策略定义的接口，提供具体的算法实现或行为。
* 环境（Context）类：持有一个策略类的引用，最终给客户端调用。

### 3.3 项目实现 以及案例

P：在我们的项目中由于高速实时流量的计算分为两种情况：

+ 一：正常情况下是通过ETC、MTC来计算高速流量的。
+ 二：在高速免费的时候我们需要通过 门架数据 来计算高速流量。

当时我们接手的时候，发现这两种算法是人工切换的，后端服务器部署了两套项目代码，每当高速免费的时候，人工停掉一个项目，换成另一个。

A：我们接手之后，觉得这个操作太低效了，就将这两种算法抽象了一下，做成了一个交通流量算法的接口，然后两种算法的实现类对应了两种情况。

通过一个Context类的构造方法，来注入我们对应的交通流计算算法类的实现类，来动态的切换不同的算法实现，通过前端的参数我们判断使用那个算法。

R：这样使得我们不用再手动的切换代码，提高了效率。并且算法的实现与调用相解耦，每个算法有自己的实现类不是都在service层堆叠在一起。

**代码如下：**

定义交通流量计算算法的接口

```java
public interface TrafficflowAlgorithm {
    Response compute();
}
```

定义具体算法（Concrete Strategy）：具体的算法

```java
//日常计算算法
public class DailyTrafficflowAlgorithm implements TrafficflowAlgorithm {

    public Response compute() {
        System.out.println("日常计算算法");
        return new Response();
    }
}

//高速免费计算算法
public class FreeTrafficflowAlgorithm implements TrafficflowAlgorithm {

    public Response compute() {
        System.out.println("高速免费计算算法");
        return new Response();
    }
}

```

定义环境角色（Context）：用于连接上下文

```java
public class TrafficflowAlgorithmContext {                        
    //持有抽象策略角色的引用                              
    private TrafficflowAlgorithm trafficflowAlgorithm;                 
                                               
    public TrafficflowAlgorithmContext(TrafficflowAlgorithm trafficflowAlgorithm) {       
        this.trafficflowAlgorithm = trafficflowAlgorithm;              
    }                                          
                                               
    //向客户展示促销活动                                
    public Response compute(){                
        return trafficflowAlgorithm.compute();                       
    }                                          
}                                              
```



## 四、责任链模式

### 4.1 什么是责任链模式

为了避免请求发送者与多个请求处理者耦合在一起，将所有请求的处理者通过前一对象记住其下一个对象的引用而连成一条链；当有请求发生时，可将请求沿着这条链传递，直到有对象处理它为止。其实有点类似于链表。

比较常见的springmvc中的拦截器，web开发中的filter过滤器

### 4.2 角色

* 抽象处理者（Handler）角色：定义一个处理请求的接口，包含抽象处理方法和一个后继连接。
* 具体处理者（Concrete Handler）角色：实现抽象处理者的处理方法，判断能否处理本次请求，如果可以处理请求则处理，否则将该请求转给它的后继者。
* 客户类（Client）角色：创建处理链，并向链头的具体处理者对象提交请求，它不关心处理细节和请求的传递过程。

### 4.3 代码实现

类图

![image-20240808141329100](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408081413229.png)

**代码：**

抽象处理者

```java
package com.itheima.designpattern.chain;

/**
 * 抽象处理者
 */
public abstract class Handler {

    protected Handler handler;

    public void setNext(Handler handler) {
        this.handler = handler;
    }

    /**
     * 处理过程
     * 需要子类进行实现
     */
    public abstract void process(OrderInfo order);
}
```

订单信息类：

```java
package com.itheima.designpattern.chain;


import java.math.BigDecimal;

public class OrderInfo {

    private String productId;
    private String userId;

    private BigDecimal amount;

    public String getProductId() {
        return productId;
    }

    public void setProductId(String productId) {
        this.productId = productId;
    }

    public String getUserId() {
        return userId;
    }

    public void setUserId(String userId) {
        this.userId = userId;
    }

    public BigDecimal getAmount() {
        return amount;
    }

    public void setAmount(BigDecimal amount) {
        this.amount = amount;
    }
}
```



具体处理者：

```java
/**
 * 订单校验
 */
public class OrderValidition extends Handler {

    @Override
    public void process(OrderInfo order) {
        System.out.println("校验订单基本信息");
        //校验
        handler.process(order);
    }

}

/**
 * 补充订单信息
 */
public class OrderFill extends Handler {
    @Override
    public void process(OrderInfo order) {
        System.out.println("补充订单信息");
        handler.process(order);
    }

}

/**
 * 计算金额
 */
public class OrderAmountCalcuate extends Handler {
    @Override
    public void process(OrderInfo order) {
        System.out.println("计算金额-优惠券、VIP、活动打折");
        handler.process(order);
    }

}

/**
 * 订单入库
 */
public class OrderCreate extends Handler {
    @Override
    public void process(OrderInfo order) {
        System.out.println("订单入库");
    }
}

```

客户类：

```java
public class Application {

    public static void main(String[] args) {
        //检验订单
        Handler orderValidition = new OrderValidition();
        //补充订单信息
        Handler orderFill = new OrderFill();
        //订单算价
        Handler orderAmountCalcuate = new OrderAmountCalcuate();
        //订单落库
        Handler orderCreate = new OrderCreate();

        //设置责任链路
        orderValidition.setNext(orderFill);
        orderFill.setNext(orderAmountCalcuate);
        orderAmountCalcuate.setNext(orderCreate);

        //开始执行
        orderValidition.process(new OrderInfo());
    }

}
```

**优点**

* 降低了对象之间的耦合度

* 增强了系统的可扩展性，可以根据需要增加新的请求处理类，满足开闭原则。

* 责任链简化了对象之间的连接

* 责任分担

  每个类只需要处理自己该处理的工作，不能处理的传递给下一个对象完成，明确各类的责任范围，符合类的单一职责原则。

**缺点：**

* 不能保证每个请求一定被处理。由于一个请求没有明确的接收者，所以不能保证它一定会被处理，该请求可能一直传到链的末端都得不到处理。
* 职责链建立的合理性要靠客户端来保证，增加了客户端的复杂性，可能会由于职责链的错误设置而导致系统出错，如可能会造成循环调用。