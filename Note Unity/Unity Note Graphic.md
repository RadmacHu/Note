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

##### 法线变换

用$$M_{A→B}$$来变换顶点。
$$
T_B \cdot N_B = (M_{A-B}T_A)\cdot(GN_A) = 0
$$
其中$$T_A$$、$$T_B$$分别表示坐标空间A下和坐标空间B下的切线方向。G为变换矩阵。
$$
G = (M^T_{A→B})^{-1} = (M^{-1}_{A→B})^{T}
$$


如果变换只包括旋转和统一缩放，而不包括非统一缩放。利用统一缩放系数k。
$$
(M^T_{A→B})^{-1} = \frac{1}{k} M_{A→B}
$$






### 坐标空间

假设， 父空间P、子空间C。在父空间中，子空间的原点位置已经3个单位坐标轴。

> 需求1：
>
> 把子坐标空间下表示的点或矢量$$A_c$$转换的父坐标空间下的表示$$A_p​$$。
>
> $$
> A_p = M_{c→p}A_cw
> $$
>
> 需求2：
>
> 把父坐标空间下表示的点和矢量$$B_p$$转换到子坐标空间下的表示$$B_c$$。
> $$
> B_c = M_{p→c}B_p
> $$

### 模型空间

别称对象空间、局部空间。



### 世界空间

特殊的坐标系。

> 顶点变换第一步:
>
> 模型变换（model transform）：
>
> 顶点坐标→模型空间坐标→世界空间坐标。



### 观察空间

又称摄像机空间。

> 观察空间是三维空间。屏幕空间是二维的。从观察空间到屏幕空间的转换是要经过**投影**操作的。
>
> 顶点变换第二步:
>
> 观察变换（view transform）：
>
> 将顶点坐标从世界空间坐标变换到观察空间坐标。

### 裁剪空间(clip space)

又称齐次裁剪空间

这个用于变换的矩阵叫做**裁剪矩阵（clip matrix）**，也称**投影矩阵（projection matrix）**。

> 裁剪空间的目的是能够方便地对渲染图元进行裁剪：完全位于这块空间的图元将会保留，以外的将会被删除，而与这块空间边界相交的图元就会被裁剪。这些都由视锥体决定。



#### 1.透视投影 (FOV)

![1610416053816](.\Unity Note Graphic.assets\1610416053816.png)


$$
nearClipPlaneHeight = 2 \times Near \times  \tan \frac{FOV }{2}
$$

$$
farClipPlaneHeight = 2 \times Far \times \tan \frac{FOV}{2}
$$



当前摄像机横纵比为Aspect。

$$
Aspect = \frac{nearClipPlaneWidth}{nearClipPlaneHeight}
$$

$$
Aspect = \frac{farClipPlaneWidth}{farClipPlaneHeight}
$$



Unity坐标系下的投影矩阵。

$$
M_{frustum} = 
\left[
\begin{matrix}
\frac{\cot\frac{FOV}{2}}{Aspect} & 0 & 0 & 0 \\
0 & \cot\frac{FOV}{2} & 0 & 0 \\
0 & 0 & -\frac{Far + Near}{Far - Near} & -\frac{2 \times Near\times Far}{Far - Near} \\
0 & 0 & -1 & 0 \
\end{matrix}
\right]
$$


一个顶点和上述投影相乘。

$$
P_{clip} = M_{frustum}P_{view}=
\left[
\begin{matrix}
\frac{\cot\frac{FOV}{2}}{Aspect} & 0 & 0 & 0 \\
0 & \cot\frac{FOV}{2} & 0 & 0 \\
0 & 0 & -\frac{Far + Near}{Far - Near} & -\frac{2 \times Near\times Far}{Far - Near} \\
0 & 0 & -1 & 0 \
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
x\frac{\cot\frac{FOV}{2}}{Aspect} \\
y\cot\frac{FOV}{2} \\
-z\frac{Far + Near}{Far - Near} - \frac{2 \times Near \times Far}{Far - Near} \\
-z \\
\end{matrix}
\right]
$$

顶点的w分量不再是1。而是z分量取反的结果。顶点在视锥体内，那么它变换后的坐标满足：

$$
-w \leq x \leq w \\
-w \leq y \leq w \\
-w \leq z \leq w
$$
不满足上述条件的图元都需要被剔除或者裁剪。

#### 2.正交投影

![1610419377269](.\Unity Note Graphic.assets\1610419377269.png)


