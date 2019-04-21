## JDK与JRE

- JRE：Java的运行环境
- JDK：Java的开发环境

## 第一个Java程序

```java
java/Hello.java

public class Hello{
	public static void main(String args[]){
		System.out.println("Hello,World");
	}
}
```

在终端执行：

- javac Hello.java：对文件进行编译，生成Hello.class
- java Hello：对文件进行执行

有的小伙伴看到后不理解，为什么是 String[] args，这个 args 是干嘛的？String[] args 可以看出来它是一个数组。在命令行中比如运行 Test.class 文件,你可以这样写：

```java
java Test runoob
```

相当于给数组传入了一个 runoob 字符串。也可以打印出来，可以作为简单的输入。

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(args[0]);
    }
}
```

运行以上实例，输出结果如下：

```java
$ javac Test.java
$ java Test runoob
runoob
```

此处注意，main 是一个程序的入口，一个 java 程序运行必须而且有且仅有一个 main 方法。args[0] 是你传入的第一个参数，args[1]是传入的第二个参数，以此类推。

**public static void main(String[] args)** ，这是 Java 程序的入口地址，Java 虚拟机运行程序的时候首先找的就是 main 方法。跟 C 语言里面的 main() 函数的作用是一样的。只有有 main() 方法的 Java 程序才能够被 Java 虚拟机运行，可理解为规定的格式。

有关参数：

-  **public**：表示的这个程序的访问权限，表示的是任何的场合可以被引用，这样 Java 虚拟机就可以找到 main() 方法,从而来运行 **javac** 程序。
-  **static**： 表明方法是静态的，不依赖类的对象的，是属于类的，在类加载的时候 main() 方法也随着加载到内存中去。
-  **void:main()**：方法是不需要返回值的。
-  **main**：约定俗成，规定的。
-  **String[] args**：从控制台接收参数。

## 注意事项

- 文件名可以和类名不一致，但是如果一个类被public修饰，那么必须与文件名保持一致
- 生成的class文件，名称与类名一致
- 执行class的文件名称
- 一个*.java文件中可以定义多个class，但是编译后会分别形成不同的*.class
- 所有的程序都是从主方法执行的

```java
public static void main(String arg[]){
    这里写你的程序代码
}
```

- 我们会把主方法所在的类使用public class 定义，并将其称为主类

## 输出

- 输出换行：System.out.println()
- 输出不换行：System.out.print()

类前面加不加public的区别

- 加了public则文件名必须与类名保持一致，否则编译的时候会报错
- 加了public的类的文件可以被当做包导入
- 每一个文件都只能有一个被public修饰的类

