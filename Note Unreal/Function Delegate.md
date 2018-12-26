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







