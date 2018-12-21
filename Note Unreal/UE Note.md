# UE Beginner Note

[TOC]



## First Cpp Files

创建Actor类,对应生成 .h 和 .cpp。

> MyActor.h

```C++
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class UBEGINER01_API AMyActor : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	AMyActor();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

	
	
};
```

> MyActor.cpp

```C++
// Fill out your copyright notice in the Description page of Project Settings.

#include "MyActor.h"


// Sets default values
AMyActor::AMyActor()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;

}

// Called when the game starts or when spawned
void AMyActor::BeginPlay()
{
	Super::BeginPlay();
	
}

// Called every frame
void AMyActor::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

}
```

### .h 和 .cpp 首行注释修改

![1545016472283](.\UE Note.assets\.%5Cuenotepic%5C1545016472283.png)

可在项目描述中的 Legal > Copyright 中进行修改头文件的注释。



## Remove Class

### 删除Class文件

1. 进入项目路径中的Source > 项目名字 。
2. 删除需要对应的.cpp和.h文件。
3. 前往项目的根目录，右键.uproject文件，重新生成VS Files。



## 预处理宏#pragma once

用于防止头文件多次include



## Unreal Macros【预处理宏】

|  Unreal Macros   | Desc                                                         |
| :--------------: | :----------------------------------------------------------- |
|      UCLASS      | [Link](https://docs.unrealengine.com/Programming/UnrealArchitecture/Objects) |
| GENERATED_BODY() |                                                              |
|      TEXT()      | In general, you should be using the **TEXT()** macro when setting string variable literals. If you do not specify the TEXT() macro, your literal will be encoded using ANSI, which is highly limited in what characters it supports. Any ANSI literals being passed into FString need to undergo a conversion to TCHAR (native Unicode encoding), so it is more efficient to use TEXT().<br /><br />[More About](https://docs.unrealengine.com/en-us/Programming/UnrealArchitecture/StringHandling) |
|   UPROPERTY()    | **UPROPERTY** 宏使得变量对 **虚幻引擎** 可见。 这样，当我们启动游戏或在之后的工作部分重新载入关卡或项目时，这些变量中设置的值将不会被重置。 我们还添加了 **EditAnywhere** 关键帧，这让我们可以在 **虚幻编辑器** 中设置<br /><br /><br />[More About 1](https://blog.csdn.net/u012793104/article/details/78480085)<br />[More About 2](https://wiki.unrealengine.com/UPROPERTY)<br /> |
|   UFUNCTION()    | 将此函数以如下方式设为一个 **UFUNCTION**，即可将其对 **虚幻引擎**公开。 |



###  宏参数

1. **BlueprintCallable** 函数在 C++ 中进行编写，可从 **蓝图图表** 进行调用。但必须编辑 C++ 代码方可对其进行变更和覆写。以这种方式进行标记的函数通常是非程序员使用的功能，但这些功能不应被改变，或改变后不存在实际意义。简单的例子就是任意类型的数学函数。
2. **BlueprintImplementableEvent** 函数在 C++ header (.h) 文件中进行设置，但函数主体完全在蓝图图表中进行编写，而非 C++ 中。它们创建的目的是使非程序员可针对特殊情况（这些情况不存在默认操作或标准行为）创建自定义响应。范例：在宇宙飞船游戏中玩家飞船获得强化道具时发生的事件。
3. **BlueprintNativeEvent** 函数就像是 BlueprintCallable 和 BlueprintImplementableEvent 函数的组合。它们的默认行为已在 C++ 中完成编程，但通过蓝图图表中的覆写即可对它们进行补充或替换。对它们进行编程时，C++ 代码始终将进入命名尾部添加有 _Implementation 的虚拟函数，如下所示。这是灵活性最高的选项，因此我们将在此教程中使用。







## 日志输出

> [出处Link](https://www.cnblogs.com/blueroses/p/6037981.html)

快速使用 :

```c++
UE_LOG(LogTemp, Warning, TEXT("Your message"));
```



自义定:

头文件中加入

```C++
DECLARE_LOG_CATEGORY_EXTERN(LogMe, Log, All);
```



对应CPP中加入

```C++
DEFINE_LOG_CATEGORY(LogMe);
```



使用输出

```C++
UE_LOG(LogMe, Warning , TEXT("Default Log ."));
```



### Log格式：

### Log Message

```C++
//"This is a message to yourself during runtime!"
UE_LOG(YourLog,Warning,TEXT("This is a message to yourself during runtime!"));
```

### Log an FString

```C++
 %s strings are wanted as TCHAR* by Log, so use *FString()
//"MyCharacter's Name is %s"
UE_LOG(YourLog,Warning,TEXT("MyCharacter's Name is %s"), *MyCharacter->GetName() );
```

### Log an Int

```C++
//"MyCharacter's Health is %d"
UE_LOG(YourLog,Warning,TEXT("MyCharacter's Health is %d"), MyCharacter->Health );
```

### Log a Float

```c++
//"MyCharacter's Health is %f"
UE_LOG(YourLog,Warning,TEXT("MyCharacter's Health is %f"), MyCharacter->Health );
```

### Log an FVector

```C++
//"MyCharacter's Location is %s"
UE_LOG(YourLog,Warning,TEXT("MyCharacter's Location is %s"), 
    *MyCharacter->GetActorLocation().ToString());
```

### Log an FName

```C++
//"MyCharacter's FName is %s"
UE_LOG(YourLog,Warning,TEXT("MyCharacter's FName is %s"), 
    *MyCharacter->GetFName().ToString());
```

### Log an FString,Int,Float

```C++
//"%s has health %d, which is %f percent of total health"
UE_LOG(YourLog,Warning,TEXT("%s has health %d, which is %f percent of total health"),
    *MyCharacter->GetName(), MyCharacter->Health, MyCharacter->HealthPercent);
```







# 静态加载和动态加载

[More About](https://aigo.iteye.com/blog/2281373)







# UMG 

## C++ 与 UMG 交互

[C++ 与 UMG 交互][https://blog.csdn.net/niu2212035673/article/details/82792910]

在使用进行绑定事件时候，绑定的点击方法需要用UFUNCTION宏进行标记。

OnClicked.__Internal_AddDynamic



添加点击事件。

```C++
this->BtnOne->OnClicked.__Internal_AddDynamic(this, &UBaseUI::BtnOneOnClick, FName("BtnOneOnClick"));
```

BtnOneOnClick为添加事件函数。

BtnOne为控件。



或者是

```C++
if (BtnOne != nullptr)
	{
		FScriptDelegate Del;
		Del.BindUFunction(this, "OnBtnChangeImgClick");
		BtnOne->OnClicked.Add(Del);
	}
```





> 额外参考:
>
> [UE4/CPP C++绑定UMG中的按钮事件](https://blog.csdn.net/I_itaiit/article/details/80344864)









# 退出游戏

[参考链接](https://aigo.iteye.com/blog/2291979)

```C++
#include "KismetSystemLibrary.h"  
  
void ASomeActor::SomeActorFunction()  
{  
    UKismetSystemLibrary::QuitGame(this, nullptr, EQuitPreference::Quit);  
}  
```



# UE4 项目结构

[参考链接](https://blog.csdn.net/you_lan_hai/article/details/71943386)

[参考链接](https://blog.csdn.net/a359877454/article/details/53382539)

[参考链接](https://juejin.im/post/584e5c290ce463005c6328f0)



### 文件目录：

Binaries:存放编译生成的结果二进制文件。该目录可以gitignore,反正每次都会生成。

Config:配置文件。

Content:平常最常用到，所有的资源和蓝图等都放在该目录里。

DerivedDataCache：“DDC”，存储着引擎针对平台特化后的资源版本。比如同一个图片，针对不同的平台有不同的适合格式，这个时候就可以在不动原始的uasset的基础上，比较轻易的再生成不同格式资源版本。gitignore。

Intermediate：中间文件（gitignore），存放着一些临时生成的文件。有：

- Build的中间文件，.obj和预编译头等
- UHT预处理生成的.generated.h/.cpp文件
- VS.vcxproj项目文件，可通过.uproject文件生成编译生成的Shader文件。
- AssetRegistryCache：Asset Registry系统的缓存文件，Asset Registry可以简单理解为一个索引了所有uasset资源头信息的注册表。CachedAssetRegistry.bin文件也是如此。

Saved：存储自动保存文件，其他配置文件，日志文件，引擎崩溃日志，硬件信息，烘培信息数据等。gitignore

Source：代码文件。





# UE4 文件配置

[参考链接](https://blog.csdn.net/u012999985/article/details/52801264)









# ModuleRules

[参考链接](https://qiita.com/takayashiki2/items/db995c558024c3db8223)

[参考链接](https://zhuanlan.zhihu.com/p/45398694)

[参考链接](http://gad.qq.com/article/detail/14451)









# 输入绑定



1. 设置按键

![1545119180169.png](.\uenotepic\1545119180169.png)



2.绑定代码

```C++
InputComponent->BindAxis("MoveY", this, &AMyPawn::Move_YAxis);

PlayerInputComponent->BindAction("Action00", IE_Pressed,this, &AFirstPawn::StartGrowing);

//InputComponent 为SetupPlayerInputComponent函数中的UInputComponent参数组件

```



# 计时器的使用



### 头文件

```C++
#include "TimerManager.h"
```

### FTimerManager::SetTimer



```C++
void SetTimer
(
    FTimerHandle & InOutHandle,
    TFunction < void(void)> && Callback,
    float InRate,
    bool InbLoop,
    float InFirstDelay
)
```



> 可以通过GetWorldTimerManager()获取FTimerManager类





# 移动组件 Pawn Movement Components

**Pawn Movement Components（Pawn移动组件）** 有一些强劲的、内置的功能可用于辅助常见的物理功能，同时还是一个在许多 **Pawn** 类型间共享移动代码的好方法。 随着您的项目变得越来越大， **Pawns** 变得越来越复杂，使用 **Components（组件）** 来隔离功能是一个很好的方法。



# GameMode 创建**Widgets**

[Link](https://api.unrealengine.com/CHN/Programming/Tutorials/UMG/2/index.html)

```c++
void AHowTo_UMGGameMode::ChangeMenuWidget(TSubclassOf<UUserWidget> NewWidgetClass)
{
    if (CurrentWidget != nullptr)
    {
        CurrentWidget->RemoveFromViewport();
        CurrentWidget = nullptr;
    }
    if (NewWidgetClass != nullptr)
    {
        CurrentWidget = CreateWidget<UUserWidget>(GetWorld(), NewWidgetClass);
        if (CurrentWidget != nullptr)
        {
            CurrentWidget->AddToViewport();
        }
    }
}
```



> 这个代码将会创建我们提供的任意 **Widgets（控件）** 的实例，并将其放置于屏幕上。 它也可以用来移除实例，所以即使 **Unreal Engine（虚幻引擎）** 可以同时处理许多 **Widgets（控件）** 的显示和互动，一次也只能激活一个实例。 我们不需要直接销毁 **Widgets（控件）** ，因为将其从视口中移除并清除（或者变更）引用它的所有变量将会导致其被 **Unreal Engine's（虚幻引擎的）** 垃圾收集系统清除。







# Header And Class



## GameplayStatics

### GameplayStatics.h

```C++
#include "Kismet/GameplayStatics.h"
```

GameplayStatics头文件让我们可以访问一些有用的通用函数。



### UGameplayStatics类

[Link](https://api.unrealengine.com/INT/API/Runtime/Engine/Kismet/UGameplayStatics/GetPlayerController/index.html)

UGameplayStatics::GetPlayerController(this, 0)；

```c++
static APlayerController * GetPlayerController
(
    const UObject * WorldContextObject,
    int32 PlayerIndex
)
```



Remarks :

> Returns the player controller at the specified player index



## PlayerController

### PlayerController.h

```C++
#include "GameFramework/PlayerController.h"
```



### APlayerController类

#### APlayerController::GetViewTarget

```C++
virtual AActor * GetViewTarget()
```



Remarks :

>  Get the actor the controller is looking at



#### APlayerController::SetViewTargetWithBlend



```C++
virtual void SetViewTargetWithBlend
(
    class AActor * NewViewTarget,
    float BlendTime,
    enum EViewTargetBlendFunction BlendFunc,
    float BlendExp,
    bool bLockOutgoing
)
```



Remarks :

> Set the view target blending with variable control

| Parameter     | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| NewViewTarget | new actor to set as view target                              |
| BlendTime     | time taken to blend                                          |
| BlendFunc     | Cubic, Linear etc functions for blending                     |
| BlendExp      | Exponent, used by certain blend functions to control the shape of the curve. |
| bLockOutgoing | If true, lock outgoing viewtarget to last frame's camera position for the remainder of the blend. |





### UPawnMovementComponent

#### UPawnMovementComponent::ConsumeInputVector

```C++
virtual FVector ConsumeInputVector()
```



Remarks :

> Returns the pending input vector and resets it to zero. This should be used during a movement update (by the Pawn or PawnMovementComponent) to prevent accumulation of control input between frames. Copies the pending input vector to the saved input vector (GetLastMovementInputVector()).





### AActor



#### AActor::GetActorLocation

> Returns the location of the RootComponent of this Actor

#### AActor::SetActorLocation

> Move the actor instantly to the specified location.
>
> Whether the location was successfully set if not swept, or whether movement occurred if swept.

```c++
bool SetActorLocation
(
    const FVector & NewLocation,
    bool bSweep,
    FHitResult * OutSweepHitResult,
    ETeleportType Teleport
)
```





### FObjectInitializer

#### FObjectInitializer::CreateDefaultSubobject

Create a component or subobject。


### FMath

#### FMath::Clamp

限制数值范围。

