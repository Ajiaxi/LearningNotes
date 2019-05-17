这节要讲的是接口、抽象类、内部类之间的关系。首先来看一个简单的代码：

```java
interface Message{
	public abstract void print();
}

class MessageImp implements Message{
	public void print(){
		System.out.println("hello");
	}
}

public class Hello{
	public static void main(String[] args){
		fun(new MessageImp());
	}

	public static void fun(Message msg){
		msg.print();
	}
}
```

上面是一个简单的接口调用并实现，但是有一点需要注意。在实际开发中，一个java文件一般只有一个类，如果这里的MessageImp类在应用中只会用到一次，那么肯定就会造成空间的浪费，有没有办法可以解决这个问题呢？是的，今天要讲的匿名内部类就能实现：

```java
interface Message{
	public abstract void print();
}

public class Hello{
	public static void main(String[] args){
		fun(new Message(){
			public void print(){
				System.out.println("hello");
			}
		});
	}
	public static void fun(Message msg){
		msg.print();
	}
}
```

在在Message()后面直接写上MessageImp的方法体就实现了一个匿名内部类，但是目前不建议使用它，等到学Spring的时候再去用仔细了解即可。

-   匿名内部类必须要基于接口或匿名内部类。
-   如果匿名内部类定义在了方法里面，方法的参数或者是变量要被匿名内部类所访问，必须加上finally关键字，只不过jdk1.8之后这个规则被改变了。