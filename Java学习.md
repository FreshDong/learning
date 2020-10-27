# Java学习笔记

------

## 001：在命令行编译和执行Java程序

1. 首先我们创建一个存放Java项目的目录

![image-20201027094146034](C:\Users\huawei\AppData\Roaming\Typora\typora-user-images\image-20201027094146034.png)





2. 打开src目录，在里面新建一个.txt文本文件，命名为HelloWorld.txt

   然后我们打开HelloWorld.txt并且在里面输入

![image-20201027094511883](C:\Users\huawei\AppData\Roaming\Typora\typora-user-images\image-20201027094511883.png)

​		保存这个HelloWorld.txt



3. **然后将文件拓展名改为HelloWorld.java**,这一步很重要

   

4. 接着我们进入命令行界面(按win+R,输入cmd,回车)

   因为我们项目在D盘，所以先进入D盘，即先输入**d:**命令并回车

   然后cd进入我们刚刚创建的存放Java项目的文件夹，即输入**cd D:\A009_JavaProject\src**

![image-20201027094941143](C:\Users\huawei\AppData\Roaming\Typora\typora-user-images\image-20201027094941143.png)

​		接着我们输入**javac HelloWorld.java**

​		然后输入**java HelloWorld**

​		可以看到控制台打印输出了我们刚刚在记事本中输入的hello world

![image-20201027095308179](C:\Users\huawei\AppData\Roaming\Typora\typora-user-images\image-20201027095308179.png)

​		到此为止，我们就完整地在控制台编译和运行了一个我们自己的Java项目



## 002：IDEA实用快捷键

**查找相关**的快捷键：

1.双击shift
在项目的所有目录查找，就是你想看到你不想看到的和你没想过你能看到的都给你找出来

2.ctrl+f
当前文件查找特定内容

3.ctrl+shift+f
当前项目查找包含特定内容的文件

4.ctrl+n
查找类

5.ctrl+shift+n
查找文件

6.ctrl+e
最近的文件

7.alt+F7
非常非常频繁使用的一个快捷键，可以帮你找到你的函数或者变量或者类的所有引用到的地方

------

**编辑相关**的快捷键：

1.shift+enter
另起一行

2.ctrl+r
当前文件替换特定内容

3.ctrl+shift+r
当前项目替换特定内容

4.shift+F6
非常非常省心省力的一个快捷键，可以重命名你的类、方法、变量等等，而且这个重命名甚至可以选择替换掉注释中的内容

5.ctrl+d
复制当前行到下一行

6.ctrl+x
剪切当前行

7.ctrl+c \ ctrl+v
大家都懂的

8.ctrl+z
撤销

9.ctrl+shift+z
取消撤销

10.ctrl+k
提交代码到SVN

11.ctrl+t
更新代码

12.alt+insert 可以自动生成构造器、getter/setter等等常用方法

13.alt+enter 自动修复

14.ctrl+alt+L 格式化

------

**全部**的快捷键

![](https://stepimagewm.how2j.cn/5778.png)



## 003：类和对象之访问修饰符

![](https://stepimagewm.how2j.cn/612.png)

**什么情况用什么修饰符？**

从作用域来看，public能够使用所有的情况。 但是大家在工作的时候，又不会真正全部都使用public,那么到底什么情况该用什么修饰符呢？

1. 属性通常使用private封装起来
2. 方法一般使用public用于被调用
3. 会被子类继承的方法，通常使用protected
4. package用的不多，一般新手会用package,因为还不知道有修饰符这个东西

再就是**作用范围最小原则**
简单说，能用private就用private，不行就放大一级，用package,再不行就用protected，最后用public。 这样就能把数据尽量的封装起来，没有必要露出来的，就不用露出来了



## 004：类和对象之单例模式

单例模式又叫做 Singleton模式，指的是一个类，在一个JVM里，只有一个实例存在。

#### 饿汉式单例模式

GiantDragon 应该只有一只，通过私有化其构造方法，使得外部无法通过new 得到新的实例。
GiantDragon 提供了一个public static的getInstance方法，外部调用者通过该方法获取12行定义的对象，而且每一次都是获取同一个对象。 从而达到单例的目的。
这种单例模式又叫做**饿汉式**单例模式，无论如何都会创建一个实例

```java
package charactor;
 
public class GiantDragon {
 
    //私有化构造方法使得该类无法在外部通过new 进行实例化
    private GiantDragon(){
         
    }
 
    //准备一个类属性，指向一个实例化对象。 因为是类属性，所以只有一个
 
    private static GiantDragon instance = new GiantDragon();
     
    //public static 方法，提供给调用者获取12行定义的对象
    public static GiantDragon getInstance(){
        return instance;
    }
     
}
```

```java
package charactor;
 
public class TestGiantDragon {
 
    public static void main(String[] args) {
        //通过new实例化会报错
		//GiantDragon g = new GiantDragon();
         
        //只能通过getInstance得到对象
         
        GiantDragon g1 = GiantDragon.getInstance();
        GiantDragon g2 = GiantDragon.getInstance();
        GiantDragon g3 = GiantDragon.getInstance();
         
        //都是同一个对象
        System.out.println(g1==g2);
        System.out.println(g1==g3);
    }
}
```



#### 懒汉式单例模式

**懒汉式**单例模式与**饿汉式**单例模式不同，只有在调用getInstance的时候，才会创建实例

```java
package charactor;
 
public class GiantDragon {
  
    //私有化构造方法使得该类无法在外部通过new 进行实例化
    private GiantDragon(){       
    }
  
    //准备一个类属性，用于指向一个实例化对象，但是暂时指向null
    private static GiantDragon instance;
      
    //public static 方法，返回实例对象
    public static GiantDragon getInstance(){
        //第一次访问的时候，发现instance没有指向任何对象，这时实例化一个对象
        if(null==instance){
            instance = new GiantDragon();
        }
        //返回 instance指向的对象
        return instance;
    }
      
}
```

```java
package charactor;
 
public class TestGiantDragon {
 
    public static void main(String[] args) {
        //通过new实例化会报错
		//GiantDragon g = new GiantDragon();
         
        //只能通过getInstance得到对象
         
        GiantDragon g1 = GiantDragon.getInstance();
        GiantDragon g2 = GiantDragon.getInstance();
        GiantDragon g3 = GiantDragon.getInstance();
         
        //都是同一个对象
        System.out.println(g1==g2);
        System.out.println(g1==g3);
    }
}
```



#### 什么时候使用饿汉式，什么时候使用懒汉式

**饿汉式**是立即加载的方式，无论是否会用到这个对象，都会加载。
如果在构造方法里写了性能消耗较大，占时较久的代码，比如建立与数据库的连接，那么就会在启动的时候感觉稍微有些卡顿。

**懒汉式**，是延迟加载的方式，只有使用的时候才会加载。 并且有[线程安全](https://how2j.cn/k/thread/thread-synchronized/355.html#step793)的考量
使用懒汉式，在启动的时候，会感觉到比饿汉式略快，因为并没有做对象的实例化。 但是在第一次调用的时候，会进行实例化操作，感觉上就略慢。

看业务需求，如果业务上允许有比较充分的启动和初始化时间，就使用饿汉式，否则就使用懒汉式



#### 单例模式三要素

> 什么是单例模式？

回答的时候，要答到三元素
1. 构造方法私有化
2. 静态属性指向实例
3. public static的 getInstance方法，返回第二步的静态属性

