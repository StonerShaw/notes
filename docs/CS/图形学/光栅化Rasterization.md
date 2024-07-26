在MVP(Model, View, Projection transformation)后，下一个问题是如何把$[-1,1]^3$的正方体放在屏幕上。Raster实际上就是德语中屏幕的意思。在本课程中，屏幕被视为pixel的数组，pixel是一个具有均匀色彩的方块。

### Canoinical Cube to Screen
缩放平移
![](../../img/Pasted%20image%2020240726144822.png)
![](../../img/Pasted%20image%2020240726144831.png)

### Rasterizing a triangle
通过采样(sample) 光栅化三角形
![](../../img/Pasted%20image%2020240726145615.png)
![](../../img/Pasted%20image%2020240726145627.png)

由于三角形的bounding box很好寻找，在实践中不需要判断屏幕上的所有点，只需要判断bounding box中的点即可。
![](../../img/Pasted%20image%2020240726145744.png)

### Aliasing(Jaggies)
锯齿问题
![](../../img/Pasted%20image%2020240726150137.png)

### Antialiasing
减少走样错误的两种思路：
1. 增加采样率
	- Essentially increasing the distance between replicas in the Fourier domain
	- Higher resolution displays, sensors, framebuffers…
	- But: costly & may need very high resolution
2. 抗走样(Antialiasing)
	- Making Fourier contents “narrower” before repeating
		- 滤去高频。**高频代表边缘**
		- 方法：convolving=filtering=averaging


#### Blurring(Pre-FIltering) Before Sampling
![](../../img/Pasted%20image%2020240726150413.png)

#### Antialiasing By Supersampling (MSAA)
Approximate the effect of the 1-pixel box filter by sampling multiple locations within a pixel and averaging their values
![](../../img/Pasted%20image%2020240726151018.png)
![](../../img/Pasted%20image%2020240726151033.png)

MSAA虽然有效，但代价过高
![](../../img/Pasted%20image%2020240726152117.png)

