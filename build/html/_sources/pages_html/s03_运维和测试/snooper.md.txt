Github上出现了一个新的专门用于调试Python程序的第三方库，名叫PySnooper。Snooper在英文中是监听器的意思。PySnooper，顾名思义，就是监听Python程序执行过程的工具。PySnooper一经问世，便引起Python社区的严重关注。仅仅一个月时间便收获了10K+个STAR，着实十分火爆。

相比Print调试往往需要写很多行Print语句，使用PySnooper仅仅一行代码就能实现对整个函数的调试，更加高效；相比Logging模块，使用PySnooper无需进行繁琐的配置，更加简单。





只需要导入PySnooper模块，并且给函数加上装饰器@pysnooper.snoop()，我们就可以实现对一个Python函数的监听(调试)。

在上面这个例子中，根据输出结果，我们可以得到：

- 程序执行步骤的顺序，比如执行结果的第2行告诉我们，在15:29:11.327032这一时刻执行了def number_to_bits这一行代码。
- 程序中变量的值的变化情况，比如执行结果的第9行告诉我们，局部变量number此时的值发生了变化，变成了3。







PySnooper支持灵活多样的程序调试，包括但不限于：

1. 给函数添加装饰器@pysnooper.snoop()，完成对函数的监听。
2. 使用with pysnooper.snoop()语句，实现对程序块(block)，即一行或者多行程序进行监听。
3. 使用@pysnooper.snoop(‘/my/log/file.log’)，将监听结果重定向到文件系统。
4. 监听非局部变量的值：
   @pysnooper.snoop(variables=('foo.bar','self.whatever'))
5. 监听一个列表或者字典变量的所有元素或者属性：
   @pysnooper.snoop(watch=('foo.bar','self.x["whatever"]'))
6. 深度监听——监听函数中的行所调用的其他函数：
   @pysnooper.snoop(depth=2)
7. 在多线程程序中，指定监听哪些线程：
   @pysnooper.snoop(thread_info=True)

另外，PySnooper的安装十分简单：

```text
pip install pysnooper
```

总结一下，PySnooper是一个使用简单，功能强大，效率高的Python调试工具，聚集各种优点于一身，难怪这么快就受到了社区的热烈欢迎。