## Bare Tensor
>When we create a PyTorch Tensor with `requires_grad=True`, then operations involving that Tensor will not just compute values; they will also build up a computational graph in the background, allowing us to easily backpropagate through the graph to compute gradients of some Tensors with respect to a downstream loss. Concretely, if `x` is a Tensor with `x.requires_grad == True` then after backpropagation `x.grad` will be another Tensor holding the gradient of `x` with respect to the scalar loss at the end.

### Init
- `nn.init.kaiming_normal_(torch.empty(shape))`
- `nn.init.zeros_(torch.empty(shape))`

### Functional
**小写**，需要传入参数(w, b)
- `import torch.nn.functional as F`
- `F.linear()`
	- `F.linear(x, w, b)` is equivalent to `x.mm(w.T) + b`
- `F.relu()`

### Loss
in `torch.nn.functional`
- `F.cross_entropy(scores, y)`
	- scores是**未通过softmax归一化**的分数

### Optim
`import torch.optim as optim`
- `optim.SGD()`
> CLASS torch.optim.SGD(_params_, _lr=0.001_, _momentum=0_, _dampening=0_, _weight_decay=0_, _nesterov=False_, _*_, _maximize=False_, _foreach=None_, _differentiable=False_, _fused=None_)

>torch.optim.Adam(_params_, _lr=0.001_, _betas=(0.9, 0.999)_, _eps=1e-08_, _weight_decay=0_, _amsgrad=False_, _*_, _foreach=None_, _maximize=False_, _capturable=False_, _differentiable=False_, _fused=None_)


## Module API
![](../../img/Pasted%20image%2020240525173152.png)
- 注意在forward中用到的所有layers均应该在__init__中被定义
```Python
class ThreeLayerConvNet(nn.Module):
  def __init__(self, in_channel, channel_1, channel_2, num_classes):
    super().__init__()
    H = 32
    W = 32
    self.conv1 = nn.Conv2d(in_channel, channel_1, kernel_size=5, padding=2)
    self.relu1 = nn.ReLU()
    self.conv2 = nn.Conv2d(channel_1, channel_2, kernel_size=5, padding=2)
    self.relu2 = nn.ReLU()

    self.fc = nn.Linear(channel_2 * H * W, num_classes)

    nn.init.kaiming_normal_(self.conv1.weight)
    nn.init.kaiming_normal_(self.conv2.weight)
    nn.init.kaiming_normal_(self.fc.weight)

    nn.init.zeros_(self.conv1.bias)
    nn.init.zeros_(self.conv2.bias)
    nn.init.zeros_(self.fc.bias)

  def forward(self, x):
    scores = None
    x = self.conv1(x)
    x = self.relu1(x)
    x = self.conv2(x)
    x = self.relu2(x)
    x = flatten(x)
    x = self.fc(x)
    scores = x
    return scores
```

