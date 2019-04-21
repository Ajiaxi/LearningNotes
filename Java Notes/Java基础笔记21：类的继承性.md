继承性是类的三大特性之一，它主要解决的是代码重用性问题。

## 1. 问题的引出

假设我们现在需要定义两个类，一个是人类，一个是学生类，按照之前我们学到的类的定义方式，我们会进行如下的定义：

```java
class Person{
	private String name;
	private int age;

	public void setName(String name){
		this.name = name;
	}

	public String getName(){
		return this.name;
	}

	public void setAge(int age){
		this.age = age;
	}

	public int getAge(){
		return this.age;
	}
}

class Student{
	private String name;
	private int age;
	private String school;

	public void setName(String name){
		this.name = name;
	}

	public String getName(){
		return this.name;
	}

	public void setAge(int age){
		this.age = age;
	}

	public int getAge(){
		return this.age;
	}

	public void setSchool(int age){
		this.age = age;
	}

	public String getSchool(){
		return this.school;
	}
}
```

以上代码的一个很明显的问题就是代码重复，出现重复的原因是学生属于人，具有与人相同的属性。

## 2. 继承的实现

java中实现类的继承，用extends关键字来完成。我们称子类也叫做派生类，叫父类也叫做基类、超类。

```java
class 子类名 extends 父类名{

}
```

使用类的继承之后，Student类可以改造为：

```java
class Student extends Person{
	private String school;

	public void setSchool(String school){
		this.school = school;
	}

	public String getSchool(){
		return this.school;
	}
}
```

由此可以发现，类的继承有如下的优点：

-   子类可以直接将父类的操作继续使用，属于代码重用
-   子类可以继续扩充自己的标准。

## 3. 继承的限制

-   限制一：Java不允许多重继承，但允许多层继承

下面的操作是不对的

```java
class A{}
class B{}
class C extends A,B{}
```

但是如果我们确实需要继承使用多个类的功能该如何实现呢？这时可以使用多层继承：

```java
class A{}
class B extends A{}
class C extends B{}
```

通过上面的多层继承，我们可以完成一个子类继承多个类的功能。但是从开发的角度来讲，不要超过三层。

-   限制二：子类严格来说会继承父类的所有操作，但是所有的私有操作属于隐式继承，而所有的非私有操作属于显示继承。

如果直接在B中使用A的私有属性会报错

```java
class A{
	private String msg;

	public void setMsg(String mag){
		this.msg = msg;
	}

	public String getMsg(){
		return this.msg;
	}
}

class B extends A{
	// B 不能直接访问A中的私有属性
	public String getMsg(){
		return msg;	//报错
	}
}
```

但是通过间接的方式，通过B的实例化对象来使用的话，就不会报错

```java
class A{
	private String msg;
	public void setMsg(String msg){
		this.msg = msg;
	}
	public String getMsg(){
		return this.msg;
	}
}

class B extends A{}

public class Hello{
	public static void main(String[] args){
		B b = new B();
		b.setMsg("hello");
		System.out.println(b.getMsg());	//输出hello
	}
}
```

-   限制三：在子类对象构造之前一定会默认调用父类的无参构造。

```java
class A{
	public A(){
		System.out.println("A的构造方法");
	}
}

class B extends A{
	public B(){
		System.out.println("B的构造方法");
	}
}

public class Hello{
	public static void main(String[] args){
		B b = new B();
        // 先输出A的构造方法
        // 在输出B的构造方法
	}
}
```

此时并未假如任何的代码，但是可以发现，在实例化子类对象的时候，先实例化了父类对象，调用了父类的构造方法，然后再去实例化子类对象，调用子类的构造方法。

那么此时对于子类构造而言，就相当于隐藏了一个super()，因为父类也叫做super类，所以super()相当于对父类进行实例化对象。也就是相当于在B类中加了一个super()：

```java
class B extends A{
	public B(){
        super(); 	//加与不加，结果都是一样的。
		System.out.println("B的构造方法");
	}
}
```

那什么时候加super，什么时候不加super呢？当父类中没有无参构造，只有有参构造的时候，我们需要在继承的时候，加一个super(参数…..)，对父类进行有参构造方法的调用。

```java
class A{
	public A(String title){
		System.out.println("A的构造方法"+title);
	}
}

class B extends A{
	public B(String title){
		super(title);
		System.out.println("B的构造方法");
	}
}

public class Hello{
	public static void main(String[] args){
		B b = new B("hello");
        // 先输出  A的构造方法hello
        // 在输出  B的构造方法
	}
}
```

下面这样写和上面的写法功能一样，就是给super传参的方式不一样，下面是直接，上面是间接：

```java
class A{
	public A(String title){
		System.out.println("A的构造方法"+title);
	}
}

class B extends A{
	public B(){
		super("hello");
		System.out.println("B的构造方法");
	}
}

public class Hello{
	public static void main(String[] args){
		B b = new B();
	}
}
```

通过观察发现，super是子类调用父类的构造方法，那么这一行一定要放在子类构造方法的首行。这一点和this()是类似的。

但是，this()也是要求放在构造函数的首行的，这两个岂不是冲突了？是的，冲突了，通过验证，this()和super()，不管子类如何折腾，他都永恒有一个存在的前提：子类构造的调用前，已确定要先执行父类构造。为父类的对象初始化之后，才轮到子类的对象初始化。