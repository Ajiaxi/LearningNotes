##  1.泛型的引出

现要求定义一个表示坐标的操作类(point)，在这儿类里面要求保存以下几种坐标。

-   保存数字：x=10，y=20
-   保存小数：x=10.2，y=20.3
-   保存字符串：x=东经20度，y=北纬15度

这个类的关键在于x和y的这两个变量的设计上。必须有一种类型可以保存着三类数据，首先想到的是Object类。

-   int：int自动装箱为Integer，Integer向上转型为Object
-   double：double自动装箱为Double，Double向上转型为Object
-   String：直接向上转型为Object

```java
package com.east.demo;

class Point{
    private Object x;
    private Object y;

    // 设置x，y
    public void setX(Object x){
        this.x = x;
    }

    public void setY(Object y){
        this.y = y;
    }

    // 取出x、y
    public Object getX(){
        return this.x;
    }

    public Object getY(){
        return this.y;
    }
}

public class Hello {
    public static void main(String[] args){
        Point p = new Point();
        p.setX(10.2);
        p.setY("东经20度");
        double x = (double) p.getX();
        String y = (String) p.getY();
        System.out.println(x);
        System.out.println(y);
    }
}

```

以上就通过向上转型、向下转型完成了整型、浮点型、字符串性数据的随意存入取出。但是这样还容易出现一个异常：

```java
Exception in thread "main" java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String
	at com.east.demo.Hello.main(Hello.java:32)
```

这个异常出现在，如果我们设置的坐标是整型的，但是我们以字符串类型来取出，就会报出这个错误。

向上转型的目的是为了统一参数，而向下转型的目的是操作子类定义的特殊功能。但是现在发现，向下转型是一件非常不安全的操作，因为容易发生类型错误。

在jdk1.5之后增加了泛型技术，其核心意义在于，类在定义的时候可以使用一个标记，此标记就表示类中属性或方法参数的类型的标记，在使用的时候才动态设置类型。

```java
package com.east.demo;

class Point<T>{
    private T x;
    private T y;

    // 设置x，y
    public void setX(T x){
        this.x = x;
    }

    public void setY(T y){
        this.y = y;
    }

    // 取出x、y
    public T getX(){
        return this.x;
    }

    public T getY(){
        return this.y;
    }
}
```

上面就使用标记T对原始代码做了更改，在使用Point类的时候才去设置标记的内容，也就是设置了类中的属性的内容。

```java
public class Hello {
    public static void main(String[] args){
        Point<String> p = new Point<String>();
        p.setX("东经");
        p.setY("西经");
        String x = p.getX();
        String y = p.getY();
        System.out.println(x);
        System.out.println(y);
    }
}
```

在实例化对象的时候，在Point后面添加一个String的标记，则表示将Point类中的所有的T都替换成了String，那么在输出的时候就不用向下转型了。因此也不会出现ClassCastException。 

于是，使用了泛型之后，类中所有的属性都是动态设置的，而所 有使用泛型标记的方法的参数类型也都发生了改变，避免了向下转型的问题。

但需要特别注意的是，如果要想使用泛型，那么它能够采用的类型只能够是类，即：不能是基本类型，只能是引用类型。比如说，不能写int，而要写包装类Integer。

```java
public class Hello {
    public static void main(String[] args){
        Point<Integer> p = new Point<Integer>();
        p.setX(123);
        p.setY(321);
        int x = p.getX();
        int y = p.getY();
        System.out.println(x);
        System.out.println(y);
    }
}
```

上面使用的就是包装类的自动装箱功能，将基本数据类型自动转换为包装类。但是对于泛型还需要两点说明：

-   如果在使用泛型类或者是接口的时候，没有设置泛型具体类型，那么会出现编译时的警告。为了保证不出错，所有的泛型都将使用Object表示。比如：

```java
 Point p = new Point();
```

上面的代码没有设置标记，那么系统会自动将标记设置为Object类。那么在取值的时候还需要执行转型操作。

-   从JDK1.7开始可以简化声明泛型，实例化的泛型可以省略

```java
Point<Integer> p = new Point();
```

## 2.通配符

在使用泛型接收参数的时候，要考虑到参数的同一，就可以使用通配符<?>

```java
package com.east.demo;

class Message<T>{
    private T title;
    public void setTitle(T title){
        this.title = title;
    }
    public T getTitle(){
        return this.title;
    }
}

public class Hello {
    public static void main(String[] args){
        Message<String> msg = new Message();
        msg.setTitle("Nihao");
        fun(msg);
    }

    public static void fun(Message<?> msg){
        System.out.println(msg.getTitle());
    }
}
```

这样设置的好处在于，如果我在主函数中修改了Message类的数据类型，那么在调用fun方法的时候无需更改参数的类型。

在?的基础上还有两个子通配符

-   ？extends类：设置泛型上线，可以在声明上和方法参数上使用；

比如设置：? extends Number：意味可以设置Number或者它的子类，包括Integet、double

-   ？super 类：设置泛型下限，方法参数上使用。

比如：? super String，意味着只能设置String或者它的父类，Object

最后，泛型不仅仅可以定义一个，可以定义多个，用逗号隔开即可

## 3.泛型接口

在此之前，泛型都是定义在了类上面，那么泛型也可以在接口上定义。这样的接口叫做泛型接口。

```java
interface Message<T>{
    void fun(T t);
}
```

上面定义了一个泛型接口，但是一个接口必须要有子类，那么泛型接口实现子类的方式有两种，第一种是在子类继续设置泛型：

```java
class MessageImp<T> implements Message<T>{
    public void fun(T t) {
        System.out.println(t);
    }
}

public class Hello {
    public static void main(String[] args){
        MessageImp<String> str = new MessageImp<>();
        str.fun("hello");
    }
}
```

第二种是子类不设置泛型，而为父接口明确定义一个泛型。

```java
class MessageImp implements Message<String>{
    public void fun(String t) {
        System.out.println(t);
    }
}

public class Hello {
    public static void main(String[] args){
        MessageImp str = new MessageImp();
        str.fun("hello");
    }
}
```

## 4.泛型方法

之前定义的泛型方法都是在支持泛型的类或者接口中定义的，其实也可以直接定义。

```java
public class Hello {
    public static void main(String[] args){
        String str = fun("String");
        System.out.println(str);
    }

    public static <T> T fun(T t){
        return t;
    }
}
```

方法中传入什么类型，T就是什么类型

## 5.总结

-   泛型解决的就是向下转型所带来的安全隐患，其核心组成就是在声明类或接口的时候不设置参数的类型。
-   ？可以接收任意的泛型类型，只能够取出，但是不能够修改。

