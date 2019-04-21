## 数据表与简单Java类的一对一的、一对多的映射

要求使用Java程序描述dept-emp关系，使用字段如下：

- dept：deptno、dname、loc
- emp：empno、ename、job、sal、comm、deptno、mgr

在dept-emp表的关系里面有如下关联

- 一个部门有多个雇员
- 一个雇员有0个或多个领导

### 第一步：实现基本字段的转换、进行关系关联。

```java
class Dept{
	private int 	deptno;
	private String 	dname;
	private String 	loc;
    private Emp emps[];	// 一个部门多个雇员的关系、多个雇员用对象数组来表示
	
	// 设置雇员信息	
    public void setEmps(Emp[] emps){
        this.emps = emps;
    }

    // 获取雇员信息
    public Emp[] getEmps(){
    	return this.emps;
    }

	//getter、setter略
	public Dept(int deptno,String dname,String loc){
		this.deptno = deptno;
		this.dname = dname;
		this.loc = loc;
	}

	public String getInfo(){
		return "部门编号："+this.deptno+"、部门名称："+this.dname+"、部门位置"+this.loc;
	}

}

class Emp{
	private int 	empno;
	private String 	ename;
	private String 	job;
	private Double 	salary;
	private Double 	comm;
	private Dept 	dept;
	private Emp  	mgr;

	// 设置领导
	public void setMgr(Emp mgr){
		this.mgr = mgr;
	}	

	// 获取领导信息
	public Emp getMgr(){
		return this.mgr;
	}

	// 设置部门
	public void setDept(Dept dept){
		this.dept = dept;
	}

	// 获取部门信息
	public Dept getDept(){
		return this.dept;
	}


	public Emp(int empno,String ename,String job,Double salary,Double comm,int deptno,String mgr){
		this.empno = empno;
		this.ename = ename;
		this.job = job;
		this.salary = salary;
		this.comm = comm;
		this.deptno = deptno;
		this.mgr = mgr;
	}

	public String getInfo(){
		return "编号："+this.empno + "、姓名："+this.ename+"、工作："+this.job+"、薪水："+this.salary+"、佣金"+this.comm+"、部门："+this.deptno+"、领导:"+this.mgr;
		
	}
}

public class Hello{
	public static void main(String[] args){

	}
}
```

### 第二步：设置和取出数据

- 根据结构设置数据
- 根据结构取出数据

```java
		// 第一步，设置数据
		// 设置部门和雇员信息
		Dept dept = new Dept(10,"IT技术部门","NewYork");

		Emp empa = new Emp(1,"张三","CEO",800.0,0.0);
		Emp empb = new Emp(2,"李四","CTO",400.0,1.0);
		Emp empc = new Emp(3,"王二","CFO",300.0,2.0);

		// 雇员与部门的关系
		dept.setEmps(new Emp[] {empa,empb,empc}); //使匿名对象

		//设置雇员与领导的关系
		empa.setMgr(empb);
		empb.setMgr(empc);

		// 取出数据
		// 根据雇员查询其领导信息
		System.out.println(empa.getInfo());
		System.out.println("\t|-:"+empa.getMgr().getInfo());

		// 根据部门查询雇员信息
		System.out.println(dept.getInfo());
		for(int i=0; i<dept.getEmps().length; i++){
			System.out.println("\t|-:"+dept.getEmps()[i].getInfo());
		}

		//根据部门信息查询员工信息及员工的领导信息
		System.out.println(dept.getInfo());
		for(int i=0; i<dept.getEmps().length; i++){
			System.out.println("\t|-:"+dept.getEmps()[i].getInfo());
			// 判断用户是否有领导
            if(dept.getEmps()[i].getMgr() != null){
				System.out.println("\t\t|-:"+dept.getEmps()[i].getMgr().getInfo());
			}
		}
```

得到的输出如下：

```shell
编号：1、姓名：张三、工作：CEO、薪水：800.0、佣金0.0
	|-:编号：2、姓名：李四、工作：CTO、薪水：400.0、佣金1.0
部门编号：10、部门名称：IT技术部门、部门位置NewYork
	|-:编号：1、姓名：张三、工作：CEO、薪水：800.0、佣金0.0
	|-:编号：2、姓名：李四、工作：CTO、薪水：400.0、佣金1.0
	|-:编号：3、姓名：王二、工作：CFO、薪水：300.0、佣金2.0
部门编号：10、部门名称：IT技术部门、部门位置NewYork
	|-:编号：1、姓名：张三、工作：CEO、薪水：800.0、佣金0.0
		|-:编号：2、姓名：李四、工作：CTO、薪水：400.0、佣金1.0
	|-:编号：2、姓名：李四、工作：CTO、薪水：400.0、佣金1.0
		|-:编号：3、姓名：王二、工作：CFO、薪水：300.0、佣金2.0
	|-:编号：3、姓名：王二、工作：CFO、薪水：300.0、佣金2.0
```

## 2. 多对多的关系映射

- 生成的只需要是实体表的转换操作
- 多对多的中间转换表，只需要类属性的配置关系设置即可
- 一对多用一个数组
- 多对多用两个数组

## 3.总结

在我们设置数据表与Java类的映射关系的时候，无论是一对一的关系，还是一对多的关系，都应该遵循下面的编写步骤。

1. 实现基本字段的转换
2. 设置类之间的关系关联
3. 设置和取出数据
   1. 设置数据
   2. 取出数据

