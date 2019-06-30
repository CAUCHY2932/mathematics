## 基础语法
## spark入门

>https://www.imooc.com/video/14388



学习地址：
>https://www.bilibili.com/video/av24631394/?p=8

### 变量声明
#### 变量
```scala
var
变量值可变，类型不可变

val
变量值不可变

同时也可以指定类型赋值
注：Scala推荐val声明，var取值并不断赋值推荐var

实例如下：
    
    val a=89
    var b = "happy"
    
    val c:Int = 78
    var d:String = "enen"

## Scala的值类型有七种（无引用类型）

    Byte
    Char
    Short
    Int
    Long
    Float
    Double

## 条件表达式

    val y= if(a>1) 1 else -1
    val x= if(b<8) 56 else "haode"
    val z= if(c>8) 6
    val h= if(d<7) 6 else()
    val m= if(3>4) 5 else if(4>5) 8 else 1

## for 循环
    从1到10 1,2,3,4...10 （包括10）
    1 to 10
    1.to(10)
    
    从1到10 1,2,3,4..9 (不包括10)
    1 until 10
    
    循环代码实例：

for (i <- 1 to 10){
    println(i)
}


for(i <- Array("jljo","weg","geg","cvb")) println(i)


嵌套循环

for (i <-1 to 3;j <- 1 to 3 if (j!=i) ) println(i+10*j)

    平常的写法
    for(){
        for(){
            
        }
    }
    
    val x = for(i<- 1 to 3) yield i

## 方法和函数的声明
    1 + 3
    等效于：
    1.+(3)

### 在idea中，双击shift键可以查找类，例如我可以在弹出的窗口中输入Int，即可查找到Int类型的类

    声明一个方法
    
    def m1(x:Int,y:Int): Int =x+y
    
    调用这个方法
    m1(3,4)

### Scala中函数和方法是不同的
    定义一个函数
    
    val func=(x:Int, y:Int) => x+y
    
    调用这个函数
    
    func(3,6)
### 实际生产过程中，函数会经常当成参数传递给方法
    声明一个方法
    
    def m3(f:(Int, Int) => Int) =f(3,4)
    
    定义一个函数
    
    val f1=(x:Int, y:Int) => x+y
    
    调用
    m3(f1)
```



