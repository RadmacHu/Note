## Unity中Shader

### Unity Shader 类型

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



### Unity Shader 基本格式

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



#### Properties 属性块

```
Properties
{
    Name ("display name" , PropertyType) = DefaultValue
    // 栗子
    _Metallic ("Metallic", Range(0,1)) = 0.0
}
```



##### Properties 属性基本类型

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



#### Subshader 块

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



#### RenderSetup 渲染状态设置

| 状态名称 |                          设置指令                           |                 解释                  |
| :------- | :---------------------------------------------------------: | :-----------------------------------: |
| Cull     |                  Cull Back \| Font \| Off                   | 设置剔除模式 : 剔除背面/正面/关闭剔除 |
| ZTest    | ZTest Less Greater\|LEqual\|GEqual\|Equal\|NotEqual\|Always |       设置深度测试相关使用函数        |
| ZWrite   |                      ZWrite On \| Off                       |           开启/关闭深度写入           |
| Blend    |                  Blend SrcFactor DstFactor                  |          开启并设置混合模式           |

在SubShader块内设置渲染状态会应用到所有的Pass。如果需要某个Pass起作用可以单独在Pass块中设置。



#### Tags 标签

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



#### Pass 语义块

```
Pass
{
    [Name]
    [Tags]
    [RenderSetup]
}
```



##### Pass 名称

```
// 定义名字
Name "PassName"

// 通过UsePass命令直接使用其他Unity Shader中的Pass
UsePass "MyShader/OTHERPASSNAME"

// Unity内部会把所有的Pass转换为名称大写。

```



##### Pass 标签

| 标签类型       | 说明 | 例子 |
| -------------- | ---- | ---- |
| LightMode      |      |      |
| RequireOptions |      |      |



> Unity Shader还支持特殊的Pass。方便代码复用或者实现更复杂的效果。

+ UsePass :
+ GrabPass :



#### Fallback

```
Fallback "name"

Fallback Off
```





### 表面着色器

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



### 顶点/片元着色器

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





### 矩阵 Matrix



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


$$m_{ij} $$为这个元素在 M 的第i行、第j行。
$$
M = \left[
\begin{matrix}
m_{11} & m_{12} & m_{13} \\
m_{21} & m_{22} & m_{23} \\
m_{31} & m_{32} & m_{33} \\
\end{matrix} 
\right]w
$$

--- 待补充



#### 矩阵几何意义:变换

##### 齐次坐标

> 齐次坐标 => 笛卡尔
> $$
> (x , y , w) <=> (\frac{x}{w} , \frac{y}{w})
> $$
>



##### 分解基础变换矩阵

可以使用一个$$4\times 4$$的矩阵来表示平移、旋转和缩放。

纯平移、纯旋转、纯放缩的变换矩阵称为基础变换矩阵。共同点可以分解为4部分。
$$
\left[
\begin{matrix}
	M_{3\times3} & t_{3\times1} \\
    0_{1 \times3} & 1
\end{matrix}
\right]
$$


> $$ M_{3\times3}​$$用于表示放缩和旋转。
> $$t_{3\times1}​$$用于表示平移。
> $$0_{1\times3}​$$是零矩阵。



##### 平移矩阵

为点 (x , y , z) 在空间中平移了 ($$t_x$$ , $$t_y$$, $$t_z$$) 个单位。
$$
\left[
\begin{matrix}
1 & 0 & 0 & t_x \\
0 & 1 & 0 & t_y \\
0 & 0 & 1 & t_z \\
0 & 0 & 0 & 1 \\
\end{matrix}
\right]

\left[

\begin{matrix}
x \\
y \\
z \\
1
\end{matrix}
\right]

= 
\left[
\begin{matrix}
x + t_x \\
y + t_y \\
z + t_z \\
1
\end{matrix}
\right]
$$


为矢量(x,y,z)在空间做平移


$$
\left[
\begin{matrix}
1 & 0 & 0 & t_x \\
0 & 1 & 0 & t_y \\
0 & 0 & 1 & t_z \\
0 & 0 & 0 & 1 \\
\end{matrix}
\right]

\left[

\begin{matrix}
x \\
y \\
z \\
0
\end{matrix}
\right]

= 
\left[
\begin{matrix}
x + t_x \\
y + t_y \\
z + t_z \\
0
\end{matrix}
\right]
$$

