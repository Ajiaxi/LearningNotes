-   现在只能先讲解多态的相关概念，以及相关的使用限制。
-   多态性所以来的是方法的覆写。
-   多态性严格来讲有两种描述形式：
    1.  方法的多态性
        -   方法的重载：同一方法名称，会根据传入参数类型和个数的不同而调用不同的方法体。
        -   方法的覆写：同一个方法，会根据子类的不同，而实现不同的功能。
    2.  对象的多态性：指的是发生在继承关系之中，子类和父类之间的转换问题。
        -   向上转型（自动转型）：父类 父类对象 = 子类实例
        -   向下转型（强制完成）：子类 子类对象 = (子类) 父类实例
        -   小范围变大范围自动完成，大范围变小范围强制完成。

## 1.向上转型

```java
class A{
	public void print(){
		System.out.println("A的方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的方法");
	};	
}

public class Hello{
	public static void main(String [] args){
		A a = new B();	//向上转型
		a.print();		//输出：B的方法
	}
}
```

##2.向下转型

```java
class A{
	public void print(){
		System.out.println("A的方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的方法");
	};	
}

public class Hello{
	public static void main(String [] args){
		A a = new B();	//向上转型
		B b = (B) a;	//向下转型
		a.print();		//输出：B的方法
	}
}
```

## 3.转型的意义

-   向上转型的意以：由于所有的子类对象实例都可以自动向上转型，因此它的意以在于参数的统一

```java
class A{
	public void print(){
		System.out.println("A的方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的方法");
	};	
}

class C extends A{
	public void print(){
		System.out.println("C的方法");
	}
}

public class Hello{
	public static void main(String [] args){
		//只要是A类的子类都可以向上转型
		A a1 = new B();	//向上转型
		A a2 = new C(); //向上转型
		a1.print();		//输出：B的方法
		a2.print();		//输出：C的方法
	}
}
```

参数统一之后，还可以调用被子类覆写的方法体。即同一个方法针对于不同的子类，可以有不同的实现。比如说之前的链表里面，最大的问题就是参数没有统一，所以针对于不同的数据类型要编写一个单独的链表供他使用，但是如果参数能够统一的话，就不用如此麻烦了。

-   向下转型的意以：指的是父类调用自己特殊的方法。

比如说下面的代码

```java
class A{
	public void print(){
		System.out.println("A的print方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的print方法");
	}

	public void fun(){
		System.out.println("B的fun方法");
	}
}

public class Hello{
	public static void main(String [] args){
		A a = new B();	//向上转型
		a.fun();		//这样调用会出错，A中无fun方法
	}
}
```

通过向上转型，A的实例化对象找不到子类B中的特殊方法，是无法调用的。于是此时就需要进行向下转型，将父类对象转换为子类对象，就能使用子类的特殊功能了。

```java
class A{
	public void print(){
		System.out.println("A的print方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的print方法");
	}

	public void fun(){
		System.out.println("B的fun方法");
	}
}

public class Hello{
	public static void main(String [] args){
		A a = new B();	//向上转型
		B b = (B) a;	//向下转型
		b.fun();		//这样调用会出错，A中无fun方法
	}
}
```

但是现在有一个疑问，在主方法里面明明可以直接生成一个B的实例化对象，然后直接调用fun就行了，为什么还要先向上转型，再向下转型呢？其实还是为了能够进行参数的同一。假如说在主方法中存在一个test方法，该方法只能接受一个A的对象：

```java
public void test(A a){

}
```

于是我们通过向上转型生成了一个a对象：

```java
public class Hello{
	public static void main(String [] args){
		A a = new B();	//向上转型
		test(a);
	}

	public static void test(A a){
		a.print();	//输出B的print方法，没毛病
	}
}
```

但是当a传到了test中发现A中的方法不太够用，想调用B中的fun方法，直接掉会出错

```java
public class Hello{
	public static void main(String [] args){
		A a = new B();	//向上转型
		test(a);

	}

	public static void test(A a){
		a.fun();	//输出B的print方法
	}
}
```

此时用向下转型将a转换为b就会变得特别的方便:

```java
class A{
	public void print(){
		System.out.println("A的print方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的print方法");
	}

	public void fun(){
		System.out.println("B的fun方法");
	}
}

public class Hello{
	public static void main(String [] args){
		A a = new B();	//向上转型
		test(a);

	}

	public static void test(A a){
		B b = (B) a;
		b.fun(); //输出B的fun方法
	}
}
```

## 4.什么时候用转型

对于对象的转型：

-   80%的情况下我们只会用到向上转型，因为可以得到参数类型的统一，方便我们做程序设计，比如说链表。
    -   子类中的方法尽量以父类的方法为主进行覆写，不要写太多父类中没有的方法（特殊的方法）。
-   5%的情况下会使用向下转型，目的是调用子类的特殊方法。
-   15%的情况下不转型，String。

个性化的操作在一个标准的开发中应该尽量少出现，因为对象的转型的操作里面有强制的问题，容易带来安全隐患。

```java
class A{
	public void print(){
		System.out.println("A的print方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的print方法");
	}
}

public class Hello{
	public static void main(String [] args){
		A a = new A();
		B b = (B) a;
		b.print();	//出错
	}
}
```

上面的代码是运行不通过的，因为a仅仅是A的实例化对象，它不知道他有B这个子类，所以让a向下转型的时候会出错。但是B却知道A是它的父类。因此向下转型并不是一帆风顺的，是会存在风险的。

为了保证顺利转型，在java里提供一个关键字：instanceof，此关键字的使用是这样的：

```java
对象  instanceof 类  返回值为boolean
```

如果某个对象是某个类的实例(或者说存在关系)，那么就返回true，否则返回false。

```java
class A{
	public void print(){
		System.out.println("A的print方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的print方法");
	}
}

public class Hello{
	public static void main(String [] args){
		A a = new B();
		System.out.println(a instanceof A);	// 返回true
		System.out.println(a instanceof B);	// 返回true
	}
}
```

因此，如果我们在进行向下转型的时候尽心一个判断，就可以避免出错。

```java
class A{
	public void print(){
		System.out.println("A的print方法");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的print方法");
	}
}

public class Hello{
	public static void main(String [] args){
		A a = new B();
		if (a instanceof B){
			B b = (B) a;
			b.print();
		}
	}
}
```

对于向下转型发生之前，一定要首先发生对象的向上转型，建立关系后才可以进行向下转型。

## 5.总结

-   开发中尽量使用向上转型
-   使用了向下转型之后才可以使用向下转型。
-   子类尽量不要扩充过多的与父类无关的操作方法。

