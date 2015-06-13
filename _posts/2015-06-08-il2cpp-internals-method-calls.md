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

我们再来看看通过 delegate 调用会生成什么样的代码：

	// Get the object instance used to call the method.
	Important_t1 * L_0 = HelloWorld_ImportantFactory_m15(NULL /*static, unused*/, /*hidden argument*/&HelloWorld_ImportantFactory_m15_MethodInfo);
	V_0 = L_0;
	Important_t1 * L_1 = V_0;
	 
	// Create the delegate.
	IntPtr_t L_2 = { &Important_Method_m1_MethodInfo };
	ImportantMethodDelegate_t4 * L_3 = (ImportantMethodDelegate_t4 *)il2cpp_codegen_object_new (InitializedTypeInfo(&ImportantMethodDelegate_t4_il2cpp_TypeInfo));
	ImportantMethodDelegate__ctor_m4(L_3, L_1, L_2, /*hidden argument*/&ImportantMethodDelegate__ctor_m4_MethodInfo);
	V_1 = L_3;
	ImportantMethodDelegate_t4 * L_4 = V_1;
	 
	// Call the method
	NullCheck(L_4);
	VirtFuncInvoker1< int32_t, String_t* >::Invoke(&ImportantMethodDelegate_Invoke_m5_MethodInfo, L_4, (String_t*) &_stringLiteral1);

实际上的调用代码 `VirtFuncInvoker1<int32_t, String_t*>::Invoke` 在 GeneratedVirtualInvokers.h 文件里。这个文件是由 il2cpp.exe 生成的，并不是翻译自 IL 代码。il2cpp 会为调用有返回值和没有返回值的虚函数分别生成 VirtFuncInvokerN 和 VirtActionInvokerN，其中 N 代表参数的个数

Invoke 方法原型：

	template <typename R, typename T1>
	struct VirtFuncInvoker1
	{
	  typedef R (*Func)(void*, T1, MethodInfo*);
	 
	  static inline R Invoke (MethodInfo* method, void* obj, T1 p1)
	  {
	    VirtualInvokeData data = il2cpp::vm::Runtime::GetVirtualInvokeData (method, obj);
	    return ((Func)data.methodInfo->method)(data.target, p1, data.methodInfo);
	  }
	};

这里调用了 libil2cpp 中的 GetVirtualInvokeData 方法后发查找 vtable 中的 vitual 方法，然后调用之。

#通过反射调用

开销最大的就是通过反射来调用，我们来看一下 CallViaReflection 对应的代码：

	// Get the object instance used to call the method.
	Important_t1 * L_0 = HelloWorld_ImportantFactory_m15(NULL /*static, unused*/, /*hidden argument*/&HelloWorld_ImportantFactory_m15_MethodInfo);
	V_0 = L_0;
	 
	// Get the method metadata from the type via reflection.
	IL2CPP_RUNTIME_CLASS_INIT(InitializedTypeInfo(&Type_t_il2cpp_TypeInfo));
	Type_t * L_1 = Type_GetTypeFromHandle_m19(NULL /*static, unused*/, LoadTypeToken(&Important_t1_0_0_0), /*hidden argument*/&Type_GetTypeFromHandle_m19_MethodInfo);
	NullCheck(L_1);
	MethodInfo_t * L_2 = (MethodInfo_t *)VirtFuncInvoker1< MethodInfo_t *, String_t* >::Invoke(&Type_GetMethod_m23_MethodInfo, L_1, (String_t*) &_stringLiteral2);
	V_1 = L_2;
	MethodInfo_t * L_3 = V_1;
	 
	// Call the method.
	Important_t1 * L_4 = V_0;
	ObjectU5BU5D_t9* L_5 = ((ObjectU5BU5D_t9*)SZArrayNew(ObjectU5BU5D_t9_il2cpp_TypeInfo_var, 1));
	NullCheck(L_5);
	IL2CPP_ARRAY_BOUNDS_CHECK(L_5, 0);
	ArrayElementTypeCheck (L_5, (String_t*) &_stringLiteral1);
	*((Object_t **)(Object_t **)SZArrayLdElema(L_5, 0)) = (Object_t *)(String_t*) &_stringLiteral1;
	NullCheck(L_3);
	VirtFuncInvoker2< Object_t *, Object_t *, ObjectU5BU5D_t9* >::Invoke(&MethodBase_Invoke_m24_MethodInfo, L_3, L_4, L_5);

这里我们创建一个数组来放置各个参数，然后调用 MethodBase::Invoke() 方法，在调用最终方法之前还要去调用另一个虚函数！

#总结

我们一直在寻找优化 il2cpp.exe 生成代码的方法，以后版本的 Unity 方法调用可能会变得不一样。

下一篇我们会讨论方法的实现然后看一下我们是如何利用共享模板方法的实现来减小代码体积的。