>平移不会对方向矢量产生任何影响。
>
>平移矩阵的逆矩阵就是反向平移得到的矩阵。

$$
\left[
\begin{matrix}
1 & 0 & 0 & -t_x \\
0 & 1 & 0 & -t_y \\
0 & 0 & 1 & -t_z \\
0 & 0 & 0 & 1 \\

\end{matrix}
\right]
$$



##### 放缩矩阵

为一个模型验证x、y、z三个轴进行放缩。

$$
\left[
\begin{matrix}
k_x & 0 & 0 & 0 \\
0 & k_y & 0 & 0 \\
0 & 0 & k_z & 0 \\
0 & 0 & 0 & 1 \\
\end{matrix}
\right]

\left[
\begin{matrix}
x \\
y \\
z \\
0
\end{matrix}
\right]

= 
\left[
\begin{matrix}
k_{x}x \\
k_{y}y \\
k_{z}z \\
1 
\end{matrix}
\right]
$$

如果$$k_x​$$=$$k_y​$$=$$k_z​$$时候，称为 统一放缩。否则称为非统一放缩。



##### 旋转矩阵

绕x轴旋转旋转矩阵 :
$$
R_x(\theta)  =

\left[
\begin{matrix}
1 & 0 & 0 & 0 \\
0 & \cos\theta& -\sin\theta & 0 \\
0 & \sin\theta & \cos\theta & 0 \\
0 & 0 & 0 & 1 \\
\end{matrix}
\right]
$$



绕y轴旋转旋转矩阵 :
$$
R_x(\theta)  =
\left[
\begin{matrix}
\cos\theta & 0 & \sin\theta & 0 \\
0 & 1 & 0 & 0 \\
-\sin\theta & 0 & \cos\theta & 0 \\
0 & 0 & 0 & 1
\end{matrix}
\right]
$$



绕z轴旋转旋转矩阵 :
$$
R_x(\theta)  =

\left[
\begin{matrix}
\cos\theta & -\sin\theta & 0 & 0 \\
\sin\theta & \cos\theta & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{matrix}
\right]
$$



> 旋转矩阵的逆矩阵是旋转相反角度得到的变换矩阵。
>
> 旋转矩阵是正交矩阵。



##### 复合变换



变换过程：

$$
P_{new} = M_{translation}M_{rotation}M_{scal\theta}P_{old}
$$


>约定变换顺序:
>
>先放缩，再旋转，最后再平移。



#### 坐标空间

假设， 父空间P、子空间C。在父空间中，子空间的原点位置已经3个单位坐标轴。

> 需求1：
>
> 把子坐标空间下表示的点或矢量$$A_c$$转换的父坐标空间下的表示$$A_p​$$。
>
> $$
> A_p = M_{c→p}A_c
> $$
>
> 需求2：
>
> 把父坐标空间下表示的点和矢量$$B_p$$转换到子坐标空间下的表示$$B_c$$。
> $$
> B_c = M_{p→c}B_p
> $$

#### 模型空间

别称对象空间、局部空间。



#### 世界空间

特殊的坐标系。

> 顶点变换第一步:
>
> 模型变换（model transform）：
>
> 顶点坐标→模型空间坐标→世界空间坐标。



#### 观察空间

又称摄像机空间。

> 观察空间是三维空间。屏幕空间是二维的。从观察空间到屏幕空间的转换是要经过**投影**操作的。
>
> 顶点变换第二步:
>
> 观察变换（view transform）：
>
> 将顶点坐标从世界空间坐标变换到观察空间坐标。

#### 裁剪空间(clip space)

又称齐次裁剪空间

这个用于变换的矩阵叫做**裁剪矩阵（clip matrix）**，也称**投影矩阵（projection matrix）**。

> 裁剪空间的目的是能够方便地对渲染图元进行裁剪：完全位于这块空间的图元将会保留，以外的将会被删除，而与这块空间边界相交的图元就会被裁剪。这些都由视锥体决定。



1.透视投影 (FOV)

![1610416053816](.\Unity Note Graphic.assets\1610416053816.png)


$$
nearClipPlaneHeight = 2 \times Near \times  \tan \frac{FOV }{2}
$$

$$
farClipPlaneHeight = 2 \times Far \times \tan \frac{FOV}{2}
$$

