面向对象的主要内容有：类，接口，抽象类，对象。之前的章节都已经讲解过主体结构了，接下来将会学习包、异常，开发工具，这节来看包的概念：

##1.包的定义

包指的是程序的目录，在最早的时候，做开发会将所有的程序（类）写在一个文件里，该文件一般名为`*.java`，编译之后，程序将直接保存在根目录之下，但有了包之后，可以实现同一个程序的拆分，即将代码保存在不同的目录之下。

```java
package com.eastnotes.hello;

public class Hello{
	public static void main(String[] args){
		System.out.println("hello word");
	}
}
```

上面就定义了一个包，定义包要使用关键字`package`来完成，后面的.代表的目录下，比如上面的包会保存在当前目录下的com目录下的eastnotes目录下的hello目录中，名为Hello.class。要注意，生成包有专门的命令，这些目录不需要我们手动创建，用如下命令会自动创建：

```bash
javac -d . Hello.java
javac -d . *.java
```

-   -d：表示根据package的定义生成包
-   .：设置保存的路径，此处为.，表示在当前目录下生成。
-   *.java：表示编译所有的java文件

生成包之后，在执行程序的时候，要加上包的目录路径：

```bash
java com.eastnotes.hello.Hello
```

以后所有的类都要定义在包中，完整的类名称是`包.类`。

## 2.包的导入

不同包之间访问的话需要进行导入包的操作。使用import语句完成即可。首先我们定义一个Message类，将这个类保存在`com.eastnotes.util`中：

```java
package com.eastnotes.util;

class Message{
	public void print(){
		System.out.println("hello word");
	}
}
```

然后我们再定义一个TestMessage类，保存在`com.eastnotes.test`中，并且要使用上刚刚定义的Message类：

```java
package com.eastnotes.test;

import com.eastnotes.util.Message;

class TestMessage{
	public static void main(String[] args){
		Message msg = new Message();
		msg.print();
	}
}
```

接着，我们先后编译这两个文件，先编译Message，再编译TestMessage：

```java
javac -d . Message.java
javac -d . TestMessage.java
```

但是再编译第二个文件的时候遇到了如下的错误：

```bash
TestMessage.java:3: 错误: Message在com.eastnotes.util中不是公共的; 无法从外部程序包中对其进行访问
```

再次看一下刚才定义的Message类，我们发现此类并没有用public关键字修饰，因此不能被其他类所访问到，解决办法也很简单，直接在class前面用public修饰即可。修改后再次执行，我们发现错误没有了，运行TsetMessage包，可以得到下面的结果：

```bash
java com.eastnotes.test.TestMessage
```

输出：

```bash
hello word
```

总结：public class  和 class 声明类的完整区别：

-    public class：文件名称必须与类名称一致，在一个*.java里面只能有一个public  class声明，如果这个包要想让不同的包访问，一定要定义为Public
-   class：文件名不必与类名称一致，，在一个*.java里面可以有多个class声明，编译后有多个class文件。如果一个类使用的是class定义，那么这个类只能够被本包所访问。

另外，如果一个包中由多个class文件，则可以使用如下语句导入程序所需要的类。

```java
import 包.*
```

但是需要注意的是，这里的*不是导入所有的类，而是仅仅导入程序所需要的类。不用考虑性能问题。

此时就有一个问题出现，如果一个程序中导入了不同的包，但是这些不同的包里面有某些相同名称的包，比如需要同时导入如下两个包：

-   com.eastnotes.com.util.Message
-   com.reborn.Message

那么在编译的时候就会发生报错，报错信息是指Message包指定不明确。

解决办法是在使用类的时候，也加上包的目录，比如：

```java
com.reborn.Message msg = new com.reborn.Message();
```

以上就是所有的包的导入和定义。但是以后再开发中很少自己去写，因为开发工具会帮你完成。

## 3.系统常用包

-   java.lang：包含String、Object、Integer。从jdk1.1开始此包自动导入：
-   java.lang.reflect：反射开发包，框架的精髓
-   java.util：java的工具包，提供了大量的类，如链表
-   java.util.regex：正则工具包。
-   java.text：国际化处理程序包
-   java.io：进行输入输出处理以及文件操作
-   java.net：网络编程开发包
-   java.sql：数据库程序开发包
-   java.awt、java.swing：图像界面开发包，主要功能是进行单机版程序界面编写。

## 4.Jar命令

在任何一个项目里面，一定有大量的*.class，但是如果直接拿这些class文件交给用户使用的话，会非常没有结构，因此需要做一个压缩，用的就是jar命令，压缩的单位都是包。

## 5.总结

-   以后开发一定要有包的概念
-   如果包冲突，要写完整的类名称：包.类
-   以后使用第三方jar文件，必须配置CLASSPATH