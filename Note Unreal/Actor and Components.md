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





# 创建Component

自定义的Actor没有Location，并且不能够Attach其他Actor上。

```c++
Mesh = CreateDefaultSubobject<UStaticMeshComponent>("BaseMeshComponent");
```





# FObjectFinder加载资源【构造函数中加载】

在构造函数中进行资源加载。**注意不能在BeginPlay中使用**

```c++
// 加载资源
auto MeshAsset = 
ConstructorHelpers::
FObjectFinder<UStaticMesh>(TEXT("Static Mesh'/Engine/BasicShapes/Cube.Cube'")); 
	
if (MeshAsset.Object != nullptr) {
    Mesh->SetStaticMesh(MeshAsset.Object);
}
```

