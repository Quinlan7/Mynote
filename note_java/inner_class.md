### JAVA内部类

前言：这是韩顺平老师的[java基础课程](https://www.bilibili.com/video/BV1fh411y7R8?p=1&vd_source=88553068aea08e911c13f3696d2bfaa2)的学习笔记，只是对韩顺平老师上课内容的总结

##### 内部类基本介绍

1. 定义

​		一个类的内部又完整的嵌套了另一个类结构。被嵌套的类称为内部类(inner class),嵌套其他类的类称为外部类(outer class)。是我们类的第五大成员[属性、方法、构造器、代码块、内部类]，内部类最大的特点就是可以直接访问私有属性，并且可以体现类与类之间的包含关系，注意:内部类是学习的难点，同时也是重点，后面看底层源码时，有大量的内部类.

2. 分类

> 定义在外部类的局部位置上（比如方法内）
>
> > （1）局部内部类（有类名）
> >
> > （2）**匿名内部类**（没有类名）
>
> 定义再外部类的成员位置上
>
> > （1）成员内部类（没用static修饰）
> >
> > （2）静态内部类（使用static修饰）

##### 局部内部类

1. 细节

+ 可以直接访问外部类的所有成员，包含私有的
+ 只能用final修饰
+ 外部类中访问局部内部类，访问方式：创建对象再访问
+ 外部其他类无法访问局部内部类
+ 外部类和局部内部类成员重命时，遵循就近原则，想要在局部内部类中访问外部类成员则可以使用（外部类名.this.成员）去访问

2. 使用

```java
class Outer02 {//外部类
	private int n1 = 100;
	private void m2() {
		System.out.println("Outer02 m2()");
	}//私有方法
    
	public void m1() {//方法
        
		//1.局部内部类是定义在外部类的局部位置,通常在方法
		//3.不能添加访问修饰符,但是可以使用 final 修饰
		//4.作用域 : 仅仅在定义它的方法或代码块中
		final class Inner02 {//局部内部类(本质仍然是一个类)
            
			//2.可以直接访问外部类的所有成员，包含私有的
			private int n1 = 800;
			public void f1() {
				//5. 局部内部类可以直接访问外部类的成员，比如下面 外部类 n1 和 m2()
				//7. 如果外部类和局部内部类的成员重名时，默认遵循就近原则，如果想访问外部类的成员，
				// 使用 外部类名.this.成员）去访问
				// 解读 Outer02.this 本质就是外部类的对象, 即哪个对象调用了 m1, Outer02.this 就是哪个对象
				System.out.println("n1=" + n1 + " 外部类的 n1=" + Outer02.this.n1);
				System.out.println("Outer02.this hashcode=" + Outer02.this);
				m2();
			}
		}
	//6. 外部类在方法中，可以创建 Inner02 对象，然后调用方法即可
	Inner02 inner02 = new Inner02();
	inner02.f1();
}
}
```

##### 匿名内部类

1. 定义

   ​	（1）本质是类（2）内部类（3）该类没有名字（4）**同时是一个对象**

   说明：匿名内部类是定义在类的局部位置，比如方法中，并且没有类名

   语法：

   ```java
   new 类或接口(参数列表){
   	类体
   }
   ```

2. 使用

```java
class Outer04 { //外部类
	private int n1 = 10;//属性
	public void method() {//方法

        //基于接口的匿名内部类
		//解读
		//1.需求： 想使用 IA 接口,并创建对象
		//2.传统方式，是写一个类，实现该接口，并创建对象
		//3.需求是 Tiger/Dog 类只是使用一次，后面再不使用
		//4. 可以使用匿名内部类来简化开发
		//5. tiger 的编译类型 ? IA
		//6. tiger 的运行类型 ? 就是匿名内部类 Outer04$1
		/*
			我们看底层 会分配 类名 Outer04$1
			class Outer04$1 implements IA {
				@Override
				public void cry() {
					System.out.println("老虎叫唤...");
				}
			}
		*/
		//7. jdk 底层在创建匿名内部类 Outer04$1,立即马上就创建了 Outer04$1 实例，并且把地址返回给 tiger
		//8. 匿名内部类使用一次，就不能再使用
		IA tiger = new IA() {
			@Override
			public void cry() {
				System.out.println("老虎叫唤...");
			}
		};
		System.out.println("tiger 的运行类型=" + tiger.getClass());
		tiger.cry();
	

		//演示基于类的匿名内部类
		
        //分析
		//1. father 编译类型 Father
		//2. father 运行类型 Outer04$2
		//3. 底层会创建匿名内部类
		/*
			class Outer04$2 extends Father{
				@Override
				public void test() {
					System.out.println("匿名内部类重写了 test 方法");
				}
			}
		*/
		//4. 同时也直接返回了 匿名内部类 Outer04$2 的对象
		//5. 注意("jack") 参数列表会传递给 构造器
		Father father = new Father("jack"){
			@Override
			public void test() {
				System.out.println("匿名内部类重写了 test 方法");
			}
		};
		System.out.println("father 对象的运行类型=" + father.getClass());//Outer04$2
		father.test();
        
		//基于抽象类的匿名内部类
		Animal animal = new Animal(){
			@Override
			void eat() {
				System.out.println("小狗吃骨头...");
			}
		};
		animal.eat();
	}
}
interface IA {//接口
	public void cry();
}

class Father {//类
	public Father(String name) {//构造器
		System.out.println("接收到 name=" + name);
	}
	public void test() {//方法
	}
}
abstract class Animal { //抽象类
	abstract void eat();
}
```

3. 细节

+ 匿名内部类的语法比较奇特，因为匿名内部类既是一个类的定义，同时其本身也是一个对象，因此从语法上来看，它既有定义类的特征，也有创建对象的特征

```java
class Outer05 {
	private int n1 = 99;
	public void f1() {
		//创建一个基于类的匿名内部类
		//不能添加访问修饰符,因为它的地位就是一个局部变量
		//作用域 : 仅仅在定义它的方法或代码块中
		Person p = new Person(){
			private int n1 = 88;
			@Override
			public void hi() {
				//可以直接访问外部类的所有成员，包含私有的
				//如果外部类和匿名内部类的成员重名时，匿名内部类访问的话，
				//默认遵循就近原则，如果想访问外部类的成员，则可以使用 （外部类名.this.成员）去访问
				System.out.println("匿名内部类重写了 hi 方法 n1=" + n1 +
" 外部内的 n1=" + Outer05.this.n1 );
				//Outer05.this 就是调用 f1 的 对象
				System.out.println("Outer05.this hashcode=" + Outer05.this);

			}
		};
		p.hi();//动态绑定, 运行类型是 Outer05$1
		//也可以直接调用, 匿名内部类本身也是返回对象
		
		new Person(){
			@Override
			public void hi() {
				System.out.println("匿名内部类重写了 hi 方法,哈哈...");
			}
			@Override
			public void ok(String str) {
				super.ok(str);
			}
		}.ok("jack");
	}
}
class Person {//类
	public void hi() {
		System.out.println("Person hi()");
	}
	public void ok(String str) {
		System.out.println("Person ok() " + str);
	}
}
```

4. 最佳实践

+ 当作实参直接传递，简洁高效(当一个对象只被使用一次，并且需要重写类中的方法的时候使用)

```java
public class InnerClassExercise01 {
	public static void main(String[] args) {
	//当做实参直接传递，简洁高效
	f1(new IL() {
			@Override
			public void show() {
			System.out.println("这是一副名画~~...");
			}
		});

//静态方法,形参是接口类型
public static void f1(IL il) {
il.show();
}
}
//接口
interface IL {
	void show();
}
//类->实现 IL => 编程领域 (硬编码)
class Picture implements IL {
	@Override
	public void show() {
		System.out.println("这是一副名画 XX...");
	}
}

```

##### 成员内部类

1. 定义：

​	成员内部类是定义在外部类的成员位置，并且没有static修饰的。

2. 细节：

+ 可以直接访问外部类的所有成员，包含私有的
+ **可以添加任意访问修饰符（public、protected、默认、private），因为它的地位就是一个成员**
+ 外部类访问成员内部类，访问方式：创建对象，再访问
+ 如果外部类和内部类的成员重名时，内部类访问的话，遵循就近原则，如果想要访问外部类的成员则可以使用（外部类名.this.成员）去访问

3. 使用

   外部其他类访问内部成员类

   **无论什么方式都需要通过外部类的实例来访问内部成员类**，因为这不是一个static类，没有对象就没有这个类（属性）

```java
public class MemberInnerClass01 {
	public static void main(String[] args) {
        
		Outer08 outer08 = new Outer08();
		outer08.t1();
		//外部其他类，使用成员内部类的三种方式
        //无论什么方式都需要通过外部类的实例来访问内部成员类
		//解读
		// 第一种方式
		// outer08.new Inner08(); 相当于把 new Inner08()当做是 outer08 成员
		// 这就是一个语法，不要特别的纠结. 
        Outer08.Inner08 inner08 = outer08.new Inner08();
		inner08.say();
		// 第二方式 在外部类中，编写一个方法，可以返回 Inner08 对象
		Outer08.Inner08 inner08Instance = outer08.getInner08Instance();
		inner08Instance.say();
	}
}
class Outer08 { //外部类
	private int n1 = 10;
	public String name = "张三";
	private void hi() {
		System.out.println("hi()方法...");
	}

	//1.注意: 成员内部类，是定义在外部内的成员位置上
	//2.可以添加任意访问修饰符(public、protected 、默认、private),因为它的地位就是一个成员
	public class Inner08 {//成员内部类
		private double sal = 99.8;
		private int n1 = 66;
		public void say() {
			//可以直接访问外部类的所有成员，包含私有的
			//如果成员内部类的成员和外部类的成员重名，会遵守就近原则. 
            //，可以通过 外部类名.this.属性 来访问外部类的成员
			System.out.println("n1 = " + n1 + " name = " + name + " 外部类的 n1=" + Outer08.this.n1);
			hi();
		}
	}
	//方法，返回一个 Inner08 实例
	public Inner08 getInner08Instance(){
		return new Inner08();
	}
	//写方法
	public void t1() {
		//使用成员内部类
		//创建成员内部类的对象，然后使用相关的方法
		Inner08 inner08 = new Inner08();
		inner08.say();

		System.out.println(inner08.sal);
	}
}

```

##### 静态内部类

1. 定义

   静态内部了时定义在外部类的成员位置，并且由static修饰的。

2. 细节

+ 可以直接访问外部类的所有静态成员，包含私有的，但不能访问非静态成员

3. 使用

​		外部其他类访问静态内部类

```java
public class StaticInnerClass01 {
	public static void main(String[] args) {

        Outer10 outer10 = new Outer10();
		outer10.m1();
		
        //外部其他类 使用静态内部类
		//方式 1
		//因为静态内部类，是可以通过类名直接访问(前提是满足访问权限)
		Outer10.Inner10 inner10 = new Outer10.Inner10();
		inner10.say();
		//方式 2
		//编写一个方法，可以返回静态内部类的对象实例. 
        Outer10.Inner10 inner101 = outer10.getInner10();
		System.out.println("============");
		inner101.say();
		Outer10.Inner10 inner10_ = Outer10.getInner10_();
		System.out.println("************");
		inner10_.say();
	}
}

class Outer10 { //外部类
	private int n1 = 10;
	private static String name = "张三";
	private static void cry() {}
	//Inner10 就是静态内部类
	//1. 放在外部类的成员位置
	//2. 使用 static 修饰
	//3. 可以直接访问外部类的所有静态成员，包含私有的，但不能直接访问非静态成员
	//4. 可以添加任意访问修饰符(public、protected 、默认、private),因为它的地位就是一个成员
	//5. 作用域 ：同其他的成员，为整个类体
	static class Inner10 {
		private static String name = "韩顺平教育";
		public void say() {
			//如果外部类和静态内部类的成员重名时，静态内部类访问的时，
			//默认遵循就近原则，如果想访问外部类的成员，则可以使用 （外部类名.成员）
			System.out.println(name + " 外部类 name= " + Outer10.name);
			cry();
		}
	}
	public void m1() { 
        //外部类---访问------>静态内部类 访问方式：创建对象，再访问
		Inner10 inner10 = new Inner10();
		inner10.say();
	}
	public Inner10 getInner10() {
		return new Inner10();
	}
	public static Inner10 getInner10_() {
	return new Inner10();
	}
}

```

##### 小结

+ 重点是掌握匿名内部类的使用

​		new 类/接口（参数列表）{//。。。}

+ 成员内部类，静态内部类 是放在外部类的成员位置，本质就是一个成员

<br>