$$
nearClipPlaneHeight = 2 \cdot Size \\
farClipPlaneHeight = nearClipPlaneHeight
$$
摄像机横纵比为Aspect。
$$
nearClipPlaneWidth = Aspect \cdot nearClipPlaneHeight
$$


Unity坐标系下的正交投影矩阵的裁剪矩阵：
$$
M_{ortho} = 
\left[
\begin{matrix}
\frac{1}{Aspect \cdot Size} & 0 & 0 & 0 \\
0 & \frac{1}{Size} & 0 & 0 \\
0 & 0 & -\frac{2}{Far - Near} & -\frac{Far + Near}{Far - Near} \\
0 & 0 & 0 & 1 \\
\end{matrix}
\right]
$$
顶点和投影矩阵相乘：
$$
P_{clip} =
M_{ortho}P_{view} =
\left[
\begin{matrix}
\frac{1}{Aspect \cdot Size} & 0 & 0 & 0 \\
0 & \frac{1}{Size} & 0 & 0 \\
0 & 0 & -\frac{2}{Far - Near} & -\frac{Far + Near}{Far - Near} \\
0 & 0 & 0 & 1 \\
\end{matrix}
\right]

\left[
\begin{matrix}
x \\
y \\
z \\
1 \\
\end{matrix}
\right]
=
\left[
\begin{matrix}
\frac{x}{Aspect \cdot Size} \\
\frac{y}{Size} \\
-\frac{2z}{Far - Near} - \frac{Far + Near}{Far - Near} \\
1 \\
\end{matrix}
\right]
$$


### 屏幕空间

经过投影变换后，可以进行裁剪操作了。裁剪完成后，就需要把视锥体投影到屏幕空间。经过这一步会得到真正的像素位置。

第一步是齐次除法，又称透视除法。

> 这个步骤下，其实就是用齐次坐标系的w分量去除以x、y、z分量。在OpenGL中这一步得到的坐标称为**归一化的设备坐标**。
>
> 按照传统OpenGL下，x、y、z分量范围都是[-1 , 1]。但是，DirectX这样的API中，z分量的范围会是[0 , 1]。而Unity选择了OpenGL这样的齐次裁剪空间。

![1610421611761](.\Unity Note Graphic.assets\1610421611761.png)

![1610421793957](Unity Note Graphic.assets\1610421793957.png)



在Unity中，屏幕空间左下角的像素坐标是(0,0),右上角是(pixelWidth , pixelHeight)。x、y坐标都是[-1, 1]。
$$
screen_x = \frac{clip_x \cdot pixelWidth}{2 \cdot clip_w} + \frac{pixelWidth}{2} \\
screen_y = \frac{clip_y \cdot pixelHeight}{2 \cdot clip_w} + \frac{pixelHeight}{2} \\
$$
上面处理了x、y。z分量通常会用于**深度缓冲**。一个传统的方式是把$$\frac{clip_z}{clip_w}$$的值存进深度缓冲中。在Unity中，裁剪空间转换到屏幕空间是自动完成的。顶点着色器只需要把顶点转换到裁剪空间即可。



### 顶点空间转换总结

顶点着色器的最基本的任务就是把顶点坐标从模型空间转换到裁剪空间。而在片元着色器中，我们通常也可以得到片元在屏幕空间的像素坐标。

![1610422805202](Unity Note Graphic.assets\1610422805202.png)

Unity坐标系的旋向性会随着变换发生改变。![1610423019001](Unity Note Graphic.assets\1610423019001.png)

### Unity Shader的内置变量

#### 变换矩阵

> 类型都是float4x4类型

| 变量名             | 描述 |
| ------------------ | ---- |
| UNITY_MARTIX_MVP   |      |
| UNITY_MARTIX_MV    |      |
| UNITY_MARTIX_V     |      |
| UNITY_MARTIX_P     |      |
| UNITY_MARTIX_VP    |      |
| UNITY_MARTIX_T_MV  |      |
| UNITY_MARTIX_IT_MV |      |
| _Object2World      |      |
| _World2Object      |      |



#### 摄像机和屏幕参数

|                             | 类型     | 描述 |
| --------------------------- | -------- | ---- |
| _WorldSpaceCameraPos        | float3   |      |
| _ProjectionParams           | float4   |      |
| _ScreenParams               | float4   |      |
| _ZBufferParams              | float4   |      |
| unity_OrtoParams            | float4   |      |
| unity_CameraProjection对应  | float4x4 |      |
| unity_CameraInvProjection   | float4x4 |      |
| unity_CameraWorldClipPlanes | float4   |      |



