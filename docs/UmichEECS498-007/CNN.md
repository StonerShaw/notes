## Components
### Conv Layer
#### 输出shape计算
![](../img/Pasted%20image%2020240617210207.png)
![](../img/Pasted%20image%2020240617210352.png)
#### 1 × 1卷积
Stacking 1×1 conv layers gives MLP operating on each input **position**
![](../img/Pasted%20image%2020240617210311.png)

### Pooling Layer
Downsample
>降采样是指在时间或者空间上减少采样点的密度，以减少数据的数量。它通常发生在信号处理、音频处理、图像处理和机器学习中的数据预处理阶段。

#### Max Pooling
**Introduces invariance to small spatial shifts**
![](../img/Pasted%20image%2020240617210955.png)

### Batch Normalization
>Helps reduce “internal covariate shift”, improves optimization

![](../img/Pasted%20image%2020240617211744.png)
#### BatchNorm 和 LayerNorm的差别
![](../img/Pasted%20image%2020240617212207.png)

## CNN Architectures
### LeNet-5
![](../img/Pasted%20image%2020240617211205.png)

### AlexNet
![](../img/Pasted%20image%2020240617212939.png)
- Most of the memory usage is in the early convolution layers
- Nearly all parameters are in the fully-connected layers
- Most floating-point ops occur in the convolution layers

### VGG: Deeper Networks, Regular Design
![](../img/Pasted%20image%2020240617213304.png)
- 使用两个3x3的卷积层代替一个5x5的卷积层，**Two 3x3 conv has same receptive field as a single 5x5 conv, but has fewer parameters and takes less computation!**
- 通道加倍后，计算量不变。Conv layers at each spatial resolution take the same amount of computation!
### ResNet
学习F(x)-x
#### 残差块(Residual Block)
![](../img/Pasted%20image%2020240617214641.png)
激进降采样：
![](../img/Pasted%20image%2020240617214448.png)
使用AvgPooling代替FC：
![](../img/Pasted%20image%2020240617214543.png)

![](../img/Pasted%20image%2020240617214740.png)
**ResNet-50 is the same as ResNet-34, but replaces Basic blocks with Bottleneck Blocks.**
#### 瓶颈块(Bottleneck Block)
![](../img/Pasted%20image%2020240617214853.png)


### SENet
### MobileNet