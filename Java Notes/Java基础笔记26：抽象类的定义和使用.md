本篇文章主要讲解Java抽象类的定义和使用，这是整个应用开发之中最为重要（核心）的部分。此部分主要分为3点，第一点将抽象类的定义语法，第二点学习抽象类的使用限制，第三个看抽象类的实际应用。其中实际应用知识简单介绍一下，具体应用需要在后面学习。

## 1.抽象类的概念

普通类可以直接产生实例化对象，并且在类里面包含有构造方法，普通方法，static方法。而抽象类就是在普通类的结构里面增加抽象方法的组成部分。

普通方法是包含{}的方法，这个表示方法体，有方法体的方法一定可以被类的实例化对象调用，而抽象方法指的是没有方法体的方法，同时还需要用abstract关键字定义。

## 2.抽象类的定义

拥有抽象方法的类一定是抽象类，抽象类也需要用abstract声明。下面定义一个最简单的抽象类：

```java
abstract class A{
	public void print(){
		System.out.println("这是一个抽象类中的普通方法");
	}

	// 这是一个抽象方法
	public abstract void fun();
}
```

然后我们像实例化普通类一样，实例化一个抽象类：

```java
public class Hello{
	public static void main(String [] args){
		A a = new A();
		a.print();
	}
}
```

结果却得到了如下的错误提示：

```java
Hello.java:12: 错误: A是抽象的; 无法实例化
		A a = new A();
		      ^
1 个错误
```

可以发现，我们并不能实例化一个抽象类。为什么？因为抽象类里面有抽象方法，而抽象方法没有方法体，所以类不能调用没有方法体的方法，因此就不能实例化抽象类了。 抽象类如何使用呢？

## 3.抽象类的使用

抽象类有如下两个使用原则：

-   抽象类必要有子类。
-   子类不能是抽象类
-   抽象类的子类必须覆写父类的全部抽象方法，相当于指定了子类继承的一个标准。
-   抽象类的实例化，需要依靠子类完成，采用向上转型的方式处理。

```java
abstract class A{
	public void print(){
		System.out.println("这是一个抽象类中的普通方法");
	}

	// 这是一个抽象方法
	public abstract void fun();
}

class B extends A{
	public void fun(){
		System.out.println("这是子类覆写的父类的一个抽象方法");
	}
}

public class Hello{
	public static void main(String [] args){
		A a = new B();	// 向上转型
		a.fun();
	}
}
```

## 4.抽象类概念总结

-   抽象类定义了子类继承的要求，即子类一定要覆写父类中的抽象方法。
-   抽象类的组成只比普通方法多了一个抽象方法，和一个abstract关键字。
-   抽象类不能够直接实例化对象，需要子类向上转型产生实例化对象。

## 5. 什么时候用抽象类

开发的时候，如果需要明确指定子类覆写的方法，那么需要用抽象类。

虽然一个子类可以继承任意一个普通类，可是从开发的实际要求来讲，普通类不要去继承另外一个普通类，而是去继承一个抽象类。

实际项目中，20%写抽象类，80%写接口。

## 6.抽象类的使用限制

-   由于抽象类中有一些属性，所以抽象方法中一定会有构造方法，其目的就是为属性的初始化，子类实例化的时候，依然满足于限制性父类的构造，在执行子类的构造的情况。
-   抽象类不能用final关键字修饰，因为抽象类必须有子类，但是final定义的类不能有子类，冲突了。
-   外部抽象类不能用static声明，内部抽象类可以用static声明，使用static声明的内部抽象类，相当于一个外部抽象类，继承的时候使用"外部类.内部类"的形式表示类名称。

```java
static abstract class A{

}

class B extends A{

}

public class Hello{
	public static void main(String [] args){
		A a = new B();
	}
}
```

上面这样写会爆出如下的错误：

```java
Hello.java:1: 错误: 此处不允许使用修饰符static
static abstract class A{
                ^
1 个错误
```

但是我们在一个抽象类里面去定义一个用static修饰的内部类就可以被调用

```java
// 外部抽象类不能用static
abstract class A{
	// 内部抽象类可以用static
	static abstract class B{
		public abstract void print();
	}
}

class C extends A.B{
	public void print(){
		System.out.println("子类C覆写内部类");
	}
}

public class Hello{
	public static void main(String [] args){
		A.B ab = new C();
		ab.print();
	}
}
```

-   任何情况下，要执行类中static方法的时候，都可以在没有对象的时候，直接调用，抽象类也是如此。

```java

abstract class A{
	public static void print(){
		System.out.println("hello");
	}
}

public class Hello{
	public static void main(String [] args){
		A.print();
	}
}
```

-   有时抽象类只需要一个特定的系统的系统子类操作，所以可以忽略掉外部子类，也就是说，可以这样写子类的继承：

```java
abstract class A{
	public abstract void print();

	// 内部抽象类子类
	private static class B extends A{
		// 覆写抽象类方法
		public void print(){
			System.out.println("hello");
		}
	}

	// 此方法不受实例化对象的限制
	public static A getInstance(){
		return new B();
	}
}

public class Hello{
	public static void main(String [] args){
		A a = A.getInstance();
		a.print();
	}
}
```

这样的设计在系统类库中比较常见，目的是为用户隐藏我们不需要知道的子类。平时开发不会写出这样的代码，但是需要理解。

还有一个遗留问题：在任何一个类的构造执行之前，所有属性的内容都是对应数据类型的默认值，而子类对构造执行之前，一定先执行父类构造。

