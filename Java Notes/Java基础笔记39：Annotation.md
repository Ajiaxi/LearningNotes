Annotation利用注解的形式来实现程序的不同功能的实现，Java SE里面支持自定义Annotation的开发，并且提供了 三个最为常用的基础Annotation：@Override、@Deprecate、@SuppressWarnings

##1.准确的覆写：@Override

假如，我们打算覆写toString()方法，但是把S写成了小写：

```java
class Book{
    public void tostring(){
        System.out.println("hello");
    }
}

```

这样明显不能完成覆写的功能，但是这个错误无法再编译的时候发现，只能在程序运行的时候出现。

所以此时为了告诉编译器，toString 是覆写的方法，那么就可以加上@override，明确告诉编译器，这个方法是覆写来的，如果不是请报错。

```java
class Book{
    @Override
    public void tostring(){
        System.out.println("hello");
    }
}
```

## 2.声明过期操作：@Deprecated

如果一个类中有一个fun方法，之前一直用，但是现在他的功能有限，又开发了一个新的fun1方法，现在为了告诉之前使用fun方法的程序员，告诉他们fun现在已经有fun1这个改进的方法了，我们可以使用@Deprecated

```java
class Book{
    @Deprecated
    public void tostring(){
        System.out.println("hello");
    }
}

public class Hello {
    public static void main(String[] args){
        Book b = new Book();
        b.tostring();		//编译器会在这个函数上打上斜线
    }
}
```

过期的方法会被开发工具打上斜线，告诉你这个方法过期了。如果你在Java文档中看到一些方法下面写了了个Deprecated，那么这个方法就不要使了。

## 3.压制警告：@SupressWarnings

加入我们顶一个一个泛型，但是在使用的时候并没有给他指定一个明确的数据类型，那么编译器会给我们一个警告，但是我们就是故意留下的这个警告，又不想编译器提示警告，那么我们就可以使用@SupressWarnings

```java
class Message<T> {
    private T t;
    public Message(T t){
        this.t = t;
    }
    public T getT(){
        return this.t;
    }
}

public class Hello {
    @SuppressWarnings("rwatypes") 
    public static void main(String[] args){
        Message msg = new Message(123);
        System.out.println(msg.getT());
    }
}
```



