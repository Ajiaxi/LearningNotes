## 1.基本概念

内部类就是指，在已经定义了一个类的基础之上，再其内部又定义了一个或多个类。从开发的角度，内部类，能少用就少用，优先考虑普通类。下面是一个内部类的代码实例：

```java
class Outer{
	private String msg = "hello world";
	class Inner{
		public void fun(){
			System.out.println(msg);
		}
	}
	public void fun(){
		new Inner().fun();
	}
}
public class Hello{
	public static void main(String[] args){
		Outer out = new Outer();
		out.fun();	//输出hello world
	}
}
```

使用内部类虽然牺牲了结构的简洁性，但是达到了一个很好的目的。为了凸显内部类的特点，我们将内部类拿到外面来，并实现与之前一样的功能。

```java
class Outer{
	private String msg = "hello world";
	
	public String getMsg(){
		return this.msg;
	}

	public void fun(){
		new Inner().fun();
	}
}

class Inner{
	public void fun(){
		System.out.println(new Outer().getMsg());
	}
}

public class Hello{
	public static void main(String[] args){
		Outer out = new Outer();
		out.fun();	//输出hello world
	}
}
```

将内部类拿出来之后，为了能够使得内部类能够访问外部类的属性，我们做了如下两个改变：

- 在外部类定义一个getter方法
- 内部类使用外部类的属性的时候，先实例化一个外部类对象

虽然说结果上达到了将内部类放在外部类里面的一样的效果，但是我们发现，程序中实例化了两个outer对象，这是我们不想看到的结果。

因此，内部类的一大优势就是能够轻松访问外部类中的私有操作。当然，内部类的好处不止这一个：虽然内部类可以轻松访问外部类的私有操作，反之，外部类也可以访问内部类的私有操作。

```java
class Outer{
	private String msg = "hello world";
	
	public String getMsg(){
		return this.msg;
	}

	class Inner{
		private String info = "世界,你好！";
	}

	public void fun(){
		Inner in = new Inner();
		System.out.println(in.info);
	}
}

public class Hello{
	public static void main(String[] args){
		Outer out = new Outer();
		out.fun();	//输出世界你好
	}
}
```

内部类的使用，使得私有属性的操作变得很方便，但是新的问题出现了，在内部类之中访问外部类属性的时候，直接写了属性的名，但是之前我们说的，只要类中访问属性，就应该用this关键字，但是如果再内部类中使用this调用外部类的属性，就会报错，因为this指的是当前对象，此时的当前对象就是内部类，但是内部类中没有外部类的属性，所以就会报错，但是解决办法也很简单：Outer.this.mag。

```java
class Outer{
	private String msg = "hello world";
	class Inner{
		public void fun(){
			System.out.println(Outer.this.msg);
		}
	}
	public void fun(){
		new Inner().fun();
	}
}
public class Hello{
	public static void main(String[] args){
		Outer out = new Outer();
		out.fun();	//输出hello world
	}
}
```

上面代码编译后，我们文件目录查看*.class的文件，会发现除了Hello.class、Outer.class之外，还有一个Outer$Inner.class，也就是说，内部类也会被编译成一个class文件，并且这里的\$在程序中就是.的意思。那我们能不能像实例化一个没有内部类的对象一样，仅仅实例化一个内部类对象呢？当然可以，语法如下：

```java
外部类.内部类 对象名 = new 外部类().new 内部类()
```

```java
class Outer{
	private String msg = "hello world";
	class Inner{
		public void fun(){
			System.out.println(Outer.this.msg);
		}
	}
	public void fun(){
		new Inner().fun();
	}
}
public class Hello{
	public static void main(String[] args){
		Outer.Inner in = new Outer().new Inner();
		in.fun();
	}
}
```

内部类不可能离开外部类的实例化对象，所以应该首先实例化外部类。

如果希望一个内部类只允许外部类调用，而不允许外面调用，那么可以用private关键词对内部类进行修饰。那么此时内部类就不可能在外部进行直接实例化对象，下面的程序就会出错。

```java
class Outer{
	private String msg = "hello world";
	private class Inner{
		public void fun(){
			System.out.println(Outer.this.msg);
		}
	}
	public void fun(){
		new Inner().fun();
	}
}
public class Hello{
	public static void main(String[] args){
		Outer.Inner in = new Outer().new Inner();
		in.fun();
	}
}
```

