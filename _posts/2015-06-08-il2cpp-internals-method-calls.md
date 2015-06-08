---
layout: post
title: "IL2CPP Internals: Method Calls"
description: ""
category: 
tags: []
---
{% include JB/setup %}

这是 IL2CPP Internals 系列的第四篇，这篇主要关注 il2cpp.exe 如何用 C++ 表示托管代码中的方法调用。我们主要讨论六种不同的方法调用：

	*直接调用实例和静态方法
	*通过编译时的 delegate 调用
	*通过虚函数调用
	*通过接口调用
	*通过运行时的 delegate 调用
	*通过反射调用

每种调用我们都要分析生成的 C++ 代码做了什么，耗费多少个指令。

#准备

我们设计一个包含以上六种调用的例子工程。编写了一下脚本：

	interface Interface {
	  int MethodOnInterface(string question);
	}
	 
	class Important : Interface {
	  public int Method(string question) { return 42; }
	  public int MethodOnInterface(string question) { return 42; }
	  public static int StaticMethod(string question) { return 42; }
	}

	private const string question = "What is the answer to the ultimate question of life, the universe, and everything?";
	 
	private delegate int ImportantMethodDelegate(string question);

	private void CallDirectly() {
	  var important = ImportantFactory();
	  important.Method(question);
	}
	 
	private void CallStaticMethodDirectly() {
	  Important.StaticMethod(question);
	}
	 
	private void CallViaDelegate() {
	  var important = ImportantFactory();
	  ImportantMethodDelegate indirect = important.Method;
	  indirect(question);
	}
	 
	private void CallViaRuntimeDelegate() {
	  var important = ImportantFactory();
	  var runtimeDelegate = Delegate.CreateDelegate(typeof (ImportantMethodDelegate), important, "Method");
	  runtimeDelegate.DynamicInvoke(question);
	}
	 
	private void CallViaInterface() {
	  Interface importantViaInterface = new Important();
	  importantViaInterface.MethodOnInterface(question);
	}
	 
	private void CallViaReflection() {
	  var important = ImportantFactory();
	  var methodInfo = typeof(Important).GetMethod("Method");
	  methodInfo.Invoke(important, new object[] {question});
	}
	 
	private static Important ImportantFactory() {
	  var important = new Important();
	  return important;
	}
	 
	void Start () {}

同样 Build 后的代码在 Temp\StagingArea\Data\il2cppOutput 目录下，然后用 Ctags 来帮助查看代码时候跳转。

#直接调用

最简单的就是直接调用一个方法。以下是直接调用 CallDirectory 的 C++ 代码：

	Important_t1 * L_0 = HelloWorld_ImportantFactory_m15(NULL /*static, unused*/, /*hidden argument*/&HelloWorld_ImportantFactory_m15_MethodInfo);
	V_0 = L_0;
	Important_t1 * L_1 = V_0;
	NullCheck(L_1);
	Important_Method_m1(L_1, (String_t*) &_stringLiteral1, /*hidden argument*/&Important_Method_m1_MethodInfo);

最后一行是真正的调用。因为前面的文章里已经说过 il2cpp 把所有方法都转化成了普通函数，生成的代码里并不存在成员函数或者虚函数。
调用静态方法和直接调用类似：

	Important_StaticMethod_m3(NULL /*static, unused*/, (String_t*) &_stringLiteral1, /*hidden argument*/&Important_StaticMethod_m3_MethodInfo);

相比之下调用静态方法的开销要小一些因为不需要创建实例，第一个参数也因此是 NULL

#编译时 delegate 调用



#总结

我们一直在寻找优化 il2cpp.exe 生成代码的方法，以后版本的 Unity 方法调用可能会变得不一样。

下一篇我们会讨论方法的实现然后看一下我们是如何利用共享模板方法的实现来减小代码体积的。