#### CG中的矢量和矩阵类型

在CG中，矩阵类型由float3x3、float4x4等关键字进行声明和定义的。对应float3、float4等类型的变量。可以当做个矢量，也可以当做矩阵。

```
float4 a = float4(1.0 , 2.0 , 3.0 , 4.0);
float4 b = float4(1.0 , 2.0 , 3.0 , 4.0);
// 点积时候当做矢量
float result = dot(a , b);

// float4类型当做矩阵
float4 v = float4(1.0 , 2.0 , 3.0 , 4.0);
float4x4 M = float4x4(1.0 , 2.0 , 3.0 , 4.0,
					1.0 , 2.0 , 3.0 , 4.0,
					1.0 , 2.0 , 3.0 , 4.0,
					1.0 , 2.0 , 3.0 , 4.0);
float4 column_mul_result = mul(M , v);
float4 row_mul_result = mul(v , M);

```



#### Unity 中的屏幕坐标：ComputeSceenPos、VPOS、WPOS

在顶点/片元着色器中，有两种方法获得片元的屏幕坐标。

一种是在片元着色器的输入声明VPOS或者WPOS语义。VPOS是HLSL中对屏幕坐标的语义，WPOS是CG中对屏幕坐标的语义。两种在Unity Shader中是等价的。

```
fixed4 frag(float4 sp : VPOS) : SV_Target 
{
	// 屏幕坐标除以屏幕分辨率，得到视口空间坐标
    return fixed4(sp.xy / _ScreenParams.xy , 0.0 , 1.0);
}
```

VPOS、WPOS类型是float4。x、y值代表在屏幕空间的像素坐标。屏幕分辨率是400x300，那么x范围在[0.5 , 400.5] ，y的范围在[0.5 , 300.5]。Unity中z分量范围在[0 ,1]。z值为1，在摄像机的近裁剪平面处。z值0，在远裁剪面平面处。w分量，对于透视投影。w范围是$$\left[ \frac{1}{Near} , \frac{1}{Far}\right]$$。如果是正交投影，那么w的值为1。



另外一种方式是通过ComputeScreenPos函数。

```
struct vertOut {
    float4 pos : SV_POSITION;
    float4 scrPos : TEXCOORD0;
};

vertOut vert(appdata_base v){
    vertOut o;
    o.pos = mul(UNITY_MATRIX_MVP , v.vertex);
    // 
    o.scrPos = ComputeScreenPos(o.pos);
    return 0;
}

fixed4 frag(verOut i) : SV_Target {
    float2 wcoord = (i.scrPos.xy / i.scrPos.w);
    return fixed4(wcoord , 0.0 , 1.0);
}
```

公式思想：

$$
viewport_x =  \frac{clip_x}{2 \cdot clip_w} + \frac{1}{2} \\
viewport_y =  \frac{clip_y}{2 \cdot clip_w} + \frac{1}{2} \\
$$


UnityCG.cgine文件中

```
inline float4 ComputeNonStereoScreenPos(float4 pos) {
    float4 o = pos * 0.5f;
    o.xy = float2(o.x, o.y*_ProjectionParams.x) + o.w;
    o.zw = pos.zw;
    return o;
}

inline float4 ComputeScreenPos(float4 pos) {
    float4 o = ComputeNonStereoScreenPos(pos);
#if defined(UNITY_SINGLE_PASS_STEREO)
    o.xy = TransformStereoScreenSpaceTex(o.xy, pos.w);
#endif
    return o;
}
```



## Unity Shader



### 简单的顶点/片元着色器

```
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "USB/Ch.5/simple_vf_shader"
{
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            float4 vert (float4 v : POSITION) : SV_POSITION
            {
                return UnityObjectToClipPos(v);
            }

            fixed4 frag () : SV_Target
            {
                return fixed4(1.0 , 1.0, 1.0, 1.0);
            }
            ENDCG
        }
    }
}
```





> #pragma vertex **functionName**
> #pragma fragment **functionName**

上面两个函数

vert函数

```
float4 vert (float4 v : POSITION) : SV_POSITION
{
	return UnityObjectToClipPos(v);
}
```

frag函数

```
fixed4 frag () : SV_Target
{
	return fixed4(1.0 , 1.0, 1.0, 1.0);
}
```



