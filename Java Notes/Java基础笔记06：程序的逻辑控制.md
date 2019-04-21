## 1. 顺序结构：有时候以{}为界限

## 2. 分支结构：就是一种判断结构

```java
//if
if(条件判断){
    语句
} else if(条件){
    语句
}else{
    语句
}


//switch
switch(整数 | 字符 | 枚举 | String){
    case 内容 : {
        内容满足时执行;
        [break;]
    } 
    case 内容 : {
        内容满足时执行;
        [break;]
    } 
    default:{
        内容都不满足时执行
        break;
    }
} 
```

switch不能判断布尔表达式，只能判断内容，区分大小写

判断首选是if语句

## 3. 循环结构：while，for

```java
//while:先判断再执行
    while(循环语句){
        循环语句
        循环结束条件
    }

//do....while：先执行再判断，以后不会用到do...while
    do{
        循环语句
        循环结束条件
    }while(循环结束条件)
    
//for
    for(循环初始条件, 循环判断, 循环条件变更){
        循环语句
    }
```

如何选择循环语句呢？

- 如果不知道循环次数，只知道循环结束条件的时候，用while
- 知道循环次数，用for 

## 4. 循环控制

- continue：跳过
- break：结束
- 这两个都需要和if结合