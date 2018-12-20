
# Materials  相关

------

## Materials Useage Flag

[ Link ][https://docs.unrealengine.com/en-US/Engine/Rendering/Materials/MaterialProperties]

出现问题: SetMaterial后相关的材质效果没有生效。（材质资源存在）

可能是材质的Useage Flag设置不合法。在Usage Flag设置不合法的情况下，引擎会用默认材质来替代。

![img](https://docs.unrealengine.com/portals/0/images/Engine/Rendering/Materials/MaterialProperties/UsageFlagProperties.png)

| Property                              | Description                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| **Used with Skeletal Mesh**           | Set this if the Material will be placed on a Static Mesh.    |
| **Used with Editor Compositing**      | Set if the Material will be used with the Editor UI.         |
| **Used with Landscape**               | Set if the Material will be used on a Landscape surface.     |
| **Used with Particle Sprites**        | Used if this Material will be placed on a Particle System.   |
| **Used with Beam Trails**             | Set if the Material will be used with Beam Trails.           |
| **Used with Mesh Particles**          | Indicates that the Material and its instances can be used with mesh particles. This will result in the shaders required to support mesh particles being compiled which will increase shader compile time and memory usage. |
| **Used with Static Lighting**         | This is set if the Material will be considered for Static Lighting, such as if it uses an Emissive effect that should affect lighting. |
| **Used with Fluid Surfaces**          | Fluid surfaces are no longer supported in Unreal Engine 4. This option will soon be removed. |
| **Used with Morph Targets**           | Set if this Material will be applied to a Skeletal Mesh that utilizes a Morph Target. |
| **Used with Spline Meshes**           | Set if the Material will be applied to Landscape Spline meshes. |
| **Used with Instanced Static Meshes** | Set if the Material is intended to be applied to Instanced Static Meshes. |
| **Used with Distortion**              | Distortion is no longer supported (it is now Refraction) and this option will soon be removed. |
| **Used with Clothing**                | This should be set if the Material will be applied to Apex physically simulated clothing. |
| **Used with UI**                      | This indicates that this Material and any Material Instances can be used with Slate UI and UMG. |
| **Automatically Set Usage in Editor** | Whether to automatically set usage flags based on what the Material is applied to in the Editor. The default option for this is enabled. |