#### 模型数据获取

顶点着色器中使用POSITION语义得到模型的顶点位置。获取多个模型数据时候。

```
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "UnityShaderBook/Ch.5/simple_vf_shader"
{
    SubShader
    {
        // Tags { "RenderType"="Opaque" }
        // LOD 100

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            struct a2v
            {
                // POSITION语义告诉Unity，用模型空间的顶点填充该变量
                float4 vertex : POSITION;
                // NORMAL语义告诉Unity，用模型空间的法线方向填充该变量
                float4 normal : NORMAL;
                // TEXCOORD语义告诉Unity，用模型空间的第一套纹理坐标变量
                float4 texcoord : TEXCOORD;
            };

            float4 vert (a2v v) : SV_POSITION
            {
                return UnityObjectToClipPos(v.vertex);
            }

            fixed4 frag () : SV_Target
            {
                return fixed4(1.0 , 1.0, 1.0, 1.0);
            }
            ENDCG
        }
    }
}

```

创建一个自定义的结构体:

```
struct StructName 
{
    Type Name : Semantic;
    Type Name : Semantic;
    ... ...
};
```



Unity中，填充到POSITION、TANGENT、NORMAL这些语义中的数据，来源于该材质的Mesh Render组件提供。在每帧DrawCall时候，MeshRender组件会负责发送数据给Unity Shader。一个模型通常包含一组三角面片，每个面片由3个顶点构成，每个顶点又包含一些数据，如顶点位置、法线、切线、纹理坐标、顶点颜色等。



#### 顶点着色器和片元着色器之间如何通信



```
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "UnityShaderBook/Ch.5/simple_vf_shader"
{
    SubShader
    {
        // Tags { "RenderType"="Opaque" }
        // LOD 100

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            struct a2v
            {
                // POSITION语义告诉Unity，用模型空间的顶点填充该变量
                float4 vertex : POSITION;
                // NORMAL语义告诉Unity，用模型空间的法线方向填充该变量
                float4 normal : NORMAL;
                // TEXCOORD语义告诉Unity，用模型空间的第一套纹理坐标变量
                float4 texcoord : TEXCOORD;
            };

            struct v2f
            {
                // SV_POSITION语义告诉Unity,pos里面包含了顶点在裁剪空间中的位置信息
                float4 pos : SV_POSITION;
                // COLOR0 语义可以用于存储颜色信息
                fixed3 color : COLOR0;
            };

            v2f vert (a2v v)
            {
                // 声明结构
                v2f o;
                
                o.pos = UnityObjectToClipPos( v.vertex);
                // v.normal包含法线方向，分量范围为[-1 , 1]
                o.color = v.normal * 0.5 + fixed3(0.5 , 0.5 , 0.5);

                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                // 将插值后的i.color显示到屏幕上
                return fixed4(i.color , 1.0);
            }
            ENDCG
        }
    }
}
```



新建了个结构体用于顶点着色器和片元着色器中进行传递数据。

```
struct v2f
{
	// SV_POSITION语义告诉Unity,pos里面包含了顶点在裁剪空间中的位置信息
	float4 pos : SV_POSITION;
	// COLOR0 语义可以用于存储颜色信息
	fixed3 color : COLOR0;
};
```

> 注意：顶点着色器是逐顶点调用的，而片元着色器是逐片元调用的。片元着色器中输入实际上是把顶点着色器的输出进行插值后得到的结果。

#### 如何使用属性



```
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "UnityShaderBook/Ch.5/simple_vf_shader"
{
    Properties
    {
        // 声明一个Color类型的属性
        _Color ("Color Tint" , Color) = (1,1,1,1)
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            // 在CG代码中，需要定义一个与属性名称类型都匹配的变量
            fixed4 _Color;

            struct a2v
            {
                // POSITION语义告诉Unity，用模型空间的顶点填充该变量
                float4 vertex : POSITION;
                // NORMAL语义告诉Unity，用模型空间的法线方向填充该变量
                float4 normal : NORMAL;
                // TEXCOORD语义告诉Unity，用模型空间的第一套纹理坐标变量
                float4 texcoord : TEXCOORD;
            };

            struct v2f
            {
                // SV_POSITION语义告诉Unity,pos里面包含了顶点在裁剪空间中的位置信息
                float4 pos : SV_POSITION;
                // COLOR0 语义可以用于存储颜色信息
                fixed3 color : COLOR0;
            };

            v2f vert (a2v v)
            {
                // 声明结构
                v2f o;
                
                o.pos = UnityObjectToClipPos( v.vertex);
                // v.normal包含法线方向，分量范围为[-1 , 1]
                o.color = v.normal * 0.5 + fixed3(0.5 , 0.5 , 0.5);

                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                fixed3 new_color = i.color;
                // 使用_Color属性来控制输出颜色
                new_color *= _Color.rgb;
                
                return fixed4(new_color , 1.0);
            }
            ENDCG
        }
    }
}
```



