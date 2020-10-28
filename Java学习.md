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



## 005:抽象类和接口的区别

区别1：
			子类只能继承一个抽象类，不能继承多个
			子类可以实现**多个**接口
区别2：
			**抽象类**可以定义
			public,protected,package,private
			静态和非静态属性
			final和非final属性
			但是**接口**中声明的属性，只能是
			public
			静态
			final的
			即便没有显式的声明

注：抽象类和接口都可以有实体方法。



## 006：数字与字符串

### 字符串与数字转换：

#### 数字转字符串：

方法1： 使用String类的静态方法valueOf
方法2： 先把基本类型装箱为对象，然后调用对象的toString

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
        int i = 5;
         
        //方法1
        String str = String.valueOf(i);
         
        //方法2
        Integer it = i;
        String str2 = it.toString();
         
    }
}
```

#### 字符串转数字：

调用Integer的静态方法parseInt

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
 
        String str = "999";
         
        int i= Integer.parseInt(str);
         
        System.out.println(i);
         
    }
}
```

### 格式化输出：

如果不使用格式化输出，就需要进行字符串连接，如果变量比较多，拼接就会显得繁琐
使用格式化输出，就可以简洁明了

%s 表示字符串
%d 表示数字
%n 表示换行

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
 
        String name ="盖伦";
        int kill = 8;
        String title="超神";
         
        //直接使用+进行字符串连接，编码感觉会比较繁琐，并且维护性差,易读性差
        String sentence = name+ " 在进行了连续 " + kill + " 次击杀后，获得了 " + title +" 的称号";
         
        System.out.println(sentence);
         
        //使用格式化输出
        //%s表示字符串，%d表示数字,%n表示换行
        String sentenceFormat ="%s 在进行了连续 %d 次击杀后，获得了 %s 的称号%n";
        System.out.printf(sentenceFormat,name,kill,title);
         
    }
}
```

### 换行符：

**换行符**就是另起一行 --- '\n' 换行（newline）
**回车符**就是回到一行的开头 --- '\r' 回车（return）
在eclipse里敲一个回车，实际上是**回车换行符**
Java是跨平台的编程语言，同样的代码，可以在不同的平台使用，比如Windows,Linux,Mac
然而在不同的操作系统，换行符是不一样的
（1）在DOS和Windows中，每行结尾是 “\r\n”；
（2）Linux系统里，每行结尾只有 “\n”；
（3）Mac系统里，每行结尾是只有 "\r"。
为了使得同一个java程序的换行符在所有的操作系统中都有一样的表现，使用%n，就可以做到平台无关的换行

### 其他常用格式化操作：

#### 总长度，左对齐，补0，千位分隔符，小数点位数，本地化表达

```java
package digit;
  
import java.util.Locale;
   
public class TestNumber {
   
    public static void main(String[] args) {
        int year = 2020;
        //总长度，左对齐，补0，千位分隔符，小数点位数，本地化表达
          
        //直接打印数字
        System.out.format("%d%n",year);
        //总长度是8,默认右对齐
        System.out.format("%8d%n",year);
        //总长度是8,左对齐
        System.out.format("%-8d%n",year);
        //总长度是8,不够补0
        System.out.format("%08d%n",year);
        //千位分隔符
        System.out.format("%,8d%n",year*10000);
  
        //小数点位数
        System.out.format("%.2f%n",Math.PI);
          
        //不同国家的千位分隔符
        System.out.format(Locale.FRANCE,"%,.2f%n",Math.PI*10000);
        System.out.format(Locale.US,"%,.2f%n",Math.PI*10000);
        System.out.format(Locale.UK,"%,.2f%n",Math.PI*10000);
          
    }
}
```

### 操纵字符串：

#### 获取字符：

charAt(int index)获取指定位置的字符

#### 获取对应的字符数组：

toCharArray()
获取对应的字符数组

#### 截取子字符串：

subString
截取子字符串

```java
package character;
    
public class TestString {
    
