## 1.单例设计模式：singleton

###1.1单例模式定义

给了一个类，只有通过实例化对象之后，才可以操作这个类，但是如果我们在构造方法前加上private，就造成的构造方法私有化，观察如下代码：

```java
class Singleton{
	private Singleton(){}
	public void fun(){
		System.out.println("hello");
	}
}
public class Test{
	public static void main(String[] args){
		Singleton instance = new Singleton();
		instance.fun();
	}
} 
```

编译后会报错：

```java
Test.java:9: 错误: Singleton() 在 Singleton 中是 private 访问控制
		Singleton instance = new Singleton();
		                     ^
1 个错误
```

那怎样修改A这个类，让其能被外部实例化，并且调用fun方法呢？

-   既然构造方法被私有化了，那么我们可以尝试在类的内部创建对象：

```java
class Singleton{
	Singleton instance = new Singleton();
	private Singleton(){}
	public void fun(){
		System.out.println("hello");
	}
}
public class Test{
	public static void main(String[] args){
		Singleton instance = null;
	}
} 
```

上面的操作不会报错。

-   现在instance就是一个普通的类属性，但是类的属性一般只能在实例化对象之后才能调用，有没有方法可以不实例化对象直接使用呢？有的，用static关键字修饰，然后在外部直接访问测试：

```java
class Singleton{
	static Singleton instance = new Singleton();
	private Singleton(){}
	public void fun(){
		System.out.println("hello");
	}
}
public class Test{
	public static void main(String[] args){
		Singleton instance = Singleton.instance;
		instance.fun();
	}
} 
```

编译运行后，顺利输出。

-   但是，我们说类中的属性一般要进行封装，所以要加上private，但是加上private之后，外部又不能直接访问了。此时就需要在类中添加一个getter方法，此方法还需要用static来修饰。

```java
class Singleton{
	private static Singleton instance = new Singleton();

	public static Singleton getInstance(){
		return instance;
	}

	private Singleton(){}
	
	public void fun(){
		System.out.println("hello");
	}
}
public class Test{
	public static void main(String[] args){
		Singleton instance = Singleton.getInstance();
		instance.fun();
	}
} 
```

###1.2单例模式的意以

但是饶了一大圈是为了什么呢？我们首先来看一下，如果要调用多个实例化对象应该怎样写？

```python
public class Test{
	public static void main(String[] args){
		Singleton instance1 = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		Singleton instance3 = Singleton.getInstance();
		Singleton instance4 = Singleton.getInstance();
	}
} 
```

写完之后我们在输出一下这些对象：

```java
System.out.println(instance1);
System.out.println(instance2);
System.out.println(instance3);
System.out.println(instance4);
```

输出结果为：

```java
Singleton@7852e922
Singleton@7852e922
Singleton@7852e922
Singleton@7852e922
```

我们可以看到，这些都是同一个对象。因为类中的实例化对象用static修饰了之后处于公共区域。那么这个对象也了公共的对象。无论外部调用多少次这个类，最终只能产生一个实例化对象。这样的设计就叫做单例设计模式，简称singleton。

但是现在的代码还不够完美，我们还应该在类中的实例化对象中加上final关键字，这样它就变成了常量，才表示一个最终的，唯一的实例化对象：

```java
private static final Singleton INSTANCE = new Singleton(); 
```

如果有一个面试题，让你写出一个单例设计模式的代码，并解释这样做的意义：

```java
class Singleton{
	private static final Singleton INSTANCE = new Singleton();

	public static Singleton getInstance(){
		return INSTANCE;
	}

	private Singleton(){}
	
	public void fun(){
		System.out.println("hello");
	}
}
public class Test{
	public static void main(String[] args){
		Singleton instance = Singleton.getInstance();
		instance.fun();
	}
} 
```

意义：构造方法私有化，在类的内部实例化一个类的对象，然后用static、final修饰这个实例化对象。这样无论外部产生多少次Singleton对象，本质上仅仅是这一个实例化对象。

###1.3单例模式的分类

单例设计模式有两种：饿汉式、懒汉式。

之前我们设计的单例设计模式就属于饿汉式，因为类中的实例化对象是提前定义好的，不管外部用不用。而懒汉式，是在第一次使用的时候（调用getInstance方法的时候）才进行实例化操作，不适用不会实例化。这个怎样实现呢？

```java
class Singleton{
	private static Singleton instance;

	public static Singleton getInstance(){
		if (instance == null){
			instance = new Singleton();
		}
		return instance;
	}

	private Singleton(){}
	
	public void fun(){
		System.out.println("hello");
	}
}
public class Test{
	public static void main(String[] args){
		Singleton instance = Singleton.getInstance();
		instance.fun();
	}
} 
```

但是无需太过了解这个分类，只需要知道他的核心就是要求一个类在系统里面只能有一个实例化对象就够了。

## 2.多例设计模式(理解即可)

就是让一个类产生指定多个实例化对象，比如说定义一个表示一周天数的类，这个类能去7个对象。或者表示一个定义性别的类，那就只有两个对象。

下面就定义一个表示性别的类：

```java
class Singleton{
	private String sex;

	// 定义两个对象
	public static final Singleton MALE = new Singleton("男性");
	public static final Singleton FEMALE = new Singleton("女性");

	// 私有化构造方法
	private Singleton(String sex){
		this.sex = sex;
	}

	// 获取对象	
	public static Singleton getInstance(Integer num){
		if(num == 1){
			return MALE;
		}else if (num == 2){
			return FEMALE;
		}else{
			return null;
		}
	}

	public void printSex(){
		System.out.println(this.sex);
	}
}
public class Test{
	public static void main(String[] args){
		Singleton instance = Singleton.getInstance(1);
		instance.printSex();
	}
}
```

## 3.总结

-   单例设计模式就是一个类只能产生一个实例化对象
-   多例设计模式就是一个类只能产生指定数量的对象
-   无论是单例还是多例，核心就是构造方法的私有化