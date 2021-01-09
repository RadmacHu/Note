#### Unity中Shader

##### Unity Shader 类型

+ Standard Surface Shader
+ Unlit Shader
+ Image Effect Shader
+ Compute Shader



> Standard Surface Shader 会产生一个包含标准光照模型的表面着色器模板
>
> Unlit Shader 会产生一个不包含光照（但包含雾效）的基本顶点 / 片元着色器
>
> Image Effect Shader 各种屏幕后处理效果
>
> Compute Shader  是在GPU运行却又在普通渲染管线之外的程序，通过Compute Shader我们可以将大量可以并行的计算放到GPU中计算从而节省CPU资源



##### Unity Shader 基本格式

```
Shader "ShaderName"
{
    Properties
    {
        // 所需的各种属性
    }
    SubShader
    {
        // 真正意义上的Shader代码在这里
        // 表面着色器或者顶点/片元着色器或者固定函数着色器
    }
    SubShader
    {
        // 同上SubShader
    }
    Fallback "VertexLit"
}
```



##### Properties 属性块

```
Properties
{
    Name ("display name" , PropertyType) = DefaultValue
    // 栗子
    _Metallic ("Metallic", Range(0,1)) = 0.0
}
```



###### Properties 属性基本类型

| 属性类型 |       默认值的定义语法        | 例 子                                    |
| :------: | :---------------------------: | :--------------------------------------- |
|   Int    |            number             | _Int("Int" , Int) = 2                    |
|  Float   |            number             | _Float ("Float" , Float) = 1.5           |
|  Range   |            number             | _Range("Range" , Range(0.0 , 5.0)) = 3.0 |
|  Color   | (number,number,number,number) | _Color ("Color" , Color) = (1,1,1,1)     |
|  Vector  | (number,number,number,number) | _Vector ("Vector" , Vector) = (2,3,6,1)  |
|    2D    |      "defaulttexture"{}       | _2D("2D" , 2D) = "" {}                   |
|   Cube   |      "defaulttexture"{}       | _Cube("Cube" , Cube) = "white" {}        |
|    3D    |      "defaulttexture"{}       | _3D ("3D" , 3D) = "black" {}             |



##### SubShader 块

```
SubShader {
	// 可选
	[Tags]
	
	// 可选
	[RenderSetup]
	Pass {
        
	}
	// Other Pass
}
```



###### RenderSetup 渲染状态设置

| 状态名称 |                          设置指令                           |                 解释                  |
| :------- | :---------------------------------------------------------: | :-----------------------------------: |
| Cull     |                  Cull Back \| Font \| Off                   | 设置剔除模式 : 剔除背面/正面/关闭剔除 |
| ZTest    | ZTest Less Greater\|LEqual\|GEqual\|Equal\|NotEqual\|Always |       设置深度测试相关使用函数        |
| ZWrite   |                      ZWrite On \| Off                       |           开启/关闭深度写入           |
| Blend    |                  Blend SrcFactor DstFactor                  |          开启并设置混合模式           |

在SubShader块内设置渲染状态会应用到所有的Pass。如果需要某个Pass起作用可以单独在Pass块中设置。



###### Tags 标签

 ```
// Tags 格式
Tags { "Tag1" = "Tag1Value" , "Tag2" = "Tag2Value" }
 ```

标签形式是Key-Value形式。

SubShader 的标签类型



// 后面有空再补上表格

| 标签类型             | 说明 | 例子 |
| -------------------- | ---- | ---- |
| Queue                |      |      |
| RenderType           |      |      |
| DisableBatching      |      |      |
| ForceNoShadowCasting |      |      |
| IgnoreProjector      |      |      |
| CanUseSpriteAtlas    |      |      |
| PreviewType          |      |      |



##### Pass 语义块

```
Pass
{
    [Name]
    [Tags]
    [RenderSetup]
}
```



###### Pass 名称

```
// 定义名字
Name "PassName"

// 通过UsePass命令直接使用其他Unity Shader中的Pass
UsePass "MyShader/OTHERPASSNAME"

// Unity内部会把所有的Pass转换为名称大写。

```



###### Pass 标签

| 标签类型       | 说明 | 例子 |
| -------------- | ---- | ---- |
| LightMode      |      |      |
| RequireOptions |      |      |



> Unity Shader还支持特殊的Pass。方便代码复用或者实现更复杂的效果。

+ UsePass :
+ GrabPass :



##### Fallback

```
Fallback "name"

Fallback Off
```





##### 表面着色器

```
Shader "Custom/stander_surface_shader"
{
    SubShader
    {
        Tags { "RenderType"="Opaque" }
        
        CGPROGRAM
        #pragma surface surf Lambert
        struct Input
        {
            float4 color : COLOR;
        };

        void surf (Input IN, inout SurfaceOutput o)
        {
            o.Alpha = 1;
        }
        ENDCG
    }
    FallBack "Diffuse"
}

```



##### 顶点/片元着色器

```
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "Custom/simple_vertexfragment_shader"
{
    SubShader
    {
       Pass
       {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            float4 vert(float4 v : POSITION) : SV_POSITION {
                return UnityObjectToClipPos ( v);
            }

            fixed4 frag() : SV_TARGET
            {
                return fixed4 (1.0 , 0.0 , 0.0 , 1.0);
            }
            ENDCG
       }
    }
}
```





#### 矩阵 Matrix



3 X 4 矩阵 :
$$
\left[
\begin{matrix}
1 & 0.5 & 3 & 2 \\
0 & 0 & 0 & 1 \\
0 & 1 & 0 & 2 \\
\end{matrix} 
\right]
$$


$$m_{ij} ​$$为这个元素在 M 的第i行、第j行。
$$
M = \left[
\begin{matrix}
m_{11} & m_{12} & m_{13} \\
m_{21} & m_{22} & m_{23} \\
m_{31} & m_{32} & m_{33} \\
\end{matrix} 
\right]
$$


--- 待补充





