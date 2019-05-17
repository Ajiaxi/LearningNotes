## 1.可变参数

如果要写一个函数，该函数可以接收任意数字，可以这样定义：

```java
package com.east.demo;
public class Hello {
    public static void main(String[] args){
        System.out.println(add(1,2,3));
    }
    
    public static int add(int ... data){
        int sum = 0;
        for(int x = 0; x < data.length; x++){
            sum += data[x];
        }
        return sum;
    }
}
```

可变参数接收之后就是一个数组，只不过调用的时候方便点，开发的时候尽量不去使用。

## 2.foreach

foreach最早是c#中的概念，其目的是进行数组或者集合数据的输出。之前的做法是通过索引，用for循环进行输出。但是foreach可以取消掉索引的使用，语法如下：

```java
int data[] = {1,2,3,4,5};
//普通for循环
for (int i = 0; i<data.length; i++){
    System.out.println(data[i]);
}
```

使用foreach

```java
for(int x : data){
    System.out.println(x);
}
```

每次循环都代表脚标的增长，会取得数组的内容，并将其赋给x。也就是说foreach支持数组的直接访问，而不是索引访问。

