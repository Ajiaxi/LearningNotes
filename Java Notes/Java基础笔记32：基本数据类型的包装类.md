java在设计之初有一个基本原则：一切皆为对象。一切的操作都要求用对象的形式来描述。但是这里又出现了一个矛盾：基本的数据类型它不是对象。为了符合这种要求，所以最早的时候是采用人为的方式解决此问题。比如：

## 1.包装类的定义和简介

```java
// 包装了一个整型类，可以赋值、取数据
class MyInt{
	private int num;

	public MyInt(int num){
		this.num = num;
	}

	public int intValue(){
		return this.num;
	}
}

public class Hello{
	public static void main(String[] args){
		MyInt mi = new MyInt(20);
		int temp = mi.intValue();
		System.out.println(temp);
	}
}
```

这样就实现了一个认为的包装，但是为了方便，java已经为我们转备好了这样的包装类，叫做基本数据类型包装类，常见的如下：byte(Byte)、short（Short）、int(Integer)、long(Long)、float（Float）、double（Double）、char（Character）、boolean(Boolean)。以上就是几个数据类型的包装类。

以上的包装类又分为两种子类型：

-   对象型包装类（Object直接子类）：Character、Boolean
-   数值型包装类（Number直接子类）：Byte、Short、Integer、Long、Double、Float

Number是一个抽象类，里面一共定义了留个操作方法，byteValue()、intValue()、doubleValue()、FloatValue、 shortValue()、longValue()

##2.包装类的装箱和拆箱

现在已经存在基本数据类型和包装类，那么这两种变量间的转换就通过以下方式定义

-   装箱操作：将基本数据类型变为包装类的形式，每个包装类的构造方法都可以接收各自数据类型的变量
-   拆箱操作：从包装类中取出被包装的数据，利用Number类中的xxxValue方法来完成。

以下举两个利用Java给出的包装类进行装箱和拆箱，首先来看整型的包装类Integer：

```java
public class Hello{
	public static void main(String[] args){
		Integer in = new Integer(20);	//装箱
		int temp = in.intValue();		//拆箱
		System.out.println(temp * 2);
	}
}
```
上面的输出结果是40，再来看一下Boolean包装类：
```java
public class Hello{
	public static void main(String[] args){
		Boolean in = new Boolean(false);	//装箱
		boolean temp = in.booleanValue();
		System.out.println(temp);
	}
}
```

上面的输出结果是false。

但是以上的使用方法是jdk1.5之前的，在之后可以完成自动装箱和拆箱，还可以直接用于数学运算：

```java
public class Hello{
	public static void main(String[] args){
		Integer in = 20;	 //自动装箱
		System.out.println(++in);
	}
}
```

输出结果为21.

注意：在Integer类对象上发现，可以直接赋予内容，也可以使用构造方法。这两个有什么区别呢？我们来看一下对比~

```java
public class Hello{
	public static void main(String[] args){
		Integer ina = 20;	//直接赋值
		Integer inb = 20;	//直接赋值
		Integer inc = new Integer(20);	//构造方法赋值

		System.out.println(ina == inb);	//true
		System.out.println(ina == inc);	//false
		System.out.println(inb == inc); //false
	}
}
```

可以看出，直接赋值和采用构造方法赋值得到的对象是不相同的。但是本应该是相同的呀，既然a,b,c都是Integer类的实例化对象，那么我们就可以使用类的比对方法：

```java
public class Hello{
	public static void main(String[] args){
		Integer ina = 20;	//直接赋值
		Integer inb = 20;	//直接赋值
		Integer inc = new Integer(20);	//构造方法赋值

		System.out.println(ina.equals(inb));	//true
		System.out.println(inb.equals(inc));	//true
		System.out.println(inc.equals(ina)); 	//true
	}
}
```

这样得到的结果就都是TRUE了。

在以后使用包装类的时候，很少会用构造方法完成，都是直接赋值，这一点与String类似。但是在判断内容是否相等的时候，需要用equals来完成。

提示：Object可以一统天下了，Object可以接收一切的引用数据类型，但是由于存在着自动的装箱机制，那么Object也可以存放基本类型了。

(这里我学完Object再来看)

## 3.数据类型转换(核心)

使用包装类最多的情况实际上是它的数据类型转换功能上，在包装类中提供了将String型数据变为基本数据类型的方法。使用几个有代表的类作为说明。以下都是static方法，好处就是由类名称直接调用：

-   Integer类：public static int parseInt(String s)
-   Double类：public static double parseDouble(String s)
-   Boolean类：public static Boolean parseBoolean(String s)

特别需要注意的是Character类没有将字符串变为字符的函数，因为String类有一个cahrAt()的方法，可以通过索引取出字符内容。所以没有重复提供。

```java
public class Hello{
	public static void main(String[] args){
		String str = "123";
		int temp = Integer.parseInt(str);
		System.out.println(temp * 2);
	}
}
```

上面是一个简单的将字符串转换为整型的Demo，但是需要注意的是，字符串里面必须由数字组成。Boolean转过的过程中，如果字符串不是true活着false，那么统一转为false.

现在实现了字符串转换为基本数据类型的操作，那么也一定存在将数据类型转为字符串的操作，这里有两种方法：

-   任何基本数据类型通过`+`连接字符串都会变成字符串

```java
public class Hello{
	public static void main(String[] args){
		int num = 100;
		String str = num + "";
		System.out.println(str);
	}
}
```

但是这样做会产生垃圾，不推荐使用。String里顶一个一个valueOf方法，使用它可以进行转换，它是用static定义的，所以可以直接调用：

```java
public class Hello{
	public static void main(String[] args){
		int num = 100;
		String str = String.valueOf(num);
		System.out.println(str);
	}
}
```

这样的转换不会产生垃圾，因此推荐这样使用。

## 4.总结

-   1.5之后才可自动装箱，拆箱
-   字符串和基本数据类型的相互转换
    -   字符串转为数据类型：parseInt()、parseDouble()等等parseXXX()方法
    -   数据类型转为字符串：String.ValueOf()
-   所有的开发中，一定会用到包装类、数据类型转换