ShaderLab属性类型和CG变量类型匹配关系：

| ShaderLab变量类型 |     CG变量类型      |
| :---------------: | :-----------------: |
|   Color, Vector   | float4,half4,fixed4 |
|   Range , Float   |  float,half,fixed   |
|        2D         |      sample2D       |
|       Cube        |     samplerCube     |
|        3D         |      sampler3D      |

> 有时候会在CG变量前会有uniform关键字
>
> uniform fexed4 _Color;
>
> uniform关键字是CG中修饰变量和参数的一种修饰词，它仅用于提供一些该变量的初始值是如何制定和存储的相关信息。在UnityShader中可忽略。



#### Unity中内置文件和变量

```
CGPROGRAM
// ...
#include "UnityCG.cginc"
// ...
ENDCG
```

CGIncludes中主要的包含文件以及它们主要作用。

| 文件名                     | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| UnityCG.cginc              | 包含了最常使用的帮助函数、宏和结构体等                       |
| UnityShaderVariables.cginc | 在编译Unity Shader时，会被自动包含进来，包含了许多内置的全局变量，如UNITY_MATRIX_MVP等 |
| Lighting.cginc             | 包含了各种内置光照模型，如果编写的是Surface Shader的话，会自动包含进来 |
| HLSLSupport.cginc          | 在编译Unity Shader时，会被自动包含进来。声明了很多用于跨平台编译的宏和定义。 |

UnityCG.cginc中，常用的结构体。

| 名称         | 描述                   | 包含变量                                             |
| ------------ | ---------------------- | ---------------------------------------------------- |
| appdata_base | 可用于顶点着色器的输入 | 顶点变量、顶点法线、第一组纹理坐标                   |
| appdata_tan  | 可用于顶点着色器的输入 | 顶点位置、顶点切线、顶点法线、第一组纹理坐标         |
| appdata_full | 可用于顶点着色器的输入 | 顶点位置、顶点切线、顶点法线、四组（或更多）纹理坐标 |
| appdata_img  | 可用于顶点着色器的输入 | 顶点位置、第一组纹理坐标                             |
| v2f_img      | 可用于顶点着色器的输出 | 裁剪空间中的位置、纹理坐标                           |

UnityCG.cginc中，常用的帮助函数。

| 函数名                                       | 描述                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| float3 WorldSpaceViewDir(float4 v)           | 输入一个模型空间中的顶点位置，返回世界空间中从该点到摄像机观察方向 |
| float3 ObjSpaceViewDir(float4 v)             | 输入一个模型空间中的顶点位置，返回模型空间中从该点到摄像机观察方向 |
| float3 WorldSpaceLightDir(float4 v)          | **仅用于前向渲染**。输入一个模型空间中的顶点位置，返回世界空间中从该点到光源光照方向。没有被归一化。 |
| float3 ObjSpaceLightDir(float4 v)            | **仅用于前向渲染**。输入一个模型空间中的顶点位置，返回模型空间中从该点到光源的光照方向。没有被归一化。 |
| float3 UnityObjectToWorldNormal(float3 norm) | 把法线方向从模型空间转换到世界空间中                         |
| float3 UnityObjectToWorldDir(in float3 dir)  | 把方向矢量从模型空间转换到世界空间中                         |
| float3 UnityWorldToObjectDir(float3 dir)     | 把方向矢量从世界空间转换到模型空间中                         |



#### Unity CG/HLSL语义



![1610527080555](Unity Note Graphic.assets\1610527080555.png)

![1610527129946](Unity Note Graphic.assets\1610527129946.png)



#### Unity支持的语义

从应用阶段传递给顶点着色器时Unity支持的常用语义

