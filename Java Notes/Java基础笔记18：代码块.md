在编写程序的时候，可以直接使用{}定义一段语句，根据此部分定义的关键字不同以及声明的关键字不同可以分为四种代码块：普通代码块，构造块、静态块、同步代码块（多线程的时候讲解）

开发中尽可能不要使用代码块。

## 1.普通代码块

如果一个代码块写在了方法里面，那么他就称为普通代码块。

```java
public class Hello{
	public static void main(String[] args){
		int num = 10;
		System.out.println("num = "+num);
		int num = 100;
		System.out.println("num = "+num);
	}
}
```

上面的程序会出错，因为变量不能被重复声明，但是加上{}之后，就不会报错了

```java
public class Hello{
	public static void main(String[] args){
        {int num = 10;
		System.out.println("num = "+num);
        }
		int num = 100;
		System.out.println("num = "+num);
	}
}
```

代码块使其里面的num成了局部变量，代码外面的是全局变量，二者互不影响，故不会出错。

## 2 构造块

在类里面定义的代码块叫做构造块。

```java
class Book{
	public Book(){
		System.out.println("构造方法中的内容");
	}

	//定义构造块
	{
		System.out.println("构造块中的内容");
	}
}

public class Hello{
	public static void main(String[] args){
		new Book();
		// 输出
		//构造块中的内容
		//构造方法中的内容
	}
}
```

根据输出结果可以看出，构造块的代码时先于构造方法中的代码执行的。实例化多个对象，构造块就会被执行多次。

## 3. 静态块

如果一个代码块用static修饰，那么就是一个静态块。静态块有两种使用方式：

### 3.1 非主类中使用

```java
class Book{
	public Book(){
		System.out.println("构造方法中的内容");
	}

	//定义构造块
	{
		System.out.println("构造块中的内容");
	}

	// static修饰的构造块
	static {
		System.out.println("静态块中的内容");
	}
}

public class Hello{
	public static void main(String[] args){
		new Book();
		new Book();
		new Book();
        
        //输出：静态块中的内容只会被执行一次，其余的正常被执行
    
	}
}
```

通过上面的代码我们可以发现，不管有多少个实例化对象被生成，静态块只会被执行一次，而且优先级高于构造块和构造方法。它的作用是为了static属性初始化的。

### 3.2在主类中定义

```java
public class Hello{

	// 在主类里定义的静态块
	static {
		System.out.println("======");
	}

	public static void main(String[] args){
		System.out.println("主方法内容");
	}
}
```

执行后发现，静态块中的内容先于主方法被执行。

## 4.总结

代码块能不用就不用

