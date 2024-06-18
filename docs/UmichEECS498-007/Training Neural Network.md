## Activation Functions
![](../img/Pasted%20image%2020240617215844.png)
### Sigmoid
$$
\sigma(x) = \frac{1}{1+e^{-x}}
$$
3 Problems:
1. **Saturated neurons “kill” the gradients. 在饱和时，$sigma(x)$对$x$的梯度很小**（**Worst problem in practice**）
2. Sigmoid outputs are not zero-centered(对上游梯度，同号，zigzag)
3. exp() is a bit compute expensive
### Tanh
- Squashes numbers to range [-1,1] 
- zero centered (nice) 
- still kills gradients when saturated 

### ReLU(Rectified Linear Unit)
- Does not saturate (in +region)
- Very computationally efficient
- Converges much faster than sigmoid/tanh in practice (e.g. 6x)
But
- Not zero-centered output
- An annoyance:what is the gradient when x < 0?
### LeakyReLU
- Does not saturate (in +region)
- Very computationally efficient
- Converges much faster than sigmoid/tanh in practice (e.g. 6x)
- **will not die**
#### Parametric ReLU (PReLU)
$f(x) = \max (\alpha x, x)$, $\alpha$ is learned via backprop

### ELU(Exponential Linear Unit)
![](../img/Pasted%20image%2020240617222050.png)

$$ 
f(x) = \begin{cases}
x & \text{if } x > 0 \\ \alpha(e^x - 1) & \text{if } x \le 0
\end{cases}
$$
(Default alpha=1)
- All benefits of ReLU 
- Closer to zero mean outputs 
- Negative saturation regime compared with Leaky ReLU adds some robustness to noise
But
- Computation requires exp()

### SELU(Scaled Exponential Linear Unit)
$$ 
f(x) = \begin{cases}
	\lambda x & \text{if } x > 0 \\ \lambda\alpha(e^x - 1) & \text{if } x \le 0
\end{cases}
$$
$$
\begin{array}
	a\alpha = 1.6732632423543772848170429916717 \\
	\lambda = 1.0507009873554804934193349852946
\end{array}
$$
- Scaled version of ELU that works better for deep networks
- “Self-Normalizing” property; can train deep SELU networks without BatchNorm

### GELU(Gaussian Error Linear Unit)
![](../img/Pasted%20image%2020240617222101.png)
- Idea: Multiply input by 0 or 1 at random; large values more likely to be multiplied by 1, small values more likely to be multiplied by 0 (data-dependent dropout)
- Take expectation over randomness
- Very common in Transformers (BERT, GPT, ViT)
- ????
### Summary
- Don’t think too hard. Just use ReLU 
- Try out Leaky ReLU / ELU / SELU / GELU if you need to squeeze that last 0.1% 
- **Don’t use sigmoid or tanh**

## Data Preprocessing
作数据标准化有利于模型训练
![](../img/Pasted%20image%2020240617222648.png)
![](../img/Pasted%20image%2020240617222639.png)
![](../img/Pasted%20image%2020240617222703.png)

## Weight Initialization
如果权重均初始化为0，那么输出也为0，所有神经元的梯度一致
需要注意的是，权重初始化只在意权重的初值，即只需要保证BP算法能够在训练的初期work，经过多轮BP后，权重会发生改变，不再具有初始化时的性质。
### small random numbers
```Python
W = 0.01 * np.random.randn(Din, Dout)
```
由于激活函数不断会squash每层output的分布，在深度较深时，![](../img/Pasted%20image%2020240618102607.png)
如果增加std，那么会进入激活函数的饱和区域，局部梯度为0
![](../img/Pasted%20image%2020240618102823.png)

### Xavier Initialization
$$
	std = \frac{1}{\sqrt{Din}}，\text{for conv layers}, Din = kernel\_size^2 * input\_channel
$$
Xavier initialization的思想是，使得输出的$y=Wx$的方差与$x$的方差一致
![](../img/Pasted%20image%2020240618103056.png)
但Xavier的思想只在zero-centered的激活函数上成立，如果是non-zero-centered的激活函数，如ReLU，那么经过激活函数的$y$无法保持方差

### Kaiming /MSRA Initialization
$$
	std = \frac{2}{\sqrt{Din}}，\text{for conv layers}, Din = kernel\_size^2 * input\_channel
$$
![](../img/Pasted%20image%2020240618104916.png)

## Regularization
你的模型训练集精度比验证集精度高50%.jpg
### 正则项
略
### Dropout
Can be interpreted in two ways:
- Forces the network to have a redundant representation; Prevents co-adaptation of features
- Dropout is training a large ensemble of models (that share parameters).
- ![](../img/Pasted%20image%2020240618105347.png)
- 注意test时输出×p
或者 Inverted Dropout:
![](../img/Pasted%20image%2020240618105443.png)
### Common Pattern
- **Trainning**:Add some kind of randomness
- Testing: Average out randomness


**For ResNet and later, often L2 and Batch Normalization are the only regularizers!**

### Data Augmentation
**Data augmentation encodes invariances in your model**
1. Horizontal Flips
2. Random crops and scales
3. Color Jitter
4. RandAugment(just random)

![](../img/Pasted%20image%2020240618110058.png)

## Learning Rate Schedule(Decay)
### Step
![](../img/Pasted%20image%2020240618110805.png)
### Cosine
![](../img/Pasted%20image%2020240618110820.png)
### Linear
![](../img/Pasted%20image%2020240618110836.png)
### Inverted Sqrt
![](../img/Pasted%20image%2020240618110853.png)
### Constant
![](../img/Pasted%20image%2020240618110920.png)


## Choosing Hyperparameters
1. Check initial loss
2. Overfit a small sample
3. FInd LR that makes loss to go down
4. Coarse grid, train for ~1-5 epochs
5. Refine grid, train longer
6. Look at Learning Curves!
7. GOTO step 5

## Transfer Learning
![](../img/Pasted%20image%2020240618111859.png)