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
