现在已经学了对象，接口，数组等，这么多的数据怎么能够统一呢？于是有了Object类的概念。

## 1.Object类的定义

Object类是所有类的父类，也就是任何一个类在定义的时候，如果没有明确的继承一个父类的话，那么它就是Object类的子类。也就是说，以下两种类的定义是一模一样的：

```java
class Book{}

class Book extends Object{}
```

因此，在整个Java里面，类的继承关系一直都存在(除了Object类之外)，这也就意味着，Object类能接受全部类的对象。因为可以向上自动转型。同样，一个类也可以向下转型到Object类。既然这样，我们在不确定参数类型的时候，使用Object类型是最好的选择。

观察文档中的Object类，我们可以发现，java在Object类里面定义了一个无参构造方法， 既然Object类是所有类的父类，那么在对象实例化的时候，子类的构造方法一定默认调用父类的无参构造。

从严格意义上来讲(一般不遵守)，任何一个简单Java类都应该覆写Object类中的三个方法：

-   取得对象信息：public String toString();
-   对象比较：public boolean equals(Object obj);
    -   这是标准的对象比较名称
    -   由于它自己不知道有多少子类，因此传的是Object参数
    -   接收要比较对象之后，一定要进行向下转型后，才可以取得子类的属性
-   取得对象HASH码：public int hashCode();

##2.取得对象信息：toString()

如果说现在是一个String类对象，直接输出这个对象可以取得字符串的内容，但是如果是一个自定义的对象，就会出现一个对象编码。

```java
class Book{

}

public class Hello{
	public static void main(String[] args){
		Book b = new Book();
		String s = "hello";
		System.out.println(b); //Book@7852e922
		System.out.println(s); //hello
	}
}
```

如上述代码，字符串对象输出的是字符串，但是自定义对象输出的是看不懂的东西。现在我们用toString()方法再来输出一下：

```java
System.out.println(b.toString); //Book@7852e922
```

但是得出来的结果和直接输出对象的结果是一样的。这是因为，当我们输出对象的时候，输出操作会自动调用对象中的toString方法，将对象变为字符串后在进行输出。 而Object类中的toString()为了适应所有对象的输出，所以只输出了编码！因为所有的对象可能内容不同，但是编码是一样的。但是我们也是可以根据自己的情况覆写toString方法。

```java
class Book{
	private String title; 
	private Double price;

	public Book(String title, Double price){
		this.title = title;
		this.price = price;
	}

	public String toString(){	//相当于之前写的getInfo()
		return "书名"+this.title+";价格："+this.price;
	}
}

public class Hello{
	public static void main(String[] args){
		Book b = new Book("JAVA",1000.0);
		System.out.println(b); 	// 自动输出：书名JAVA;价格：1000.0
	}
}
```

toString() 被覆写后会在对象被调用出自动输出。

## 3.对象比较：equals()

之前使用了一个自定义的compare方法进行对象的比较，但是那个不标准，标准的做法是使用equals方法来完成。下面是一个标准的对象比较方法：
```java
class Book{
	private String title; 
	private int price;

	public Book(String title, int price){
		this.title = title;
		this.price = price;
	}

	public boolean equals(Object obj){
		// 首先判断是否属于一个类
		if(obj == this){
			return true;
		}

		// 然后判断是否属于同一种类
		if(!(obj instanceof Book)){
			return false;
		}

		// 在比较一些这个对象是否为空
		if(obj == null ){
			return false;
		}

		// 将Object类向下转型为Book类
		Book book = (Book) obj;

		// 进行属性的比较
		if(this.title.equals(book.title) && this.price == book.price){
			return true;
		}

		return false;
	}
}

public class Hello{
	public static void main(String[] args){
		Book b = new Book("JAVA",1000);
		Book c = new Book("JAVA",1000);
		System.out.println(b.equals(c)); 	// true
	}
}
```

因为一些系统类库用的也是equals 所以如此强调equals。

## 4.接收引用数据类型

Object类可以接收一切引用数据类型。   比如接口和数组，先来看接收数组的例子：

```java
public class Hello{
	public static void main(String[] args){
		Object obj = new int[] {1,2,3,4,5};	// 将数组扔给Object类对象
		
		if(obj instanceof int[]){	//判断是否为整形数组
			int data[] = (int[]) obj; //向下转型给data
			for(int i = 0; i < data.length; i++ ){
				System.out.println(data[i]);
			}
		}
	}
}
```

接下来我们在写一个接口：

```java
interface A{
	public abstract void fun();
}

class B implements A{
	public void fun(){
		System.out.println("hello");
	}
}

public class Hello{
	public static void main(String[] args){
		A a = new B();  //接口对象
		Object obj = a;	//接收接口对象
		A t = (A) obj;	//向下转型
		t.fun();
	}
}
```

这样，整个程序的参数就统一在了Object类型上面了，开发就方便了 

## 5.修改链表

之前写链表的时候，一种数据类型就需要创建一个链表，数据无法统一，造成大量数据的冗余。但是现在有了Object类，它可以接收一切数据类型，包括引用数据类型和基本数据类型。因此我们只要将之前字符串链表中的String替换为Object就可以了。

之前写的链表如下，这个链表只能存储：

```java
// 每一个链表是由多个节点组成的
class Node{ 
	private String data;  // 保存数据
	private Node next;	  // 保存下一个节点

	// 生成节点的同时给他一个值
	public Node(String data){
		this.data = data;
	}

	// 实现节点的添加，添加的是一个Node
	// 第一次调用（Link）：this = Link.root
	// 第二次调用（Node）：this = Link.root.next
	// 第三次调用（Node）：this = Link.root.next.next
	public void addNode(Node newNode){
		if(this.next == null){
			this.next = newNode; //如果后面还有座，保存新节点
		}else{					//如果不为空，
			// 当前节点的下一个节点继续保存数据
			this.next.addNode(newNode);
		}
	}

	// 第一次调用（Link）：this = Link.root
	// 第二次调用（Node）：this = Link.root.next
	// 第三次调用（Node）：this = Link.root.next.next
	public void printNode(){
		System.out.println(this.data);
		// 如果还有下一个
		if(this.next != null){
			this.next.printNode(); //输出下一个
		}
	}
}

// Link负责数据的设置和输出
class Link{
	private Node root;
	// 增加数据
	public void add(String data){
		// 为了可以设置数据的先后关系，所以将data包装在一个node类对象中
		Node newNode = new Node(data);
		// 保存当前数据的时候，还没有根节点
		if(this.root == null){ //一个链表只能有一个根节点
			this.root =newNode; // 将新的节点设置为root
		}else{ //根节点存在了，定义下一个节点的工作应该交给Node来处理
			// 从root节点之后找到合适的位置
			this.root.addNode(newNode);
		}
	}

	// 数据的输出 还是Node的事儿
	public void print(){
		if(this.root != null){
			this.root.printNode();
		}else{
			System.out.println("暂无数据");
		}
	}
}

public class Hello{
	public static void main(String[] args){
		Link link = new Link();
		link.add("java");
		link.add("html");
		link.add("css");
		link.add("python");
		link.print();
	}
}
```

现在将所有的String都改为Object，除了主函数中的方法。这样就能像链表中传入所有的数据类型了：

