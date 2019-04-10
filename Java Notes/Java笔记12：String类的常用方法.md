对于系统自带的其他方法，要求会查就行了，但是String在开发中用的比较多，所以有关String类的一些方法需要背下来。

- 官方文档地址：<https://docs.oracle.com/javase/8/docs/api/>
- 如何找到String类：在Packages中（左上角）找到java.lang→Classes中（左下角）中找到String类

对于每一个文档的内容而言，他由以下几个部分组成：

- 类的定义及相关继承结构
- 类的简短说明
- 成员组成
- 构造方法
- 类中普通方法
- 方法的详细说明
- 用deprecated标注的方法不在建议使用

## 1.字符与字符串

下面是一些常用的构造方法

```java
//将字符数组转换为String类对象
public String(char[] value)
//将字符数组的某一部分转换为String类对象，offset表索引，count表数量
public String(char[] value, int offset, int count)
```

下面是一些常用的普通方法：

```java
//返回制定索引的字符
public char charAt(int index)
//将字符串变成字符数组，与上面的将字符数组转换为字符的构造方法作用相反
public char[] toCharArray()
```

取出指定位置的字符

```java
public class Hello{
    public static void main(String[] args){
        String str = "hello ";
        System.out.println(str.charAt(0));
    }
}
```

将字符串转换为字符数组并逐一打印输出

```java
public class Hello{
    public static void main(String[] args){
        String str = "hello ";
        char data[] = str.toCharArray();
        for(int i=0; i<data.length; i++){
        	System.out.println(data[i]);
        } 
    }
}
```

将小写字符串转换为大写

```java
public class Hello{
    public static void main(String[] args){
        String str = "hello ";
        char data[] = str.toCharArray();
        for(int i=0; i<data.length; i++){
        	data[i]-=32;
        }
        System.out.println(new String(data));
    }
}
```

案例：给定字符串，判断是否由数字组成。

```java
public class Hello{
    public static void main(String[] args){
    	String str = "12332w1";
    	if(isNumber(str)){
    		System.out.println("该字符串是由数字组成的");
    	}else{
    		System.out.println("该字符串不是由数字组成的");
    	}
    }

    public static boolean isNumber(String str){
    	char data[] = str.toCharArray();

    	for(int i = 0; i < data.length; i++){
    		if(data[i] < 48 || data[i] > 57){
    			return false; //有一个不通过返回false
    		}
    	}
    	return true; //全部通过 返回TRUE
    }
}
```

## 2.字节与字符

字节使用byte描述，使用字节一般用于数据的传输或编码转换的时候用，String类中就有将字符串变为字节数组的操作。目的就是为了传输，以及编码转换

相关构造方法：

```java
//将全部的字节数组转换为String对象
public String(byte[] bytes)
//将部分的字节数组转换为String对象
public String(byte[] bytes , int offset , int count)
```

相关普通方法：

```java
// 将字符串转换为字节数组
public byte[] getBytes(String charsetName) 
//进行编码转换
public byte[] getBytes(String charsetName)
       throws UnsupportedEncodingException 
```

- 以后讲IO操作的时候会牵扯到字节数组的概念

```java
public class Hello{
	public static void main(String[] args){
		String str = "helloworld!!!";
		byte data[] = str.getBytes();

		for(int i = 0; i < data.length; i++){
			data[i] -= 32; //将小写转换为大写
		}

		System.out.println(new String(data)); //全部转换，输出HELLOWORLD
		System.out.println(new String(data,5,5)); //部分转换，输出WORLD
	}
}
```

## 3.字符串的比较

比较字符串内容用equals()，但是字符串比较函数不止这一个:

- ```
  // 字符串内容的比较，返回布尔类型，特点是区分大小写
  public boolean equals(String anObject)
  //不区分大小写对字符串内容进行比较
  public boolean equalsIgnoreCase(String anotherString)
  //判断字符串的大小，按照字符编码比较：返回0代表两个字符串大小相等；>0:表示大于的结果；<0:表示小于的结果。
  public int compareTo(String anotherString)
  ```

内容比较举例

```java
public class Hello{
	public static void main(String[] args){
		String stra = "hello";
		String strb = "Hello";

		// 字符串内容比较
		System.out.println(stra.equals(strb)); //false
		System.out.println(stra.equalsIgnoreCase(strb)); //true
	}
}
```

大小比较举例

```java
public class Hello{
	public static void main(String[] args){
		String stra = "hello";
		String strb = "Hello";

		// 字符串内容比较
		System.out.println(stra.compareTo(strb)); //返回值为32，大于0，stra大于strb
	}
}
```

现在只有String类的对象才有大小关系的比较判断

## 4.字符串查找

从某个字符串中查找某个子字符串是否存在

```
// 判断指定内容是否存在,返回布尔，jdk1.5之后才有
public boolean contains(String s)

// 由前向后查找指定位置，返回整型，表示存在的具体索引位置（第一个字符位置），jdk1.5之前就有，没找到就返回-1
public int indexOf(String str)

// 从指定位置查找某字符串的指定位置，找到就返回第一个字符的位置，找不到就返回-1，这是indexOf的方法重载
public int indexOf(String str,int fromIndex)

// 由后向前查找字符串指定位置，找不到返回-1
public int lastIndexOf(String str)

// 从指定位置由后向前查找某子字符串的具体位置，找不到返回-1
public int lastIndexOf(String str,int fromIndex)

// 判断是否由指定的字符串开头
public boolean startsWith(String prefix)

//从具体位置开始判断某字符串是否以某子字符串开始
public boolean startsWith(String prefix，int toffset)

// 判断是否由指定的字符串结尾
public boolean endsWith(String suffix)
```