错误信息如下:

```shell
Hello.java:14: 错误: Outer.Inner 在 Outer 中是 private 访问控制
		Outer.Inner in = new Outer().new Inner();
		     ^
```

## 2. 使用static定义内部类

使用static定义的属性或方法，不受类的实例化对象的控制。所以如果用static定义了一个内部类，那么它就不会受到外部类的实例化对象的控制。

如果一个内部类由static定义，那么这个类就变成了一个外部类。并且只能够访问外部类中用static方法或者属性。这就相当于static定义的方法不能调用非static定义的属性和方法。

```java
class Outer{
	private static String msg = "hello world";
	//static修饰的内部类
	static class Inner{
		public void fun(){
			//这里会报错，因为static定义的类不能访问，非static属性
			System.out.println(msg);	
		}
	}
	public void fun(){
		new Inner().fun();
	}
}
public class Hello{
	public static void main(String[] args){

	}
}
```

要想修复这个bug，只需要用static修饰外部类中的msg变量。

此时，如果想实例化一个内部类的对象该如何做？下面是语法

```ava
外部类.内部类 对象名 = new 外部类.内部类()
```

```java
class Outer{
	private static String msg = "hello world";
	//static修饰的内部类
	static class Inner{
		public void fun(){
			//这里会报错，因为static定义的类不能访问，非static属性
			System.out.println(msg);	
		}
	}
	public void fun(){
		new Inner().fun();
	}
}
public class Hello{
	public static void main(String[] args){
		Outer.Inner in  = new Outer.Inner();
		in.fun();
	}
}
```

以后看到可以直接之力华“类.类.类”的形式，知道这是在实例化有static定义的内部类就行了

## 3.在方法中定义内部类

内部类可以再类中、代码块中、方法中定义，其中在方法内定义内部类是最常用形式。

```java
class Outer{
	private String msg = "hello world";

	public void fun(){
		//方法内定义的内部类
		class Inner{
			public void fun(){
				//这里会报错，因为static定义的类不能访问，非static属性
				System.out.println(msg);	
			}
		}

		new Inner().fun();
	}
}

public class Hello{
	public static void main(String[] args){
		Outer out = new Outer();
		out.fun();	//输出hello world
	}
}
```

除了可以访问外部类的属性，在方法中定义的内部类还可以访问方法中的参数和变量

```java
class Outer{
	private String msg = "hello world";
    
	public void fun(int num){
		Double score = 99.9;
		//方法内定义的内部类
		class Inner{
			public void fun(){
				//这里会报错，因为static定义的类不能访问，非static属性
				System.out.println("外部类属性是："+Outer.this.msg);	
				System.out.println("方法参数是："+num);	
				System.out.println("方法属性是："+score);	
			}
		}

		new Inner().fun();
	}
}
public class Hello{
	public static void main(String[] args){
		Outer out = new Outer();
		out.fun(100);	
	}
}
```

但这种操作只支持jdk1.8以后，1.8之前要想访问的话，需要在参数或变量前加上final：

```java
class Outer{
	private String msg = "hello world";
    
	public void fun(final int num){
		final Double score = 99.9;
		//方法内定义的内部类
		class Inner{
			public void fun(){
				//这里会报错，因为static定义的类不能访问，非static属性
				System.out.println("外部类属性是："+Outer.this.msg);	
				System.out.println("方法参数是："+num);	
				System.out.println("方法属性是："+score);	
			}
		}

		new Inner().fun();
	}
}
public class Hello{
	public static void main(String[] args){
		Outer out = new Outer();
		out.fun(100);	
	}
}
```

## 4.总结

- 内部类可以与外部类互相进行私有属性的访问。
- 内部类可以使用private关键字进行修饰，而后无法在外部实例化内部类的实例化对象，要想定义使用下面的语法结构：
  - 外部类.内部类 实例化对象 = new 外部类().new 内部类()
- 内部类可以使用static关键字进行修饰，而后可以脱离外部实例化对象的限制。此时直接实例化内部类对象的形式为
  - 外部类.内部类 实例化对象 = new 外部类.内部类()

- 内部类可以在方法中定义，这是在开发中用的比较多的。

