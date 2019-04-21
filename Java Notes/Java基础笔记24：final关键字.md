在java之中final称为终结器，在java之中可以使用final定义类，方法，属性。

## 1.使用final定义的类不能有子类

下面的定义会出错

```java
final class A{}
class B extends A{}
```

一般在进行系统类的开发的时候会使用到final，如果要进行一些架构代码的开发也会用到final，一个使用者很难有final类。

## 2. 使用final定义的方法不能被子类覆写

在一些时候由于父类中的某些方法具有隐藏的一些特性，那么子类必须使用此方法操作的时候，就加上final，意思是子类不要破坏这个方法的作用.

下面这样写是错误的：

```java
class A{
	public final void fun(){}
}

class B extends A{
	public void fun(){}
}

```

## 3. 使用final定义的变量变成了常量

常量必须在定义的时候设置好内容，并且不能被修改，下面的写法是错误的：

```java
class A{
	final double GOOD = 100.0;
	public final void fun(){
		GOOD = 122.0;
	}
}
```

为了与变量进行区分，请在定义常量的时候，将常量名称定义为全部大写。

## 4.全局常量

用public static fianl 定义的常量就是全局常量：

```java
public static final double GOOD = 100.0;
```

static保存在公共区域，此处的常量就是公共常量。

## 5.总结

-   在看到被final定义的类和方法的时候，不要去进行继承或覆写
-   使用public static final 定义的常量是全局常量，全部使用大写