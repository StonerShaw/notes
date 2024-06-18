![](../img/Pasted%20image%2020240618162349.png)
![](../img/Pasted%20image%2020240618162502.png)

## Autoergressive model
Explicit Density(Compute p(x))
Our goal is to write down an explicit function for $p(x) = f(x, W)$
Assuming x consists of multiple subparts:
$$
\begin{aligned}
x &= (x_1, x_2, \cdots, x_T) \\
p(x) &= p(x_1, x_2, \cdots, x_T) \\
&= p(x_1)p(x_2|x_1)p(x_3|x_1,x_2)\dots \\
&= \prod_{t=1}^T p(x_t|x_1, \dots, x_{t-1})
\end{aligned}
$$
Probability of the next part given all the previous subparts
### Pixel RNN
![](../img/Pasted%20image%2020240618163525.png)
Too **Slow**

### Pixel CNN
![](../img/Pasted%20image%2020240618163620.png)


## Variantional Autoencoders
VAE define an intractable density that **we cannot explicitlyl compute or optimize**
But we are still able to directly optimize **a lower bound** on the density

### (Regular, non-variantional) Autoencoders
- Problem: How can we learn this feature transform from raw data?
- Idea: Use the features to reconstruct the input data with a decoder ,“Autoencoding” = encoding itself
- ![](../img/Pasted%20image%2020240618164103.png)
- After training, **throw away decoder** and use encoder for a downstream task

- Autoencoders learn latent features for data without any labels! 
- Can use features to initialize a supervised model 
- **BUT, Not probabilistic: No way to sample new data from learned model**

### Variantional Autoencoders
![](../img/Pasted%20image%2020240618165852.png)![](../img/Pasted%20image%2020240618165901.png)
Training:
![](../img/Pasted%20image%2020240618170808.png)
Generate New Data:
![](../img/Pasted%20image%2020240618170847.png)
Edit Images:
![](../img/Pasted%20image%2020240618170935.png)
Pros:
-  Principled approach to generative models 
- Allows inference of q(z|x), can be useful feature representation for other tasks 
Cons:
- Maximizes lower bound of likelihood: okay, but not as good evaluation as PixelRNN/PixelCNN 
- Samples blurrier and lower quality compared to state-of-the-art (GANs)

## Generative Adversarial Networks
Generative Adversarial Networks give up on modeling p(x), but allow us to draw samples from p(x)

![](../img/Pasted%20image%2020240618172234.png)
![](../img/Pasted%20image%2020240618172339.png)
![](../img/Pasted%20image%2020240618172423.png)
![](../img/Pasted%20image%2020240618172554.png)
最优解：
![](../img/Pasted%20image%2020240618173424.png)
证明看不懂，摆了

![](../img/Pasted%20image%2020240618174209.png)