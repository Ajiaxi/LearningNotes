如果现在要比较两个数字之间的大小，我们用==，如果是两个字符串进行比较，我们使用equals()，如果说现在有一个自定义的类，现在想要判断他的两个对象是否相等，该如何做？因为对象  =  数据集合，那么应该对两个对象所有属性进行逐一比较。

## 1.基础比较方法

```java
class Book{
	private String title;
	private Double price;

	public Book(String title,Double price){
		this.title = title;
		this.price = price;
	}

	public String getTitle(){
		return this.title;
	}

	public Double getPrice(){
		return this.price;
	}
}

public class Hello{
	public static void main(String[] args){
		Book book1  = new Book("Java",11.1);
		Book book2  = new Book("Java",11.1);

		// 进行两个对象之间的比较
		if((book1.getTitle().equals(book2.getTitle())) && (book1.getPrice() == book2.getPrice())){
			System.out.println("两个对象是相等的");
		}else{
			System.out.println("两个对象不相等！！");
		}
	}
}
```

虽然上面的代码实现了最基本的两个对象之间的比较，但是很不完善，因为main方法使我们的客户端，我们应该使这个方法中的代码越少越好。最好隐藏所有的细节逻辑。也就是说实现两个对象之间比较的最好的方法就是交给对象自己去比较，定义一个比较的方法。

但是在说改进方法之前，先看一个程序：

如果一个属性被private封装了，那么这个属性在类外部是不能被直接调用的。

```java
class Info{
	private String msg = "hello";

	public void print(){
		System.out.println("msg = "+msg);
	}
}

public class Hello{
	public static void main(String[] args){
		Info x = new Info();
		x.print(); // 输出的是msg = hello
		x.msg = "sss"; //不可直接调用
	}
}
```

上面的例子中，我们并不能实现由对象直接调用一个被private关键字封装的属性，但是并不代表着对象调用被private封装的属性不可以，要想实现，需要将对象传回去。讲对象传到类中的方法里面，那就相当于取消了封装的形式 ，可以直接通过对象访问属性。 

```java
class Info{
	private String msg = "hello";

	public void print(){
		System.out.println("msg = "+msg);
	}

	// 讲对象传回本类方法，可实现对本类属性的调用
	public void fun(Info temp){
		// 在类的内部直接利用对象访问私有属性 
		temp.msg = "你好啊！";
	}
}

public class Hello{
	public static void main(String[] args){
		Info x = new Info();
		x.fun(x);
	}
}
```

此类的代码只会在对象比较的时候使用，但也是常见形式，即一个类可以接受本类对象是没有问题的。

改进后的对象比较代码如下：

```java
class Book{
	private String title;
	private Double price;

	public Book(String title,Double price){
		this.title = title;
		this.price = price;
	}

	public String getTitle(){
		return this.title;
	}

	public Double getPrice(){
		return this.price;
	}

	// 本类接收本类对象，对象可以直接访问属性
	// 两个功能：带回了需要比较的信息，方便访问
	public boolean compare(Book book){
		// 执行b1.compare(b2)代码会有两个对象
		// 一个是当前对象this（调用方法对象，b1引用）
		// 传递的对象book(引用传递  b2)

		if(this == book){ //内存地址相同、自己比自己，不用比了
			return true;
		}

		if(this == null ){ // 穿了个空
			return false;
		}

		if(this.title.equals(book.title) && this.price == book.price){
			return true;
		}else{
			return false;
		}

	}
}

public class Hello{
	public static void main(String[] args){
		Book book1  = new Book("Java",11.1);
		Book book2  = new Book("Java",11.1);

		// 进行两个对象之间的比较
		if(book1.compare(book1)){
			System.out.println("两个对象是相等的");
		}else{
			System.out.println("两个对象不相等！！");
		}
	}
}
```

对象比较操作代码的形式都是固定的，都会按照固定的步骤进行同一对象的验证。并且在对象比较里，也可以发现this表示当前对象出现。

## 总结

- 对象比较一定是某一个类自己定义的功能
- 对象比较时一定要比较对象是否为空，地址是否相同。属性是否相同等等若干操作才可以完成。
- 简单Java类中必须要提供对象比较的方法。