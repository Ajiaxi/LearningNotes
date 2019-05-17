枚举在Java里面是了解知识，它在任何语言里面都存在，除了2005年之前的Java里面。我们之前讲的多例设计模式就是解决枚举问题。2005年之后Java里多了一个enum的关键字，它的作用就是定义枚举，下面定义一个枚举类

```java
package com.east.demo;

enum Color{
    RED,BLUE,GREEN;
}

public class Hello {
    public static void main(String[] args){
        Color red = Color.RED;
        System.out.println(red);
    }
}
```

枚举就直接代替了多例涉及模式，里面之所以大写，表示实例化对象。

