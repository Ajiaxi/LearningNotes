this有一个核心概念：指的是当前对象。Java里面可以实现对类属性的调用、类方法的调用、表示当前对象。接下来一一介绍：

## 1.调用属性

首先看一个之前写过的很简单的代码：

```java
class Book{
	private String title;
	private Double price;

	public Book(String t, Double p){
		title = t;
		price = p;
	}

	public void getInfo(){
		System.out.println("书名："+title+",价格："+price);
	}
}
public class Hello{
	public static void main(String[] args){
		Book books = new Book("Java开发",89.2);
		books.getInfo(); //输出：书名：Java开发,价格：89.2
	}
} 
```

在构造方法中，我们设置书名和价格的参数设置为了t、p。但是这种命名方法并不符合规范，我们想将形参设置为title、price，于是有了下面的改进：

```java
class Book{
	private String title;
	private Double price;

	public Book(String title, Double price){
		title = t;
		price = p;
	}

	public void getInfo(){
		System.out.println("书名："+title+",价格："+price);
	}
}

public class Hello{
	public static void main(String[] args){
		Book books = new Book("Java开发",89.2);
		books.getInfo();// 输出：书名：null,价格：null
	}
} 
```

但是编译运行之后，我们什么也没有得到。这是因为变量有一个作用域的概念，上面的Book构造方法中有一个title、Book 类中同样也有一个成员属性title，但是构造方法中的title的优先级比较高。因此当我们用title = title 的时候，前面的title并不是类中的属性title，那怎样才能够在保证参数名和类属性名一致的情况下，正确的调用到成员属性并赋值呢？那就是this关键字了！：

```java
class Book{
	private String title;
	private Double price;

	public Book(String title, Double price){
		this.title = title;
		this.price = price;
	}

	public void getInfo(){
		System.out.println("书名："+this.title+",价格："+this.price);
	}
}

public class Hello{
	public static void main(String[] args){
		Book books = new Book("Java开发",89.2);
		books.getInfo(); //输出：书名：Java开发,价格：89.2
	}
} 
```

this关键字所指向的title就是本类中的成员属性，因此在以后的开发中，只要是在类中使用成员属性，我们就用this关键字来调用。

## 2. 调用方法

### 2.1 调用构造方法

类中的多个构造方法可能存在相互调用的情况。使用的形式就是this(参数，参数)。例子如下，先要求没生成一个对象，无论是调用的无参构造还是若干参数的构造方法，都要求输出一个提示语句：新的对象产生。没用this()的时候我们可以这样来写：

```java
class Book{
	private String title;
	private Double price;

	// 无参构造方法
	public Book(){
		System.out.println("新的对象产生");
	}

	// 一个参数的构造方法
	public Book(String title){
		System.out.println("新的对象产生");
		this.title = title; 
	}

	// 两个参数的构造方法
	public Book(String title, Double price){
		System.out.println("新的对象产生");
		this.title = title;
		this.price = price;
	}

	public void getInfo(){
		System.out.println("书名："+this.title+",价格："+this.price);
	}
}

public class Hello{
	public static void main(String[] args){
		Book books = new Book("Java开发",89.2); //输出：新的对象产生
		books.getInfo(); 
		//输出：书名：Java开发,价格：89.2
	}
}
```

但是这些写会产生很多的重复代码，比如System.out.println("新的对象产生")、this.title = title;都属于重复代码。但是有了this关键字就不一样了，我们可以用this()代表调用无参构造方法，用this(参数)表示调用一个参数的构造方法，一次类推。于是功能相同的改进代码如下：

```java
class Book{
	private String title;
	private Double price;

	// 无参构造方法
	public Book(){
		System.out.println("新的对象产生");
	}

	// 一个参数的构造方法
	public Book(String title){
		this();
		this.title = title; 
	}

	// 两个参数的构造方法
	public Book(String title, Double price){
		this(title);
		this.price = price;
	}

	public void getInfo(){
		System.out.println("书名："+this.title+",价格："+this.price);
	}
}

public class Hello{
	public static void main(String[] args){
		Book books = new Book("Java开发",89.2); //输出：新的对象产生
		books.getInfo(); 
		//输出：书名：Java开发,价格：89.2
	}
}
```

这样一来，多个构造方法之间相互调用的重复代码问题是解决了，但是新的问题油然而生

- Java规定this(参数)只能放在方法中的第一行，换个位置就会发生错误。
- 构造方法能调用普通方法，但是普通方法无法使用this()调用构造方法。
- 构造方法相互调用的时候一定要保留调用的出口。