    public static void main(String[] args) {
   
        String sentence = "盖伦,在进行了连续8次击杀后,获得了 超神 的称号";
         
        //截取从第3个开始的字符串 （从0开始）
        String subString1 = sentence.substring(3);
         
        System.out.println(subString1);
         
        //截取从第3个开始的字符串 （从0开始）到5-1的位置的字符串
        //左闭右开
        String subString2 = sentence.substring(3,5);
         
        System.out.println(subString2);
         
    }
}
```

#### 分隔:

split
根据分隔符进行分隔

```java
package character;
    
public class TestString {
    
    public static void main(String[] args) {
   
        String sentence = "盖伦,在进行了连续8次击杀后,获得了 超神 的称号";
         
        //根据,进行分割，得到3个子字符串
        String subSentences[] = sentence.split(",");
        for (String sub : subSentences) {
            System.out.println(sub);
        }
           
    }
}
```

#### 去掉首尾空格：

trim
去掉首尾空格

```java
package character;
    
public class TestString {
    
    public static void main(String[] args) {
   
        String sentence = "        盖伦,在进行了连续8次击杀后,获得了 超神 的称号      ";
         
        System.out.println(sentence);
        //去掉首尾空格
        System.out.println(sentence.trim());
    }
}
```

#### 大小写转换：

toLowerCase 全部变成小写
toUpperCase 全部变成大写

```java
package character;
    
public class TestString {
    
    public static void main(String[] args) {
   
        String sentence = "Garen";
         
        //全部变成小写
        System.out.println(sentence.toLowerCase());
        //全部变成大写
        System.out.println(sentence.toUpperCase());
         
    }
}
```

#### 定位：

indexOf 判断字符或者子字符串出现的位置
contains 是否包含子字符串

```java
package character;
     
public class TestString {
     
    public static void main(String[] args) {
    
        String sentence = "盖伦,在进行了连续8次击杀后,获得了超神 的称号";
  
        System.out.println(sentence.indexOf('8')); //字符第一次出现的位置
          
        System.out.println(sentence.indexOf("超神")); //字符串第一次出现的位置
          
        System.out.println(sentence.lastIndexOf("了")); //字符串最后出现的位置
          
        System.out.println(sentence.indexOf(',',5)); //从位置5开始，出现的第一次,的位置
          
        System.out.println(sentence.contains("击杀")); //是否包含字符串"击杀"
          
    }
}
```

#### 替换：

replaceAll 替换所有的
replaceFirst 只替换第一个

```java
package character;
    
public class TestString {
    
    public static void main(String[] args) {
   
        String sentence = "盖伦,在进行了连续8次击杀后,获得了超神 的称号";
 
        String temp = sentence.replaceAll("击杀", "被击杀"); //替换所有的
         
        temp = temp.replaceAll("超神", "超鬼");
         
        System.out.println(temp);
         
        temp = sentence.replaceFirst(",","");//只替换第一个
         
        System.out.println(temp);
         
    }
}
```

### Stringbuffer:

StringBuffer是可变长的字符串

append追加
delete 删除
insert 插入
reverse 反转

```java
package character;
  
public class TestString {
  
    public static void main(String[] args) {
        String str1 = "let there ";
 
        StringBuffer sb = new StringBuffer(str1); //根据str1创建一个StringBuffer对象
        sb.append("be light"); //在最后追加
         
        System.out.println(sb);
         
        sb.delete(4, 10);//删除4-10之间的字符
         
        System.out.println(sb);
         
        sb.insert(4, "there ");//在4这个位置插入 there
         
        System.out.println(sb);
         
        sb.reverse(); //反转
         
        System.out.println(sb);
 
    }
  
}
```

#### 为什么StringBuffer可以变长？

和String**内部是一个字符数组**一样，StringBuffer也维护了一个字符数组。 但是，这个字符数组，**留有冗余长度**
比如说new StringBuffer("the")，其内部的字符数组的长度，是19，而不是3，这样调用插入和追加，在现成的数组的基础上就可以完成了。
如果追加的长度超过了19，就会分配一个新的数组，长度比原来多一些，把原来的数据复制到新的数组中，**看上去** 数组长度就变长了 参考[MyStringBuffer](https://how2j.cn/k/number-string/number-string-mystringbuilder/331.html)
length: “the”的长度 3
capacity: 分配的总空间 19

**注：** 19这个数量，不同的JDK数量是不一样的