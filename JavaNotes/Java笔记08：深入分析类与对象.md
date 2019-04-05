2019.3.27

## 1. 封装性

- 使属性不能被外部直接访问：在属性名前面加一个private关键字
- 所有类中定义的属性都要用private声明
- 如果属性需要外部所使用，那么定义相应的getter和setter方法
  - setter方法主要是设置内容：public void setTitle(String t)	有参  这样就可以对传入的参数进行限制
  - getter方法主要是取得属性内容：public String getTitle()         无参

```java
// 自己定义的类
class Book{
	private String title;
	private double price;
    
    // 设置属性名
    public void setTitle(String t){
        title = t;
    }
   
    public void setPrice(Double p){
        price = p;
    }
    
	public void getInfo(){
		System.out.println("图书的名称："+title+",图书的价格："+price);
	}
	
    // 获取属性名
    public String getTitle(){
        return title;
    }
    
    public Double getPrice(){
        return price;
    }
    
}

// 主类
public class Hello{
	public static void main(String[] args){
		Book bk = new Book();
		bk.setTitle("月亮");
		bk.setPrice(90.0);
		bk.getInfo();
	}
}
```

## 2. 构造方法

- 类名称()：调用了一个和类名称一样的方法就是构造方法。
- 语法：public 类名称()
- 在没有自定义构造方法的前提下，程序编译后，会自动地为类里面加一个没有参数的、与类名称相同的、没有返回值的构造方法，以保证程序的正常运行。
- 构造方法是实例化对象的时候自动被调用的
- 构造方法只能被调用一次、普通方法只能被调用无数次
- 构造方法的定义原则：
  - 方法名与类名相同
  - 没有返回值

- 构造方法的核心作用吧：在类对象初始化的时候，设置属性的初始化内容。构造方法是为属性初始化准备的。

- 一个类至少有有一个构造方法。

- 构造方法也可以被重载，重载时注意参数的类型及个数即可。

- ```java
  class Book{
      // 无参构造方法
      public Book(){
          System.out.println("此为无参构造");
      }
      
      public Book(String t){
          System.out.println("此为有一个参的构造");
      }
      
      public Book(Double a, String B){
          System.out.println("此为有两个参的构造");
      }
  }
  
  public class Hello{
      public static void main(String[] args){
          Book bo = new Book(); //输出无参构造
          Book bo1 = new Book("好吧"); // 输出有一个参的构造
          Book bo2 = new Book(88.9,"你好"); // 输出两个参的构造
      }
  }
  ```

- 再对构造方法进行重载的时候，请按照参数的个数对构造方法进行升序、降序的排列。

- 类的实例化过程中需要依次经历：类的加载、内存的分配、默认值的设置、构造方法。

- 可见、构造方法是最后才执行的。

## 3. 匿名对象

没有栈内存指向的就是匿名对象。

```java
class Book{
    private String title;
    private Double price;

    public Book(String s, Double d){
        setTitle(s);
        setPrice(d);
    }

    public void setTitle(String s){
        title = s;
    }

    public void setPrice(Double d){
        price = d;
    }

    public String getTitle(){
        return title;
    }

    public Double getPrice(){
        return price;
    }

    public void getInfo(){
        System.out.println("图书名称："+title+",图书价格："+price);
    }
}

public class Hello{
    public static void main(String args[]){
        // 没有给对象命名、直接使用了这个对象。就是匿名对象。
        new Book("一本书",88.8).getInfo();
    }
}
```

- 匿名对象没有其他对象对其引用，所以只能使用一次就要等待被垃圾回收器回收了。

## 4.一个简单Java类的开发练习。

- 题目：开发一个雇员类，里面含有员工编号、姓名、职位、基本工资。
- 要求：
  - 类名称必须要有实际意义：比如：Book、Person
  - 类中的所有属性必须用private封装，封装后的属性必须要有，setter、getter方法。
  - 类中可以有多个构造方法，但必须要保留一个构造方法。
  - 类中不允许出现任何的输出、所有输出信息必须要在被调用出输出。
  - 类之中需要提供一个获得完整对象的方法、暂定为getInfo方法。返回String。

```java
// 普通方法
class Employee{
    private String name;
    private Integer number;
    private String position;
    private Double salary;

    // setter
    public void setName(String na){
        name = na;
    }

    public void setNumber(Integer num){
        number = num;
    }

    public void setPosition(String pos){
        position = pos;
    }

    public void setSalary(Double sa){
        salary = sa;
    }

    // 设置getter
    public String getName(){
        return name;
    }

    public Integer getNumber(){
        return number;
    }

    public String getPosition(){
        return position;
    }

    public Double getSalary(){
        return salary;
    }

    // 设置四个参数的构造方法
    public Employee(String na, Integer nu, String po, Double sa){
        setName(na);
        setNumber(nu);
        setPosition(po);
        setSalary(sa);
    }

    // 设置输出所有对象信息的getinfo方法
    public void getInfo(){
        System.out.println("员工姓名："+name+"\n员工编号："+number+"\n员工职位："+position+"\n员工工资："+salary);
    }
}

// 公共类
public class Hello{
    public static void main(String args[]){
        Employee emp = new Employee("孟祥东",201841022,"CEO",20000.0);
        emp.getInfo();
    }
}
```