在上面的代码中，两参构造方法调用了一参构造方法，一参构造方法调用了无参构造方法，无参构造方法停止了调用。这让程序有了一个出口，得以让代码正常运行。如果我们在无参构造中再次调用两参构造方法，会出现“构造方法递归调用”的错误提示。

```java
// 无参构造方法
	public Book(){
        this("Hello",123.0);
		System.out.println("新的对象产生");
	}

	// 一个参数的构造方法
	public Book(String title){
		this();
		this.title = title; 
	}

	// 两个参数的构造方法
	public Book(String title, Double price){
		this(title);
		this.price = price;
	}


```

## 2.2 实例联系

定义一个雇员类（员工编号，姓名，工资，部门），在这个类中编写四个构造方法：

- 无参构造：编号为：0、姓名为：“无名氏”、工资为：0.0、部门设置：“未定”
- 单参构造（传递编号）：姓名为：“临时工”、工资为：800.0、部门设置：“后勤部”
- 双参构造（传递编号、姓名）：工资为：2000.0、部门设置：“技术部”
- 四参构造（传递编号、姓名、工资、部门）

实现方法一：普通方法：要啥给啥！

```java
class Emp{
	private int number;
	private String name;
	private Double salary;
	private String department;

	// 无参构造
	public Emp(){
		this.number = 0;
		this.name = "无名氏";
		this.salary = 0.0;
		this.department = "未定";
	}

	// 单参构造
	public Emp(int number){
		this.number = number;
		this.name = "临时工";
		this.salary = 800.0;
		this.department = "后勤部";
	}

	// 双参构造
	public Emp(int number, String name){
		this.number = number;
		this.name = name;
		this.salary = 2000.0;
		this.department = "技术部";
	}

	// 四参构造
	public Emp(int number, String name, Double salary, String department){
		this.number = number;
		this.name = name;
		this.salary = salary;
		this.department = department;
	}

	public String getInfo(){
		return "编号："+this.number+"，姓名："+this.name+"，工资"+this.salary+"，部门："+this.department;
	}
}

public class Hello{
	public static void main(String[] args){
		Emp emp1 = new Emp(); 
		Emp emp2 = new Emp(1234); 
		Emp emp3 = new Emp(3389,"向东"); 
		Emp emp4 = new Emp(2232,"金津",123.0,"销售部"); 
		
		System.out.println(emp1.getInfo());
		System.out.println(emp2.getInfo());
		System.out.println(emp3.getInfo());
		System.out.println(emp4.getInfo());
	}
}

// 输出结果
编号：0，姓名：无名氏，工资0.0，部门：未定
编号：1234，姓名：临时工，工资800.0，部门：后勤部
编号：3389，姓名：向东，工资2000.0，部门：技术部
编号：2232，姓名：金津，工资123.0，部门：销售部
```

上面方法的缺点显而易见，代码重复率太高，我们可以用this()方法进行改进：

```java
class Emp{
	private int number;
	private String name;
	private Double salary;
	private String department;

	// 无参构造
	public Emp(){
		this(0,"无名氏",0.0,"未定");
	}

	// 单参构造
	public Emp(int number){
		this(number,"临时工",800.0,"后勤部");
	}

	// 双参构造
	public Emp(int number, String name){
		this(number,name,2000.0,"技术部");
	}

	// 四参构造
	public Emp(int number, String name, Double salary, String department){
		this.number = number;
		this.name = name;
		this.salary = salary;
		this.department = department;
	}

	public String getInfo(){
		return "编号："+this.number+"，姓名："+this.name+"，工资"+this.salary+"，部门："+this.department;
	}
}

public class Hello{
	public static void main(String[] args){
		Emp emp1 = new Emp(); 
		Emp emp2 = new Emp(1234); 
		Emp emp3 = new Emp(3389,"向东"); 
		Emp emp4 = new Emp(2232,"金津",123.0,"销售部"); 
		
		System.out.println(emp1.getInfo());
		System.out.println(emp2.getInfo());
		System.out.println(emp3.getInfo());
		System.out.println(emp4.getInfo());
	}
}
```

## 2.3 调用普通方法

普通方法之间的调用可以不加this，但是为了保证程序的严谨性，还是建议加上this.

## 3. 表示当前对象

this表示当前对象，表示正在调用当前方法的对象。

```java
class Book{
	public Book(){
		System.out.println("this:"+this);
	}
}

public class Hello{
	public static void main(String[] args){
		Book book = new Book(); 	// 输出当前对象的堆内存地址
		System.out.println(book);	// 也是输出地址与上面的地址是一样的
	}
}
```