| 语义                                | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| POSITION                            | 模型空间中的顶点位置，通常是float4类型                       |
| NORMAL                              | 顶点法线，通常是float3类型                                   |
| TANGENT                             | 顶点切线，通常是float4类型                                   |
| TEXCOORDn（如TEXCOORD0、TEXCOORD1） | 该顶点的纹理坐标，TEXCOORD0 表示第一组纹理坐标。以此类推。通常是float2或者float4类型 |
| COLOR                               | 顶点颜色，通常是float4或者fixed4类型                         |



从顶点着色器传递数据给片元着色器时Unity使用的常用语义

| 语义                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| SV_POSITION           | 裁剪空间中顶点坐标,结构体中必须包含一个用该语义修饰变量。等同于DirectX9中的POSITION。最好使用SV_POSITION |
| COLOR0                | 通常用于输出第一组顶点颜色，但不是必需的                     |
| COLOR1                | 通常用于输出第二组顶点颜色，但不是必需的                     |
| TEXTCOOR0 ~ TEXTCOOR1 | 通常用于输出纹理坐标，但不是必需的                           |



片元着色器输出时Unity支持的常用语义

| 语义      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| SV_Target | 输出值将会存储到渲染目标（render target）中，等同于DirectX9中的COLOR语义，最好使用SV_Target |



#### 渲染平台差异

Unity的优点之一是其跨平台性。Unity为我们隐藏了这些细节，但是有时候我们需要自己处理平台之间的差异。

##### 渲染纹理的坐标差异

![1610592916931](Unity Note Graphic.assets\1610592916931.png)

略 - 具体P115-P116



##### Shader语法、语义差异

略



#### Unity Shader建议

##### float、half、fixed

CG/HLSL中3种精度的数值类型

| 类型  | 精度                                                         |
| ----- | ------------------------------------------------------------ |
| float | 最高精度的浮点值，通常使用32位存储                           |
| half  | 中等精度的浮点数，通常使用16位存储，精度范围 -60,000 ~ +60,000 |
| fixed | 最低精度的浮点数，通常使用11位存储，精度范围 -2.0 ~ +2.0     |

> 精度上，不同平台会有所不同。

![1610595988937](Unity Note Graphic.assets\1610595988937.png)

##### 规范语法

使用和变量类型相匹配的参数数目来进行变量进行初始化。

##### 避免不必要的计算

![1610596747056](Unity Note Graphic.assets\1610596747056.png)

##### 慎用分支和循环语句

如果在Shader中使用了大量的流程控制语句，那么这个Shader的性能可能会成倍下降。一个解决方法是，尽量把

往流水线的上方或者CPU上放。例如吧放在片元着色器中的计算放到顶点着色器中，或者直接在CPU中进行预计算，在把结果传递给Shader。

+ 分支判断语句中使用的条件变量最好是常数。（在Shader运行过程中不会发生变化）
+ 每个分支中包含的操作指令尽量少
+ 分支嵌套数尽量少

##### 不要除以零

> Shader编写时候会忽略这个问题

```
fixed4 frag(v2f i) : SV_Target
{
    return fixed4(0.0 / 0.0 , 0.0 / 0.0 , 0.0 / 0.0 , 1.0);
}
```



### Unity中的基础光照

通常来说，我们要模拟真实的光照环境来生成一张图像，需要考虑3种物理现象：

+ 首先，光线从光源中发射出来
+ 光线和场景中的一些物体相交：一些光线被物体吸收，一些光线被散射到其他方向
+ 最后，摄像机吸收了一些光，产生了一张图像。

#### 光源

![1610606219150](Unity Note Graphic.assets\1610606219150.png)

#### 吸收和散射

> 光线由光源发射出来后，会与物体相交。相交结果有两种： 散射和吸收

散射只改变光线方向，但不改变光线的密度和颜色。

吸收只改变光线的密度和颜色，但是不改变光线方向。



光线在表面经过散射后，有两种方向：

+ 散射到物体内部，称折射|透射
+ 散射到物体外部，称为反射

> 对于不透明的东西，折射还会和那边的颗粒进行交互。

![1610606685682](Unity Note Graphic.assets\1610606685682.png)

为了区分两种不同的散射方向，光照模型中使用了不同的部分来计算。

> 高光反射 ：表示物体表面是如何反射光线
>
> 漫反射：表示有多少光线会被折射、吸收和散射出表面。

 根据入射光线的数量和方向，我们可以计算出射出光线的数量和方向。我们通常使用**出射度**来描述它。辐射度和出射度满足线性关系。而它们之间的比值就是材质的漫反射和高光反射属性。

