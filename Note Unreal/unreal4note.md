# Unreal Property System (Reflection)

## [Link][https://www.unrealengine.com/zh-CN/blog/unreal-property-system-reflection]



头文件

```c++
#include "FileName.generated.h"
```





### UENUM()

### UCLASS()

### USTRUCT()

### UFUNCTION()

### UPROPERTY()



#  PlayerStart 物体

> 出生点生成位置

# Actor and Pawn

[Link][https://home.gamer.com.tw/creationDetail.php?sn=3417295]

在Unity裡這種物件叫做**GameObject**，再透過內建的或是自己寫的Component掛在GameObject上來讓該物體有功能性；而在Unreal裡則是稱作**Actor**，也能跟Unity一樣掛上Component來讓Actor具有更多的功能。



然而這Unity和UE對於物件的定義上最大的差別就在於 :

- Unity的GameObject有內建Transform，沒有Transform的物件無法形成GameObject。**UE的Actor則沒有內建Transform，雖然它代表一個物體，但是不存在於3D場景的特定座標**
- Unity的父子階層建構是依靠Transform底下連結其他GameObject的Transform
  UE的父子階層建構則是透過SceneComponent來連結子物件



# Some Porject

[1. CoinCollector Example][https://www.raywenderlich.com/185-unreal-engine-4-c-tutorial]



------

# UMG, How to extend a UUserWidget:: for UMG in C++.

> [Link][https://wiki.unrealengine.com/UMG,_How_to_extend_a_UUserWidget::_for_UMG_in_C%2B%2B.]

## **Step **

###  1. Add Header Files To Project

> You add several headers to your project header. "(YourProjectName.h)".



```c++
#include "Runtime/UMG/Public/UMG.h"
#include "Runtime/UMG/Public/UMGStyle.h"
#include "Runtime/UMG/Public/Slate/SObjectWidget.h"
#include "Runtime/UMG/Public/IUMGModule.h"
#include "Runtime/UMG/Public/Blueprint/UserWidget.h"
```

###  2. Adding Modules

> And then i also made sure that the UMG and Slate Modules was included as well. In "(YourProjectName.Build.cs)":

```C++
PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "UMG", "Slate", "SlateCore"})
```

3. Compile

   > Compile and re-open the editor. And add a new class to the project derived from [UUserWidget::](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Blueprint/UUserWidget/index.html)
   >
   > *If you were wondering how the header of the derived class looks like after created/ added from the editor.*



```c++
#pragma once
    
#include "Blueprint/UserWidget.h"
#include "InventoryWidget.generated.h"
   
UCLASS()
class MYGAME_API UInventoryWidget : public UUserWidget
{
       GENERATED_UCLASS_BODY()
};
```