下面是可以返回一个位置的函数举例

```java
public class Hello{
	public static void main(String[] args){
		String stra = "helloworld";

		// 返回满足条件单词的第一个字母的索引
		System.out.println(stra.indexOf("world")); //返回5
		// 返回查到的第一个满足结果的位置
		System.out.println(stra.indexOf("l")); 	   //返回2
		// 从后开始查
		System.out.println(stra.lastIndexOf("l")); //返回8
	}
}
```

下面是可以返回布尔值的

```java
public class Hello{
	public static void main(String[] args){
		String stra = "helloworld";
		if(stra.contains("world")){
			System.out.println("可以找到数据");
		}
	}
}
```

在Java里面，contains已经成为了查询的代名词

下面是从开头或结尾判断内容:

```java
public class Hello{
	public static void main(String[] args){
		String stra = "##@@hello**";
		
		// 判断字符串stra是否以##开头
		System.out.println(stra.startsWith("##")); //TRUE
		// 判断字符串stra是否从第三个位置开始以@@开始
		System.out.println(stra.startsWith("@@")); //TRUE
		// 判断字符串stra是否以**位置结尾
		System.out.println(stra.endsWith("**")); //TRUE
	}
}
```

## 5.字符串替换

指的是使用心得字符串替换掉旧的字符串的方法，替换的返回值还是字符串

```java
// 全部替换
public String replaceAll(String regex,String replacement)
// 替换首个满足条件的内容
public String replaceFirst(String regex,String replacement)
```

举例说明

```java
public class Hello{
	public static void main(String[] args){
		String stra = "helloworld";

		// 全部替换
		System.out.println(stra.replaceAll("l","_")); 	// 返回he_ _owor_d
		// 部分替换
		System.out.println(stra.replaceFirst("l","_")); //he_loworld
	}
}
```

## 6.字符串截取

从一个完整的字符串中，截取某个子字符串，不能是负数

```java
//从某一个开始位置截取子字符串，直至最后一个,注意方法名的大小写
public String substring(int beginIndex)
    
// 重载方法、截取字符串，指定开始位置和结束位置
public String substring(int beginIndex,int endIndex)
```

## 7.字符串拆分

将完整的字符串，拆分成字符串数组（对象数组，String对象）

```java
// 按照指定字符串进行全部拆分
public String[] split(String regex)
// 按照指定字符串进行部分拆分,如果拆分的结果很多，那么数组长度由limit决定，即前面拆后面不拆
public String[] split(String regex,int limit)
```

举例说明

```java
public class Hello{
	public static void main(String[] args){
		String stra = "hello world my girl";

		//按空格将字符串拆分成字符串数组，如果只写了""，将按字符拆分
		String data[] = stra.split(" ");
		for(int i = 0; i < data.length; i++){
			System.out.println(data[i]);
		}

		// 按空格拆分，遇到第一个空格的前部分为数组的第一个元素，第二个元素是空格后面的部分
		String data1[] = stra.split(" ",2);
		for(int i = 0; i < data1.length; i++){
			System.out.println(data1[i]);
		}
	}
} 
```

- 如果拆分条件是:""，双引号里面没有任何内容，那就是将字符全部拆分
- 如果不够拆分条件，那就不拆了
- 如果是一些正则标记（.等）是拆分不了的，请用两个斜线\\\进行转以后有拆分

```java
public class Hello{
	public static void main(String[] args){
		String stra = "192.168.1.1";

		// 按.拆分需要转义
		String data[] = stra.split("\\.");
		for(int i = 0; i < data.length; i++){
			System.out.println(data[i]);
		}
	}
} 
```

举例：拆分如下数据：张三:23|李四:25|王二:33

```java
public class Hello{
	public static void main(String[] args){
		String stra = "张三:23|李四:25|王二:33";

		String data[] = stra.split("\\|");
		for(int i = 0; i < data.length; i++){
			String temp[] = data[i].split(":");
			System.out.println("姓名:"+temp[0]+"，年龄："+temp[1]);
		}
	}
} 
```

## 7.其他一些没有归类的方法

```java
// 字符串拼接，与 + 相似
public String concat(String str)

// 转为小写(非字母数据不会转换)
public String toLowerCase()
    
// 转为大写(非字母数据不会转换)
public String toUpperCase()

// 去掉字符串左右两边空格，中间空格保留
public String trim()
    
// 输出字符串长度
public int length()
    
// 数据入池，以备下次使用
public String intern()
    
// 判断字符串是否为空(空字符串不是null 而是"",也就是说长度为0)，可以用"".equals(str)的形式替换
public boolean isEmpty()
```

Java的一大遗憾就是缺少一个首字母大写的功能，自己实现如下

```java
public class Hello{
	public static void main(String[] args){
		String str1 = "hello";
		System.out.println(initCamp(str1)); // 输出Hello
	}

	public static String initCamp(String str){
		return str.substring(0,1).toUpperCase() + str.substring(1);
	}
} 
```

## 总结

- 记下所有方法的：方法名、参数、返回值、方法作用
- 一共32个基础方法