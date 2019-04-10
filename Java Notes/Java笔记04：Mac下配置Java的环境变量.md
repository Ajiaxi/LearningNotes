## 1.下载并安装jdk1.8

## 2. 查看Java安装目录

```shell
输入：/usr/libexec/java_home -v 1.8
得到：/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home
```

## 3. 打开当前用户环境变量设置文件

```shell
vim ~/.bash_profile
```

## 4.设置环境变量

```shell
 JAVA_HOME= "/usr/libexec/java_home -v 1.8"
 PATH="$JAVA_HOME/bin:$PATH"
 CLASSPATH=".:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar"

 export JAVA_HOME
 export PATH
 export CLASSPATH
```

这里的JAVA_HOME我设置为了动态的方式，这种设置方法在jdk升级的时候不会失效。因为jdk升级后的目录名会变。

设置PATH是为了能够在任何目录下使用javac命令

设置CLASSPATH是为了能后在任何目录下找到编译后生成的.class文件，一般设置为“.”，即在当前目录下招class文件。后面两个目录属于可选目录。目录之间使用“:”隔开。

```java
dt.jar：运行环境类库，主要是Swing包，这一点通过用压缩软件打开dt.jar也可以看到。如果在开发时候没有用到Swing包，那么可以不用将dt.jar添加到CLASSPATH变量中。

tools.jar：工具类库，它跟我们程序中用到的基础类库没有关系。我们注意到在Path中变量值bin目录下的各个exe工具的大小都很小，一般都在27KB左右，这是因为它们实际上仅仅相当于是一层代码的包装，这些工具的实现所要用到的类库都在tools.jar中，用压缩软件打开tools.jar,你会发现有很多文件是和bin目录下的exe工具相对性的，如下图：

```

