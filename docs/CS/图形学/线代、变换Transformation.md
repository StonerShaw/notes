## 线代
### 叉乘Cross Product
**叉乘(外积)只对三维向量有定义**
$$
\begin{array}
\\
a \times b = - b \times a \\
\lVert{a \times b} \rVert = \lVert a\rVert \lVert b\rVert \sin{\phi}
\end{array}
$$
对于右手系，左侧成立：
![](../../img/Pasted%20image%2020240720201759.png)

叉乘结果的分量计算：
![](../../img/Pasted%20image%2020240720202334.png)
矩阵：
![](../../img/Pasted%20image%2020240720202355.png)

可以使用叉乘判断向量的相对关系：
对xy平面上的$\vec{a}$和$\vec{b}$，如果$\vec{a} \times \vec{b}$为**正**，即正z轴方向，说明$\vec{a}$在$\vec{b}$的**右侧**

可以利用这个性质进一步判断点与封闭：
1. 按顺序给定多边形的多个顶点以及需要判断的点$Q$。
2. 对每条边$(P_i, P_{i+1})$，记录$P_iQ \times P_iP_{i+1}$的符号
3. 如果所有叉积的符号相同（即要么全为正，要么全为负），则点 $Q$在多边形内部。

## Transformation
### 2D Transformation
#### Scale
![](../../img/Pasted%20image%2020240720205609.png)

#### Reflection
Horizontal reflection:
![](../../img/Pasted%20image%2020240720205654.png)

#### Shear
![](../../img/Pasted%20image%2020240720205719.png)

#### Rotate
![](../../img/Pasted%20image%2020240720205743.png)

上述几种变换均可以在**二维坐标**下通过**矩阵乘法**实现


### 齐次坐标Homogenous Coordinates
- Why?
- 平移无法使用矩阵乘法表示
- ![](../../img/Pasted%20image%2020240720210346.png)

Solution:
![](../../img/Pasted%20image%2020240720210407.png)
![](../../img/Pasted%20image%2020240720210940.png)
![](../../img/Pasted%20image%2020240720210949.png)


注意到 变换的矩阵都**可逆**。设一个变换的矩阵为$M$,则其逆变换的矩阵为$M^{-1}$

#### 变换的组合与分解
左乘矩阵
![](../../img/Pasted%20image%2020240720213817.png)

如何绕一点C旋转？
![](../../img/Pasted%20image%2020240720213844.png)


### 3D变换
![](../../img/Pasted%20image%2020240720213958.png)
先**线性变换**，再**平移**
#### 3D中的旋转
![](../../img/Pasted%20image%2020240720221147.png)
注意到绕$y$轴旋转的矩阵形式与绕其他两个轴旋转的矩阵形式有差别，原因在于右手系中，$y = - x \times z$
![](../../img/Pasted%20image%2020240720221608.png)

### Rodrigues' Rotation Formula（罗德里格斯旋转公式）
给定一条轴$n$（使用单位方向向量表示），绕轴$n$旋转$\alpha$的变换矩阵
![](../../img/Pasted%20image%2020240720221703.png)

### 视图变换View / Camera Transformation
"怎么看"
定义相机：
1. 位置$\vec{e}$
2. 朝向$\hat{g}$
3. 向上方向$\hat{t}$

**注意到，如果相机与所有的对象一同移动，那么“照片”保持不变**。因此总是先将相机变换到一个特定的位置：
1. 原点
2. 看向$-Z$
3. 使$Y$朝上
然后对所有对象做相同的变换。这个变换的矩阵记作$M_{view}$

![](../../img/Pasted%20image%2020240720225816.png)

### 投影变换Projection Transformation
如何把三维的物体投影到二维？
#### 正交投影Orthographic Projection
成像面无穷远
![](../../img/Pasted%20image%2020240720230350.png)

#### 透视投影Perspective Projection
平行线交于一点
如果将正交投影视作把一个长方体映射到在原点的边长为2的标准立方体，那么透视投影可以视作对一个椎体做映射。那么我们可以先把椎体映射为长方体，再对这个长方体做正交投影
![](../../img/Pasted%20image%2020240720230846.png)

如何squish？相似三角形
![](../../img/Pasted%20image%2020240720231202.png)
![](../../img/Pasted%20image%2020240720231711.png)


近平面上的点经过映射位置不变->$An + B = n^2$
原平面上的点经过映射$z=f$不变-> $Af + B = f^2$
![](../../img/Pasted%20image%2020240720231858.png)
![](../../img/Pasted%20image%2020240720231939.png)

##### 近平面
透视投影相比于正交投影引入了一个物品/对象之外的东西：近平面。因此我们需要额外地定义近平面的参数。可以直接定义l,r,b,t，也定义**宽高比(aspect ratio)** 以及 **视角(field-of-view, fovY)**
![](../../img/Pasted%20image%2020240726143924.png)

如何将fovY和aspect转换为l,r,b,t？
![](../../img/Pasted%20image%2020240726144314.png)