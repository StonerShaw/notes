## **此章笔记待重施工**
## **Illumination & Shading**
Definition of Shading in this course: 
**The process of applying a material to an object.**

### Blinn-Phong Reflectance Model
Three kind of Illumination:
1. Specular highlights 高光
2. Diffuse reflection 漫反射
3. Ambient lighting 环境光照

Shading is local : Compute light reflected toward camera at a specific **shading point**

#### Diffuse Reflection
- Light is scattered uniformly in all directions : Surface color is the same for all viewing directions

##### Lambert’s cosine law
Decide how much light is received
![](../../img/Pasted%20image%2020240727140810.png)

##### Light Falloff
考虑单位球面上的能量$I$与距离为r时球面上的能量，可以得出距离为r时，单位面积上的能量是$I / r ^2$

##### Lambertian (Diffuse) Shading
漫反射部分的Shading与观察角度无关
![](../../img/Pasted%20image%2020240727141012.png)
$k_d$为漫反射系数，决定了多少光照被反射/吸收

#### Specular
![](../../img/Pasted%20image%2020240727141316.png)
使用半程向量与法向量的夹角决定衰减

- 为什么使用p power？
- 增加衰减：![](../../img/Pasted%20image%2020240727141555.png)

#### Ambient
Shading that does not depend on anything
- Add constant color to account for disregarded illumination and fill in black shadows 
- This is approximate / fake!
![](../../img/Pasted%20image%2020240727141701.png)

#### Blinn-Phong Reflection Model
![](../../img/Pasted%20image%2020240727141725.png)

### Shading frequencies
- Shade **each triangle** (flat shading)
	- Triangle face is flat — one normal vector
	- Not good for smooth surfaces
- Shade **each vertex** (Gouraud shading)
	- **Interpolate** colors from vertices across triangle
	- Each vertex has a normal vector (how? **Interpolate**)
- Shade **each pixel** (Phong shading)
	- Interpolate normal vectors across each triangle
	- Compute full shading model at each pixel 
	- Not the Blinn-Phong Reflectance Model\
	- **Phong Shading的pixel是指屏幕上的pixel，但对texel做是对3D model做的**
![](../../img/Pasted%20image%2020240727142735.png)

#### Define Per-Vertex Normal Vectors(定义顶点法向量)
1. Best to get vertex normals from the underlying geometry (e.g. sphere)
2. Infer vertex normals from triangle faces
	- Simple scheme: average surrounding face normals
	- $N_v = \frac{\sum_i N_i}{\lVert{\sum_i N _i}\rVert}$![](../../img/Pasted%20image%2020240727152152.png)

#### Defining Per-Pixel Normal Vectors(定义像素法向量)
Barycentric Interpolation

## Graphics (Real-time Rendering) Pipeline
![](../../img/Pasted%20image%2020240727152545.png)
1. MVP Transforms
2. Rasterization
3. Z-Buffer
4. Shading
5. Texture Mapping

## Texture Mapping
Blinn-Phong Reflectance Model决定了一个Shding point“发多少光”，纹理决定“发什么光”
Texture Mapping 将3D表面的每一个点都映射到纹理空间中。纹理空间一般是一个$u\times v = 1\times 1$的方形区域。

#### Barycentric Interpolation
为什么需要插值？
- Specify values at vertices
- Obtain smoothly varying values across triangles
需要做哪些插值？
- Texture coordinates, colors, normal vectors
![](../../img/Pasted%20image%2020240727160412.png)
几何角度：
![](../../img/Pasted%20image%2020240727161143.png)
已知坐标系坐标，如何求重心坐标：
![](../../img/Pasted%20image%2020240727161238.png)

使用重心坐标插值：
![](../../img/Pasted%20image%2020240727161512.png)

### Applying Textures
![](../../img/Pasted%20image%2020240727162036.png)

### Texture Magnification
What if the texture is too small?
纹理太小，分辨率太低，导致走样
#### Bilinear Interpolation
![](../../img/Pasted%20image%2020240727162617.png)
"$lerp$" is referred to as linear interpolation

What if the texture is too large?
透视投影下，远处一个像素覆盖的表面积更大，相对应的纹理面积也就更大
![](../../img/Pasted%20image%2020240727163155.png)

可以通过超采样解决，但开销太大
#### Mipmap
区域查询
![](../../img/Pasted%20image%2020240727163357.png)
![](../../img/Pasted%20image%2020240727163406.png)
Mipmap的overhead只有原纹理的1/3

##### How to compute 
计算Level D:
![](../../img/Pasted%20image%2020240727163530.png)
![](../../img/Pasted%20image%2020240727163618.png)
这里使用**屏幕坐标**求导

##### Trilinear Interpolation
如果D不是整数，那么就在level $\lfloor D \rfloor$ 和level $\lfloor D \rfloor + 1$ 上做三线性插值

##### Mipmap Limitations
Overblur
![](../../img/Pasted%20image%2020240727163929.png)
原因在于，一个像素覆盖的纹理区域不一定就是正方形
![](../../img/Pasted%20image%2020240727164016.png)

#### Anisotropic Filtering
![](../../img/Pasted%20image%2020240727164034.png)


### Applications of Textures
- In modern GPUs, **texture = memory + range query (filtering)**
    - General method to bring data to fragment calculations
- Many applications
    - Environment lighting 环境光照（环境贴图）：假设环境光来自无限远处，只记录
    - Store microgeometry
    - Procedural textures
    - Solid modeling
    - Volume rendering 体积渲染

- Environment Map:假设环境光来自无穷远处，只记录方向信息
	- ![](../../img/Pasted%20image%2020240802004140.png)
- Environmental Lighting:将环境光记录在纹理贴图上
- Spherical Environment Map：将环境光记录在球面上
	- Problem: top和bottom的信息不均匀
	- Cube Map:![](../../img/Pasted%20image%2020240802004332.png)![](../../img/Pasted%20image%2020240802004349.png)
#### Bump Mapping
Adding surface detail without adding more triangles
- Perturb surface normal per pixel (for shading computations only)
- “Height shift” per texel defined by a texture
- modify normal vector(How?)

![](../../img/Pasted%20image%2020240802004929.png)
2D：以球面为例：原始法向量为$(0,1)$:
![](../../img/Pasted%20image%2020240802004940.png)
3D：![](../../img/Pasted%20image%2020240802005124.png)

#### Displacement mapping——a more advanced approach
- Uses the same texture as in bumping mapping
- Actually moves the **vertices**

#### 3D Procedural Noise + Solid Modeling
#### Provide Precomputed Shading
在纹理中预计算shading
#### 3D Textures and Volume Rendering