#### 着色

着色指的是，根据材质属性、光照信息，使用一个等式去计算沿某个方向的观察方向的出射度的过程。这个等式称为光照模型。不同光照模型有不同的目的。



### 标准光照模型

把进入摄像机内的光线分为4部分。

> 自发光 ：用于描述当给定一个方向时，一个表面本身会向该方向发射多少辐射量。需要注意的是，如果没有全局光照技术，这些自发光的表面并不会真的照亮周围的物体，而是它本身看起来更亮了。
>
> 高光反射 ：用于描述当光线总光源照射到模型表面时，该表面会在完全镜面反射方向散射多少辐射量。
>
> 漫反射 ：用于描述当光线从光源照射到模型表面时，该表面会向每个方向散射多少辐射量。
>
> 环境光 ：用于描述其他所有的间接光照。

环境光：

光照模型重点在于直接光照，但是物体也是受间接光照影响。间接光照通常是指多个物体之间反射，最后进入摄像机。



漫反射：

漫反射光照是用于对那些被物体表面随机散射到各个方向的辐射度进行建模的。在漫反射中，视角的位置是不重要的，因为反射是完全随机的。但是，入射光线很重要。

漫反射符合兰伯特定律（Lambert's law）：反射光线的强度与表面法线和光源方向之间夹角的余弦值成正比。公式如下：
$$
C_{diffuse} = (C_{light} \cdot m_{diffuse})max(0 , n \cdot I)
$$
其中，n是表面法线，I是指向光源的单位矢量，$$m_{diffuse}$$是材质的漫反射颜色。$$c_{light}​$$是光源颜色。

> 为了防止法线和光源方向点乘的结果为负值，为此取最大值截取到0。

高光反射

需要知道表面法线、视角方向、光源方向、反射方向等。
$$
r = 2 (\widehat{n} \cdot I)\widehat{n} - I
$$
利用Phong模型来计算高光。
$$
C_{specular} = (C_{light} \cdot m_{specular})max(0,\widehat{v} \cdot r)^{m_{gloss}};
$$
$$m_{gloss}$$为光泽度。用于控制高光区域的“亮点”有多宽。$$m_{specular}$$是材质高光反射的颜色，用于控制高光反射强度和颜色。$$C_{light}$$则是光源颜色和强度。$$\widehat{v} \cdot \widehat{r}$$的结果为负数。

![1610614942318](Unity Note Graphic.assets\1610614942318.png)



#### 逐像素还是逐顶点

+ 在片元着色器中计算 ------ 逐像素光照
+ 在顶点着色器中计算 ------ 逐顶点光照

![1610615710869](Unity Note Graphic.assets\1610615710869.png)



### Unity的环境光和自发光

![1610616287421](Unity Note Graphic.assets\1610616287421.png)



### UnityShader 中实现漫反射光照模型

漫反射计算公式：
$$
C_{diffuse} = (C_{light} \cdot m_{diffuse})max(0 , n \cdot I)
$$


```
// Upgrade NOTE: replaced '_World2Object' with 'unity_WorldToObject'

Shader "UnityShaderBook/Ch.6/ch_6_diffuse_vertex_shader"
{
    Properties
    {
        _Diffuse("Diffuse" , Color) = (1.0 , 1.0, 1.0,1.0)
    }
    SubShader
    {
        Pass
        {
            Tags { "LightMode" = "ForwardBase"}

            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            #include "Lighting.cginc"

            fixed4 _Diffuse;

            struct appdata
            {
                float4 vertex : POSITION;
                float3 normal : NORMAL;
            };

            struct v2f
            {
                float4 vertex : SV_POSITION;
                float3 color :COLOR;
            };


            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);

                // 获取环境光部分
                fixed3 ambient = UNITY_LIGHTMODEL_AMBIENT;
                // 将从模型空间转换世界空间
                fixed3 worldNormal = normalize(mul(v.normal  , (float3x3)unity_WorldToObject));
                // 光源方向 （世界空间）
                fixed3 worldLight = normalize(_WorldSpaceLightPos0.xyz);
                // 漫反射计算
                fixed3 diffuse = _LightColor0.rgb * _Diffuse.rgb * saturate(dot(worldNormal , worldLight));
                o.color = ambient + diffuse;

                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                return fixed4(i.color , 1.0);
            }
            ENDCG
        }
    }
    Fallback "Diffuse"
}
```

