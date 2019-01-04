# Input and Collision

[TOC]

## 输入绑定



1. 设置按键

![1545119180169.png](C:/Users/Raytine/Desktop/note/Note/Note%20Unreal/uenotepic/1545119180169.png)



2.绑定代码

```C++
InputComponent->BindAxis("MoveY", this, &AMyPawn::Move_YAxis);

PlayerInputComponent->BindAction("Action00", IE_Pressed,this, &AFirstPawn::StartGrowing);

//InputComponent 为SetupPlayerInputComponent函数中的UInputComponent参数组件

```



## 代码中添加输入Mapping



获取输入组件

```c++
GetWorld()->GetFirstPlayerController()->PlayerInput
```

或者是

```c++
UPlayerInput::AddEngineDefinedAxisMapping() UPlayerInput::AddEngineDefinedActionMapping()
```



建立Mapping变量

```c++
FInputAxisKeyMapping DanceAxisCmd("DanceAxis" ,EKeys::K , 1.0f);

FInputActionKeyMapping JumpCmd("Jump", EKeys::SpaceBar, 0, 0, 0, 0);
```



添加Mapping

```c++
GetWorld()->GetFirstPlayerController()->
    PlayerInput ->AddAxisMapping( DanceAxisCmd ); 
// specific to a controller
```

或者

```c++
UPlayerInput::AddEngineDefinedActionMapping(JumpCmd);
```



最后在角色进行绑定

```c++
Input->BindAxis("Back", this, &AWarrior::Back); 
Input->BindAction("Jump", IE_Pressed, this, &AWarrior::Jump );
```





## Collision – letting objects pass through one another using Ignore

* Ignore: Collisions that pass through each other without any notification.
* Overlap: Collisions that trigger the OnBeginOverlap and OnEndOverlap events. Interpenetration of objects with an Overlap setting is allowed.
* Block: Collisions that prevent all interpenetration, and prevent objects from overlapping each other at all.



##  碰撞事件实现



[https://unrealcpp.com/on-overlap-begin/](https://unrealcpp.com/on-overlap-begin/)



定义事件

```c++
UFUNCTION(BlueprintNativeEvent, Category = Collision) void OnOverlapsBegin( UPrimitiveComponent* Comp, AActor* OtherActor,
UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult );


//In UE 4.13, the OnOverlapsBegin function's signature has changed to: 

OnOverlapsBegin( UPrimitiveComponent* Comp, AActor* OtherActor,UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitREsult& SweepResult );

```



绑定事件

```c++
GetCapsuleComponent()->OnComponentBeginOverlap.AddDynamic( this, &AWarrior::OnOverlapsBegin );

GetCapsuleComponent()
->OnComponentEndOverlap.AddDynamic( this, &AWarrior::OnOverlapsEnd );
```

