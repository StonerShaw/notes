## Ways to Represent Geometry
### Implicit
Based on classifying points
E.g. sphere: all points in 3D, where x^2+y^2+z^2 = 1
**More generally, f(x,y,z) = 0**
### Explicit
All points are **given directly** or via **parameter mapping**
![](../../img/Pasted%20image%2020240802011638.png)
![](../../img/Pasted%20image%2020240802011655.png)
Some tasks are hard with explicit representations: To know whether points are inside.

### More Implicit Representations in Computer Graphics
#### Algebraic Surfaces (Implicit)
![](../../img/Pasted%20image%2020240802013733.png)
#### Constructive Solid Geometry (Implicit)
![](../../img/Pasted%20image%2020240802013746.png)
#### Distance Functions (Implicit)
Instead of Booleans, gradually blend surfaces together using **Distance functions**: **giving minimum distance (could be signed distance) from anywhere to object**
![](../../img/Pasted%20image%2020240802013920.png)
#### Level Set Methods (Also implicit)
等高线
![](../../img/Pasted%20image%2020240802014000.png)
- used in Medical Data (CT, MRI, etc.)
-  Physical Simulation
#### Fractals (Implicit)(分型)
![](../../img/Pasted%20image%2020240802014100.png)
### Implicit Representations - Pros & Cons
Pros:
- compact description (e.g., a function)
- certain queries easy (inside object, distance to surface)
- good for ray-to-surface intersection (more later) 
- for simple shapes, exact description / no sampling error 
- easy to handle changes in topology (e.g., fluid) 
Cons:
- difficult to model complex shapes
### Explicit Representations in Computer Graphics
#### Point Cloud (Explicit)
- Easiest representation: list of points (x,y,z)
- Easily represent any kind of geometry
- Useful for LARGE datasets (>>1 point/pixel)
- Often converted into polygon mesh
- Difficult to draw in undersampled regions
#### Polygon Mesh (Explicit)
- Store vertices & polygons (often triangles or quads)
- Easier to do processing / simulation, adaptive sampling
- More complicated data structures
- Perhaps most common representation in graphics
![](../../img/Pasted%20image%2020240802014555.png)
#### The Wavefront Object File (.obj) Format
- Commonly used in Graphics research
- Just a text file that specifies vertices, normals, texture coordinates and their **connectivities**
![](../../img/Pasted%20image%2020240802014627.png)

## Curves
### Bézier Curves
#### Evaluating Bézier Curves (de Casteljau Algorithm)
1. Consider three points (quadratic Bezier)
2. Insert a point using linear interpolation![](../../img/Pasted%20image%2020240812173540.png)
3. Insert on both edges
4. Repeat recursively
5. Run the same algorithm for every t in \[0,1\]![](../../img/Pasted%20image%2020240812173836.png)

#### Evaluating Bézier Curves Algebraic Formula
![](../../img/Pasted%20image%2020240812174449.png)
#### Properties of Bézier Curves
1. Interpolates endpoints 插值端点:$b(0)=b_0, b(1) = b_3$
2. Tangent to end segments 端点处切线：$b'(0)=3(b_1-b_0), b'(1) = 3(b_3-b_2)$
3. Affine transformation property 可以通过仿射变换控制点来变换曲线
4. Convex hull property 曲线总是在控制点的凸包内
	1. What's a Convex Hull? 插钉子，放皮筋

### Piecewise Bézier Curves
Higher-Order Bézier Curves? 有点电笔：控制点与曲线的关系更加复杂，难以控制
那么如何画出复杂曲线？
**Piecewise cubic Bézier**
![](../../img/Pasted%20image%2020240812175921.png)
Piecewise cubic Bézier要求曲线C1 continuity，也就是在连接处的一阶导相等

### Other types of splines
#### Spline样条
a continuous curve constructed so as to **pass through a given set of points** and have a certain number of **continuous derivatives**.
#### B-splines
- Short for basis splines
- Require more information than Bezier curves
- Satisfy all important properties that Bézier curves have (i.e. superset)


## Surfaces
### Bézier Surfaces
![](../../img/Pasted%20image%2020240812180335.png)
多插几次

## Mesh Operations: Geometry Processing
### Mesh subdivision(upsampling)
#### Loop Subdivision
Common subdivision rule for triangle meshes
First, create more triangles (vertices)
Second, tune their positions

1. Split each triangle into four![](../../img/Pasted%20image%2020240815172342.png)
2. Assign new vertex positions according to weights
	1. For new vertices:![](../../img/Pasted%20image%2020240815172402.png)
	2. For old vertices (e.g. degree 6 vertices here):![](../../img/Pasted%20image%2020240815172438.png)
#### Catmull-Clark Subdivision (General Mesh)
适用于任意mesh
- Non-quad face: 不是四边形的面
- Extraordinary vertex：度不是4的点
Each subdivision step: 
1. Add vertex in each face（面的“中点”）
2. Add midpoint on each edge （线的中点）
3. Connect all new vertices（连接）
在一次subdivision后，所有Non-quad face消失
### Mesh Simplification (downsampling)
### Mesh Regularization (same triangles)
TBA