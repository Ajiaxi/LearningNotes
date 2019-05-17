如果不会接口，别说会Java。

## 1.接口的定义

如果一个类之中只是由抽象方法和全局常量组成的，在这种情况下，我们不会把他定义为抽象类，而只会定义为接口。所以所谓的接口就属于一个特殊的抽象类，而且这个类里面只有抽象方法和全局常量。要定义一个接口，使用interface关键字完成。

```java
interface A{
	// 静态常量
	public static final String MSG = "hello";

	// 抽象方法
	public abstract void fun();
}
```

以上就是一个最简单的接口，用interface定义了一个特殊的类，其中包含一个静态常量，一个抽象方法。由于接口有抽象方法，所以不可直接实例化对象。

## 2.接口的使用原则

-   接口必须要有子类，但是一个子类可以使用implements关键字实现多个接口
-   接口的子类(如果不是抽象类)，必须覆写接口中的全部抽象方法。
-   接口的对象可以利用子类对象向上转型进行实例化操作。

举例如下：

```java
interface A{
	// 静态常量
	public static final String MSG = "hello";

	//抽象方法
	public abstract void fun();
}

interface B{
	//静态常量
	public static final String NAME = "reborn";

	//抽象方法
	public abstract void print();
}

// 定义一个类实现两个接口
class C implements A,B{
	public void fun(){
		System.out.println("A接口的抽象方法");
	}

	public void print(){
		System.out.println("B接口的抽象方法");
	}
}

public class Hello{
	public static void main(String[] args){
		C c = new C();

		// 向上转型
		A a = c;
		B b = c;

		a.fun();
		b.print();
	}
}
```

但是，对于子类而言，除了实现接口，还有可能继承抽象类，所以写子类的时候，先写extends继承抽象类，而后在使用implements实现接口。我现在在上面的例子上加一个抽象类：

```java
interface A{
	// 静态常量
	public static final String MSG = "hello";

	//抽象方法
	public abstract void fun();
}

interface B{
	//静态常量
	public static final String NAME = "reborn";

	//抽象方法
	public abstract void print();
}

// 抽象方法
abstract class D{
	public abstract void get();
}

// 定义一个类实现两个接口
class C extends D implements A,B{
	public void fun(){
		System.out.println("A接口的抽象方法");
	}

	public void print(){
		System.out.println("B接口的抽象方法");
	}

	public void get(){
		System.out.println("D抽象方法");
	}
}

public class Hello{
	public static void main(String[] args){
		C c = new C();

		// 向上转型
		A a = c;
		B b = c;
		D d = c;

		a.fun();
		b.print();
		d.get();
	}
}
```

对于接口而言，里面只有全局常量和抽象方法，所以可以不用写abstract或者public static final，并且在方法上是否编写public 结果都是一样的。因为接口里只能有一种访问权限：public。一下两个接口的定义效果是完全相同的。

```java
interface A{
	public static final String MSG = "HELLO";
	public abstract void fun();
}
```

去掉前缀

```java
interface A{
	String MSG = "HELLO";
	void fun();
}
```

结果是一样的。因为接口里面没有写上public，其最终的访问权限也是public，绝对不是default。那么在实现接口的时候，可以直接用public覆盖其中的抽象方法，不能不写public，不写的话就成了default：下面的写法是错误的：

```java
interface A{
	String MSG = "HELLO";
	void fun();
}

class B implements A{
	void fun(){
		System.out.println("A接口的实现");
	}
}
```

报错如下

```bash
Hello.java:7: 错误: B中的fun()无法实现A中的fun()
	void fun(){
	     ^
  正在尝试分配更低的访问权限; 以前为public
1 个错误
```

子类中必须要加上public，才能够实现接口：

```java
interface A{
	String MSG = "HELLO";
	void fun();
}

class B implements A{
	public void fun(){
		System.out.println("A接口的实现");
	}
}

public class Hello{
	public static void main(String[] args){
		B b = new B();
		A a = b;
		a.fun();
	}
}
```

所以，为了防止不熟悉语法的开发者，强烈建议改写什么写全！

## 3.接口的组成

对于接口而言，99%的情况都是写抽象方法，很少有接口只是单纯的定义全局变量。

## 4.接口的继承

一个抽象类只能继承一个抽象类，但是一个接口可以使用extends关键字继承多个接口。但是接口不能继承抽象类。接下来看一下接口的多继承概念。

```java
interface A{
	public abstract void funA();
}

interface B{
	public abstract void funB();
}

// 接口C继承A和B，它有了三个抽象方法
interface C extends A,B { 
	public abstract void funC();
}
```

上面定义了三个接口，C接口同时继承A和B，因此C接口在拥有本身的抽象方法之外，还拥有了另外两个抽象方法，因此定义一个C接口的子类，需要将这三个抽象方法都要覆盖：

```java
interface A{
	public abstract void funA();
}

interface B{
	public abstract void funB();
}

// 接口C继承A和B，它有了三个抽象方法
interface C extends A,B { 
	public abstract void funC();
}

class D implements C{
	public void funA(){
		System.out.println("A接口的方法");
	}
	public void funB(){
		System.out.println("B接口的方法");
	}
	public void funC(){
		System.out.println("C接口的方法");
	}
}

public class Hello{
	public static void main(String[] args){
		D d = new D();

		//向上转型，参数统一
		A a = d;
		B b = d;
		C c = d;
		
		a.funA();
		b.funB();
		c.funC();

	}
}
```

总结：接口能够继承多个接口，子类能够实现多个接口。从继承上来讲，抽象类的限制比接口限制的太多：

-   一个抽象类只能继承一个抽象父类，而接口没有这个限制
-   一个子类只能继承一个抽象类，而接口没有限制。

在JAVA里面，接口主要是解决单继承局限的问题。

##5.内部接口

虽然从接口本身的概念上来讲只能够由全局常量和抽象方法组成，但是在接口内也可以定义普通内部类、抽象内部类、内部接口。比如在接口内定义一个抽象类。有不会这么写程序所以就不举例了，但是

我们还可以在一个接口内用static关键字定义一个静态接口，用了static关键字定义的内部类就是一个外部类，用了static关键字定义的内部接口就是一个外部接口：

```java
interface A{
	public abstract void fun();

	// 用static关键字定义的内部接口是外部接口
	static interface B{
		public abstract void funB();
	}
}

class C implements A.B{
	public void funB(){
		System.out.println("内部接口的funB");
	}
}

public class Hello{
	public static void main(String[] args){
		C c = new C();
		c.funB();
	}
}
```

注意，继承的时候写的是`A.B`的形式.

大部分情况只需要知道内部接口的操作就可以了，服务器开发，J2EE开发中用不到，Android里面才用得到。

## 6.先期总结

接口在实际开发中的三大核心作用：

-   定义在不同层之间的操作标准
-   表示一种操作的能力
-   表示将服务器端的远程方法视图暴露给客服端