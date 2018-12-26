# Actor and Components



## Spawn Actor

创建Actor via code.

```C++
FTransform SpawnLocation;
GetWorld()->SpawnActor<AMyFirstActor>( AMyFirstActor::StaticClass(), &SpawnLocation);
```



SetLifeSpan实现延迟销毁。

```c++
SetLifeSpan(sec);
// 会在sec秒后调用Destroy
```





## 创建Component -- CreateDefaultSubobject

自定义的Actor没有Location，并且不能够Attach其他Actor上。

```c++
Mesh = CreateDefaultSubobject<UStaticMeshComponent>("BaseMeshComponent");
```





## FObjectFinder加载资源【构造函数中加载】

在构造函数中进行资源加载。**注意不能在BeginPlay中使用**

路径可以通过在内容浏览器，右击资源，选择复制引用获得。

```c++
// 加载资源
auto MeshAsset = 
ConstructorHelpers::
FObjectFinder<UStaticMesh>(TEXT("Static Mesh'/Engine/BasicShapes/Cube.Cube'")); 
	
if (MeshAsset.Object != nullptr) {
    Mesh->SetStaticMesh(MeshAsset.Object);
}
```



## 函数方法UFUNCTION

对相关的方法用宏进行标记。

```C++
UFUNCTION()
int32 GetScore();
```



## 附加组件

```c++
BoxTwo->AttachTo(ChildSceneComponent);
BoxTwo->SetupAttachment(ChildSceneComponent);
```



## 创建自定义Component

创建类继承UActorComponent。

> Actor components are an easy way to implement common functionality that should be shared between Actors. Actor components aren't rendered, but can still perform actions such as subscribing to events, or communicating with other components of the Actor that they are present within.



## 创建自定义SceneCompent

创建类继承USceneComponent。

> Scene Components are a subclass of Actor Components that have a transform, that is, a relative location, rotation, and scale. Just like Actor Components, Scene Components aren't rendered themselves, but can use their transform for various things, such as spawning other objects at a fixed offset from an Actor.



## 创建自定义PrimitiveComponent



> Primitive components are the most complex type of Actor Component because **they not only have a transform, but are also rendered on screen**

