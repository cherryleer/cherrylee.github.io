---
layout: post
title: Java环境变量
category: program
comments: true
---


##PATH

<code>%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin\</code>

设置path变量的作用是系统在任何路径下都可以识别java,javac命令


##CLASSPATH

%JAVA_HOME%\lib\rt.jar;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar


rt.jar是JAVA基础类库，dt.jar是关于运行环境的类库，tools.jar是工具类库。这三个类包是java必不可少的。

设置在classpath里是为了让你 import 相应的内容



1、rt.jar

rt.jar 默认就在 根classloader的加载路径里面 放在claspath是多此一举。不信你可以去掉classpath里面的rt.jar，然后用 java -verbose XXXX 的方式运行一个简单的类 就知道 JVM的系统根Loader的路径里面 。其实不光rt.jar，jre\lib下面的大部分jar 都在这个路径里。



2、tools.jar

tools.jar 是系统用来编译一个类的时候用到的 也就是javac的时候用到

javac XXX.java 实际上就是运行

java -Classpath=%JAVA_HOME%\lib\tools.jar  xx.xxx.Main  XXX.java

javac就是对上面命令的封装 所以tools.jar 也不用加到classpath里面

tools.jar应用服务器用来编译JSP文件，应用服务器自己会加载，不需要自己设置。



3、dt.jar

dt.jar是关于运行环境的类库,主要是swing的包，如果你要用到swing时最好加上。



最后

rt.jar不需要引用；

tools.jar如果你需要手动javac，请应用；

dt.jar如果你用到swing，请引用；

另外，如果你的 JDK 版本>=1.6，编译器会自动去寻找 tools.jar 的，如果用的是 <1.6 以下的版本就加上去吧。



JAVA_HOME

仅仅是声明一个环境变量方便修改java安装位置而已。
