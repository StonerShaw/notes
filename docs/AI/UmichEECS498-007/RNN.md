## Vanilla RNN
$$
h_t = tanh(W_{hh}h_{t-1} + W_{xh}x_t + b_h)
$$
![](../../img/Pasted%20image%2020240618140441.png)
If the singular value of $W_{hh}^T$ > 1: Exploding gradients
- Gradient Clipping:
```Python
grad_norm = np.sum(grad * grad)
if grad_norm > threshold:
	grad *= (threshold / grad_norm)
```
If the singular value of $W_{hh}^T$ < 1: Vanishing gradients
- Change RNN architecture!

## LSTM
![](../../img/Pasted%20image%2020240618140813.png)
![](../../img/Pasted%20image%2020240618141128.png)
>https://stats.stackexchange.com/questions/185639/how-does-lstm-prevent-the-vanishing-gradient-problem

## GRU
![](../../img/Pasted%20image%2020240618142250.png)

![](../../img/Pasted%20image%2020240618142351.png)