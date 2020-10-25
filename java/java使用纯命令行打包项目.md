1： javac -d 编译之后的class文件输出目录   指定源文件位置即可。例如

![](https://images2018.cnblogs.com/blog/1222943/201804/1222943-20180405093816652-1891165479.png)

对于多个包下面的源码编译，貌似javac不支持迭代编译，可能需要一次传入多个源码位置进行编译。一种便捷方法就是使用windows的bat脚本或linux的shell脚本先生成一个编译源文件清单，然后根据源文件清单进行编译，例如：

```language-java
dir / s / B *.java> sources.txt
```

生成的sources.txt格式如下：

![](https://images2018.cnblogs.com/blog/1222943/201804/1222943-20180405095141250-1312600911.png)

然后使用：

```language-java
javac -d classes @sources.txt
```

即可完成编译。目前例子中的源文件不涉及依赖问题，当存在依赖时候，无需考虑sources.txt中的源文件顺序。因为在编译时候当发现使用的类没有编译时编译器会自动去编译依赖的源文件。

创建jar包可以使用jar -cvf jarname.jar class文件夹 即可打包

运行java包 可以使用java -classpath jarname.jar mainclass
