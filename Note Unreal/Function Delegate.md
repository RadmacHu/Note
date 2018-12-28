# Handling Events and Delegates

[TOC]

# Creating a delegate that is bound to a UFUNCTION

### DESC

Delegates allow us to call a function without knowing which function is assigned. They are a safer version of a raw function pointer. This recipe shows you how to associate a UFUNCTION to a delegate so that it is called when the delegate is executed.

[Link](https://api.unrealengine.com/INT/API/Runtime/Core/Delegates/TBaseDelegate/BindUObject/1/index.html)

```c++
template<typename UserClass, typename... VarTypes>
void BindUObject
(
    UserClass * InUserObject,
    typename TMemFunPtrType < false, UserClass, RetValType >::Type InFunc,
    VarTypes... Vars
)
```



### Step



1. 声明

```c++
DECLARE_DELEGATE(FStandardDelegateSignature)
```



2. 定义成员变量

```c++
FStandardDelegateSignature MyStandardDelegate;
```



3. 绑定委托方法

```c++
MyStandardDelegate.BindUObject(this , &AItemBaseActor::PickItem);
```



4. 调用委托

```c++
MyGameMode->MyStandardDelegate.ExecuteIfBound();
```



> ExecuteIfBound checks that there's a function bound to the delegate, and then invokes it for us.



# Unregistering a delegate

取消绑定委托事件

```c++
MyStandardDelegate.Unbind();
```





# Creating a delegate that takes input parameters

带参数委托



1. 声明带参数委托

```c++
DECLARE_DELEGATE_OneParam(FParamDelegateSignature, int32)

// _OneParam 为一个参数
```



最大是九个参数

```c++
DECLARE_DELEGATE_NineParams(FParamDelegateSignature , ...)
```



2. 定义成员变量

```c++
FParamDelegateSignature MyParameterDelegate;
```



3. 绑定委托方法

```c++
MyParameterDelegate.BindUObject(this , &AItemBaseActor::PickItem);
```





# Passing payload data with a delegate binding

> With only minimal changes, parameters can be passed through to a delegate at creation time. This recipe shows you how to specify data to be always passed as parameters to a delegate invocation. The data is calculated when the binding is created, and doesn't change from that point forward.





# Creating a multicast delegate

> The standard delegates used so far in this chapter are essentially a function pointer—they allow you to call one particular function on one particular object instance. Multicast delegates are a collection of function pointers, each potentially on different objects, that will all be invoked when the delegate is broadcast.





1. 声明多播委托

```c++
DECLARE_MULTICAST_DELEGATE(FMulticastDelegateSignature)
    
//添加委托局部成员变量		
FDelegateHandle MyDelegateHandle;      
```



2. 定义成员变量

```c++
FParamDelegateSignature MyParameterDelegate;
```



3. 绑定委托方法

```c++
MyDelegateHandle = MyMulticastDelegate.AddUObject(this,&AMulticastDelegateListener::ToggleLight);
```



4. 调用

```c++
MyMulticastDelegate.Broadcast();
```



5. 移除绑定委托函数

```C++
// MyDelegateHandle为委托句柄

MyMulticastDelegate.Remove(MyDelegateHandle);
```



> The Broadcast() function is the multicast equivalent of ExecuteIfBound(). Unlike standard delegates, there is no need to check if the delegate is bound either in advance or with a call like ExecuteIfBound. Broadcast() is safe to run no matter how many functions are bound, or even if none are.



# Creating a custom Event

> Custom delegates are quite useful, but one of their limitations is that they can be broadcast externally by some other third-party class, that is, their Execute/Broadcast methods are publically accessible.





```c++
DECLARE_EVENT(AMyTriggerVolume, FPlayerEntered)
```

> The first parameter is the class that the event will be implemented into. This will be the only class able to call Broadcast(), so make sure it is the right one.
>
> The second parameter is the type name for our new event function signature.
>
>
>
> We add an instance of this type to our class. Unreal documentation suggests On<x> as a naming convention.





```c++
FPlayerEntered OnPlayerEntered;		// 定义成员变量
```





```c++
UPROPERTY(EditAnywhere)
AMyTriggerVolume* TriggerEventSource;

UFUNCTION()
void OnTriggerEvent();
```



绑定

```c++
if (TriggerEventSource != nullptr) 
{
	TriggerEventSource->OnPlayerEntered.AddUObject(this, &ATriggerVolEventListener::OnTriggerEvent);
}
```



实现	

```c++
void ATriggerVolEventListener::OnTriggerEvent() 
{
	TODO...
}
```









[参考链接](https://blog.csdn.net/aslgsx12315/article/details/51169848?utm_source=blogxgwz4)

[参考链接](http://www.cnblogs.com/timy/p/8682692.html)

[参考链接](http://gad.qq.com/article/detail/39865)

