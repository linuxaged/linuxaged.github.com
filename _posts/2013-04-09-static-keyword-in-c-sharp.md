---
layout: post
title: "Static Keyword in C Sharp"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#References
[link 1](http://msdn.microsoft.com/en-au/library/79b3xss3\(v=vs.80\).aspx)


先给出微软官方的解释：

Static classes and class members are used to create data and functions that can be accessed without creating an instance of the class. Static class members can be used to separate data and behavior that is independent of any object identity: the data and functions do not change regardless of what happens to the object. Static classes can be used when there is no data or behavior in the class that depends on object identity.

Static 形式上把数据(变量)和操作（方法）放到一个 class 里, 访问他们并不需要通过面向对象的方式(创建实例,通过实例来访问)。编译器全局得分配内存给 static 对象，所以在使用 static 的时候要很小心，因为他是全局的，不会因为释放实例而回收他所占的内存。

要理解 static 不得不提 auto 这个关键字，他们的孪生兄弟，只不过编译器默认你定义的变量和函数是 auto （除非加上 static）. auto 就是自动的意思，在程序语言里就是当你需要这个变量或者函数时才分配（按需自动分配），static 对应就是固定的意思，不管你需不需要我给你留着。

#用法
##工具类
比如 Math 就是一个 包含了很多 `static function()` 的 `static class`

在使用的时候不需要创建实例，简单的 Math.sin(x) 就行了

##单纯的调用一些方法

我只是想调用这个类中的一两个方法，并没有什么状态相关的操作，那我就可以把这个类设成 static 的。虽然在现代语言中创建实例已经不是很耗资源的操作了，但是省去了创建实例再调用的麻烦，使得代码更容易维护

##Best Practice
static 这个关键字设计的初衷是用它来定义一些无法在 instance variable 里面定义的变量的

比如,
 `static instanceNumber// instance 个数`单独的 instance 是不知道 class 究竟被实例化了几次的.

当这个成员存在所有的类实例里面,可以考虑 static variable

否则使用 instance variable

通常来说,不建议使用 public static, 因为多线程情况下访问全局变量你需要加锁

###Singleton
最常见的 static 用法就是单例模式:
单例类只允许有一个实例,

    // Singleton pattern -- Structural example  
    using System;
    
    // "Singleton"
    class Singleton
    {
      // Fields
      private static Singleton instance;
    
      // Constructor
      protected Singleton() {}
    
      // Methods
      public static Singleton Instance()
      {
        // Uses "Lazy initialization"
        if( instance == null )
          instance = new Singleton();
    
        return instance;
      }
    }
    
    /// <summary>
    /// Client test
    /// </summary>
    public class Client
    {
      public static void Main()
      {
        // Constructor is protected -- cannot use new
        Singleton s1 = Singleton.Instance();
        Singleton s2 = Singleton.Instance();
    
        if( s1 == s2 )
          Console.WriteLine( "The same instance" );
      }
    }
    
关于C#的单例模式墙裂推荐这篇文章:[Implementing the Singleton Pattern in C#](http://csharpindepth.com/Articles/General/Singleton.aspx)