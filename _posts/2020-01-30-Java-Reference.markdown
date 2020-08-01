---
layout:     post
title:      "Java参数传递：值传递or引用传递？"
subtitle:   "Java参数传递方式"
date:       2020-01-30 12:00:00
author:     "MC"
header-img: "img/post-bg-java1.jpg"
header-mask: 50%
catalog: true
tags:
    - 知乎
    - Tech
    - Java
---

> 来源：[Java参数传递（超经典）](http://blog.sina.com.cn/s/blog_4b622a8e0100c1bo.html)
>
> 首发在我的知乎：[Java参数传递：值传递or引用传递？](https://zhuanlan.zhihu.com/p/104392651)

我因为有时候会忘记Java参数是值传递还是引用传递，网上查完之后过不了多久又记不清了。所以这次自己写这篇文章来备忘。



先看**基本类型**作为参数传递的例子：

```java
public class Test1 {
    public static void main(String[] args) {
        int n = 3;
        System.out.println("Before change, n = " + n);
        changeData(n);
        System.out.println("After changeData(n), n = " + n);
   }

   public static void changeData(int n) {
       n = 10;
   }
}
```

**基本类型作为参数传递时，是传递值的拷贝，无论怎么改变这个拷贝，原值是不会改变的**，输出的结果证明了这一点：

```java
Before change, n = 3
After changeData(n), n = 3
```



那么，我们现在来看看**对象作为参数传递**的例子：

```java
public class Test2 {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer("Hello ");
        System.out.println("Before change, sb = " + sb);
        changeData(sb);
        System.out.println("After changeData(n), sb = " + sb);
    }
      
    public static void changeData(StringBuffer strBuf) {
        strBuf.append("World!");
   }
}
```

先看输出结果：

```java
Before change, sb = Hello
After changeData(n), sb = Hello World!
```

从结果来看，sb的值被改变了，那么是不是可以说：对象作为参数传递时，是把对象的引用传递过去，如果引用在方法内被改变了，那么原对象也跟着改变。从上面例子的输出结果来看，这样解释是合理。



但是我们再对上面的例子稍加改动一下，我们在方法体内新建一个sb对象：

```java
public class Test3 {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer("Hello ");
        System.out.println("Before change, sb = " + sb);
        changeData(sb);
        System.out.println("After changeData(n), sb = " + sb);
    }
      
    public static void changeData(StringBuffer strBuf) {
        strBuf = new StringBuffer("Hi ");
        strBuf.append("World!");
    }
}
```

按照上面例子的经验：对象作为参数传递时，是把对象的引用传递过去，如果引用在方法内被改变了，那么原对象也跟着改变。你会认为应该输出：

```java
Before change, sb = Hello
After changeData(n), sb = Hi World!
```

但运行一下这个程序，你会发现结果是这样的：

```java
Before change, sb = Hello
After changeData(n), sb = Hello
```

这就是让人迷惑的地方，对象作为参数传递时，同样是在方法内改变了对象的值，为什么有的是改变了原对象的值，而有的并没有改变原对象的值呢？这时候究竟是值传递还是引用传递呢？

下面就让我们仔细分析一下。



先看Test2这个程序：

```java
StringBuffer sb = new StringBuffer("Hello ");
```

这一句执行完后，就会在内存的堆里生成一个sb对象，如图：

![img](https://pic4.zhimg.com/v2-77c9464b07f82a8db7c726af67d29165_r.jpg)

如图所示，sb是一个引用，里面存放的是一个地址“@fff”（这个“@fff”代表内存地址），而这个地址正是“Hello ”这个字符串在内存中的地址。

```java
changeData(sb);
```

执行这一句后，就把sb传给了changeData方法中的StringBuffer strBuf，由于sb中存放的是地址，所以，strBuf中也将存放相同的地址，请看图：

![img](https://pic1.zhimg.com/v2-6bf4eb885470e75d37e17e426b6a85fc_r.jpg)

此时，sb和strBuf中由于存放的内存地址相同，因此都指向了“Hello”。

```java
strBuf.append("World!");
```

执行changeData方法中的这一句后，改变了strBuf指向的内存中的值，如下图所示：

![img](https://pic3.zhimg.com/v2-23491e8a0298563b6cc264b41a7a4a9e_r.jpg)

所以，Test2 这个程序最后会输出：

```java
After changeData(n), sb = Hello World!
```



再看看Test3这个程序。

在没有执行到changeData方法的strBuf = new StringBuffer(“Hi “);之前，对象在内存中的图和上例是一样的，而执行了strBuf = new StringBuffer(“Hi “);之后，则变成了：

![img](https://pic1.zhimg.com/v2-3af85c704f1d0fe6da845c3850a0497a_r.jpg)

此时，strBuf中存放的不再是指向“Hello”的地址，而是指向“Hi ”的地址“@eee” （同样“@eee”是个内存地址）了，new操作符操作成功后总会在内存中新开辟一块存储区域。

```java
strBuf.append("World!");
```

而执行完这句后，

![img](https://picb.zhimg.com/v2-2297af7262d42486d2b6f87ec395bc15_r.jpg)

通过上图可以看到，由于sb和strBuf中存放地址不一样了，所以虽然strBuf指向的内存中的值改变了，但sb指向的内存中值并不会变，因此也就输出了下面的结果：

```java
After changeData(n), sb = Hello
```



String类是个特殊的类，对它的一些操作符是重载的，如：

```java
String str = “Hello”; 
```

等价于

```java
String str = new String(“Hello”);
String str = “Hello”;
```

而

```java
str = str + “ world!”;
```

等价于

```java
str = new String((new StringBuffer(str)).append(“ world!”));
```

因此，我们只要按上面的方法去分析，就会发现String对象和基本类型一样，一般情况下作为参数传递，在方法内改变了值，而原对象是不会被改变的。



综上所述，我们就会明白，**在Java中对象作为参数传递时，实际上是把对象在内存中的地址拷贝了一份传给了参数。**

最后可以这么说：**Java就只有值传递，对于基本数据类型而言，这个值是本身，而对于其他类型，传递的这个值是其内存地址。这两者都可以通过自己的引用变量修改指向的内存里相关对象的属性。**