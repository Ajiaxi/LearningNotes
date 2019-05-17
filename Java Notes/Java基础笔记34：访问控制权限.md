此节的访问控制权限是对类的封装做一个总结，先来观察四种访问控制权限：private、default、public、protected

| No   | 范围             | private | default | protected | public |
| ---- | ---------------- | ------- | ------- | --------- | ------ |
| 1    | 在同一个类中     | Y       | Y       | Y         | Y      |
| 2    | 同一个包的不同类 |         | Y       | Y         | Y      |
| 3    | 不同包的子类     |         |         | Y         | X      |
| 4    | 不同包的非子类   |         |         |           | Y      |

除了public之外，对于封装可以使用private、protected，default。一般不考虑使用default，这次重点来看protected。这种权限直接与包的定义有关，现在测试不同包的子类：

 现在cn.east.demoa中定义A类

```java
package cn.east.demoa;

public class A{
	protected String info = "hello world";
}
```

然后在cn.east.demob中定义一个A的子类B：

```java
package cn.east.demob;

import cn.east.demoa.A;

public class B extends A {
	public void print(){
		System.out.println(super.info);
	}
}
```

现在这两个类的关系属于不同包的子类，然后再写一个测试程序。执行以下B中的print函数：

```java
import cn.east.demob.B;
public class Test{
	public static void main(String[] args){
		B b = new B();
		b.print();
	}
}
```

此时可以正常输出。

但是如果是不通报的非子类，也就是说在test中直接输出A的info：

```java
import cn.east.demoa.A;

public class Test{
	public static void main(String[] args){
		A a = new A();
		System.out.println(a.info);
	}
} 
```

会得到以下报错

```java
Test.java:6: 错误: info 在 A 中是 protected 访问控制
		System.out.println(a.info);
		                    ^
1 个错
```

可以看出，在protected的保护下，不同包的非子类是访问不了的。

## 总结

-   java的封装性是以private、protected三种权限为主
-   对于权限的选择，有一下三点建议
    -   属性用private
    -   方法用public
    -   default不用
    -   protected了解即可

## 关于命名

-   属性的首单词的首字母小写，其余单词的首字母大写：studentName
-   类的所有单词的首字母大写：Studentinfo
-   方法名称：同属性名称：studentName
-   常量名称：全是大写：STUDENT
-   包名称：小写字母：demo

 

