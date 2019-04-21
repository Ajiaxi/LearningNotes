继承的主要特征是子类可以根据父类已有的功能进行功能扩展，但是子类在定义属性或者方法的时候，可能会与父类的属性和方法重名，在这样的情况下称为覆写。覆写分为两种情况：1.方法的覆写；2.属性的覆盖。

## 1. 方法的覆写

###1.1 如何使用方法的覆写

当子类产生了与父类方法名相同、参数类型和个数相同、返回值相同的函数的时候，就发生了方法的覆写。

```java
class A{
	public void fun(){
		System.out.println("A的方法");
	}
}

class B extends A{
	public void fun(){
		System.out.println("B的方法");
	}
}

public class Hello{
	public static void main(String[] args){
		B b = new B();
		b.fun();	//输出的是B的方法
	}
}
```

如果B类中没有写fun，那么输出的就是：A的方法，但是B中出现了覆写，于是就输出了：B的方法。无论是重载（参数的个数可以不一样，但是返回值和方法名是一样的）也好，覆写也好，方法的返回值一定是一样的。否者会出错。

### 1.2 什么时候使用覆写？

覆写的原则：如果父类的方法功能不足，不适合本子类，但又必须使用该方法名的时候，就需要进行覆写了。

###1.3 覆写的权限问题

以上代码虽然实现了覆写，但是我们还应该考虑权限的问题。被子类所覆写的方法，不能有比父类更加严格的访问控制权限。

访问控制权限有三个：public > default > private，也就是说，父类的权限如果是public，那么子类覆写的时候只能用public，如果父类是default，那么子类覆写只能用default 或者 public，但是99%的情况下，方法都是用public来声明。属性都是用private来声明。

但是如果父类使用的是private，那么子类覆写使用public或default或private的时候，会出现灵异事件。假如说我们现在写一个正常的覆写功能：

```java
class A{
	public void fun(){
		print();
	}

	public void print(){
		System.out.println("A中的print");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的print");
	}
}

public class Hello{
	public static void main(String[] args){
		B b = new B();
		b.fun();	//毫无疑问会输出：B的print
	}
}
```

然后我们改写父类中print方法的权限为private

```java
class A{
	public void fun(){
		print();
	}

	private void print(){
		System.out.println("A中的print");
	}
}

class B extends A{
	public void print(){
		System.out.println("B的print");
	}
}

public class Hello{
	public static void main(String[] args){
		B b = new B();
		b.fun();	// 输出的是：A的print
	}
}
```

可以看出，如果父类中使用了private关键字修饰的函数，该方法对子类是不可见的，也是是不会被覆写的。子类中的print相当于是自己单独定义的，可以直接通过B类的实例化对象进行调用。

###1.4 如何执行被覆写的父类函数？

另外还有一个问题，父类中被覆写的方法是不会被自动执行的，我们有什么办法能执行它呢？用super.fun()的方式：

```java
class A{
	public void print(){
		System.out.println("A中的print");
	}
}

class B extends A{
	public void print(){
		super.print();
		System.out.println("B的print");
	}
}

public class Hello{
	public static void main(String[] args){
		B b = new B();
		b.print();
	}
}
```

在B类中使用Super.的方法就能实现调用被覆写的父类方法了。

### 1.5 super方法和this方法的区别

-   使用this方法，会首先查找本类中是存在被调用的方法，如果本类没有就去父类查找，如果父类还没有，那就会在编译的时候报错。
-   使用super方法，明确表示不会再本类中查找被调用的方法，而是会直接去父类中查找。

### 1.6 重载与覆写的区别

这个问题也可以被问称overloading和overwrite的区别

-   发生的范围：重载发生在一个类中，而覆写发生在继承关系中
-   定义：重载时方法名相同，覆写时方法名，参数个数和类型，返回值都得一样
-   权限：重载没有权限的限制、覆写中被覆写的子类方法不能有比父类更小权限
-   **重载的时候返回值以及参数的类型和个数可以不同，但考虑到程序的统一性，建议相同**

## 2.属性的覆盖

如果子类中定义了一个与父类中名称一样的属性，就成为属性的覆盖。但是属性的覆盖是没有意义的，因为在开发之中，任何属性必须封装，一旦封装置后，这样的覆盖是无意义的。

### 2.1 super和this的区别

-   功能：this是调用本类方法和属性，super是子类调用父类构造、父类方法、父类属性
-   形式：this现查找本类，在查找父类
-   当前对象：this表示本类的当前对象，但是super不能单独使用

## 3. 总结

-   只要发生继承关系，就一定存在覆写的应用，主要以方法为主
-   如果子类要使用被覆写的父类方法，需要使用super关键字
-   杯子类覆写的方法不能有比父类更严格的控制权限

