---
title: Pytorch基础
---
转换dtype:
```Python
x0 = torch.eye(3, dtype=torch.int64)
x1 = x0.float()  # Cast to 32-bit float
x2 = x0.double() # Cast to 64-bit float
x3 = x0.to(torch.float32) # Alternate way to cast to 32-bit float
x4 = x0.to(torch.float64) # Alternate way to cast to 64-bit float

x3 = torch.ones(6, 7).to(x0)
```
Element-wise Operation:
```Python
x = torch.arange(start, stop, dtype=torch.float64)
print(x)
print(x % 10 ==0)
x = x[x % 10 == 0]
```
>tensor([ 5., 6., 7., 8., 9., 10., 11., 12., 13., 14., 15., 16., 17., 18., 19., 20., 21., 22., 23., 24.], dtype=torch.float64) 
>tensor([False, False, False, False, False, True, False, False, False, False, False, False, False, False, False, True, False, False, False, False])

## Tensor Indexing
### Slice indexing
```Python
a = torch.tensor([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
print(a[1, :])
print(a[1])  # Gives the same result; we can omit : for trailing dimensions

a = torch.tensor([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
row_r1 = a[1, :]    # Rank 1 view of the second row of a
row_r2 = a[1:2, :]  # Rank 2 view of the second row of a 保留维度

a = torch.zeros(2, 4, dtype=torch.int64)
a[:, :2] = 1
a[:, 2:] = torch.tensor([[2, 3], [4, 5]])
print(a)
```
>tensor([[1, 1, 2, 3], [1, 1, 4, 5]])
### Integer tensor indexing
>More generally, given index arrays `idx0` and `idx1` with `N` elements each, `a[idx0, idx1]` is equivalent to:

```
torch.tensor([  a[idx0[0], idx1[0]],  a[idx0[1], idx1[1]],  ...,  a[idx0[N - 1], idx1[N - 1]]])
```
### Boolean tensor indexing
```Python
# Find the elements of a that are bigger than 3. The mask has the same shape as
# a, where each element of mask tells whether the corresponding element of a
# is greater than three.
mask = (a > 3)
print('\nMask tensor:')
print(mask)

# We can use the mask to construct a rank-1 tensor containing the elements of a
# that are selected by the mask
print('\nSelecting elements with the mask:')
print(a[mask])
```
## Reshaping operations
### View
```Python
def flatten(x):
    return x.view(-1)

def make_row_vec(x):
    return x.view(1, -1)
```
>As its name implies, a tensor returned by `.view()` shares the same data as the input, so changes to one will affect the other and vice-versa

### Swapping axes
```Python
x = torch.tensor([[1, 2, 3], [4, 5, 6]])
print('Original matrix:')
print(x)

print('\nTransposing with view DOES NOT WORK!')
print(x.view(3, 2))

print('\nTransposed matrix:')
print(torch.t(x))
print(x.t())
```
More than two dimensions(transpose, permute):
```Python
# x.shape: [2, 3, 4]
# Swap axes 1 and 2; shape is (2, 4, 3)
x1 = x0.transpose(1, 2)
print('\nSwap axes 1 and 2:')
print(x1)
print(x1.shape)


# Permute axes; the argument (1, 2, 0) means:
# - Make the old dimension 1 appear at dimension 0;
# - Make the old dimension 2 appear at dimension 1;
# - Make the old dimension 0 appear at dimension 2
# This results in a tensor of shape (3, 4, 2)
x2 = x0.permute(1, 2, 0)
print('\nPermute axes')
print(x2)
print('shape:', x2.shape)
```
### Contiguous tensors
```Python
x0 = torch.randn(2, 3, 4)

try:
  # This sequence of reshape operations will crash
  x1 = x0.transpose(1, 2).view(8, 3)
except RuntimeError as e:
  print(type(e), e)

# We can solve the problem using either .contiguous() or .reshape()
x1 = x0.transpose(1, 2).contiguous().view(8, 3)
x2 = x0.transpose(1, 2).reshape(8, 3)
```
## Tensor operations

### Elementwise operations
```Python
x + y, torch.add(x, y), x.add(y)
x - y, torch.sub(x, y), x.sub(y)
x * y, mul
x / y, div
x ** y, pow

torch.sqrt(x), x.sqrt()
sin
cos
```
### Reduction operations
```Python
torch.sum(x, dim=1), x.sum(dim=1)
# dim = n, the n-th dimension will be eliminated

torch.sum(x, dim=1, keepdim=True)
```
### Matrix operations
>- [`torch.dot`](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fpytorch.org%2Fdocs%2Fstable%2Fgenerated%2Ftorch.dot.html): Computes inner product of vectors
>- [`torch.mm`](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fpytorch.org%2Fdocs%2Fstable%2Fgenerated%2Ftorch.mm.html): Computes matrix-matrix products (two-dimension)
>- [`torch.mv`](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fpytorch.org%2Fdocs%2Fstable%2Fgenerated%2Ftorch.mv.html): Computes matrix-vector products
>- [`torch.addmm`](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fpytorch.org%2Fdocs%2Fstable%2Fgenerated%2Ftorch.addmm.html) / [`torch.addmv`](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fpytorch.org%2Fdocs%2Fstable%2Fgenerated%2Ftorch.addmv.html): Computes matrix-matrix and matrix-vector multiplications plus a bias
>- [`torch.bmm`](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fpytorch.org%2Fdocs%2Fstable%2Fgenerated%2Ftorch.bmm.html) / [`torch.baddmm`](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fpytorch.org%2Fdocs%2Fstable%2Fgenerated%2Ftorch.baddbmm.html): Batched versions of `torch.mm` and `torch.addmm`, respectively
>- [`torch.matmul`](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fpytorch.org%2Fdocs%2Fstable%2Fgenerated%2Ftorch.matmul.html): General matrix product that performs different operations depending on the rank of the inputs. Confusingly, this is similar to `np.dot` in numpy.
## Broadcasting
```Python
# We will add the vector v to each row of the matrix x,
# storing the result in the matrix y
x = torch.tensor([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = torch.tensor([1, 0, 1])
y = x + v  # Add v to each row of x using broadcasting
print(y)
```
>tensor(\[\[ 2, 2, 4], [ 5, 5, 7], [ 8, 8, 10], [11, 11, 13]])

>1. If the tensors do not have the same rank, prepend the shape of the lower rank array with 1s until both shapes have the same length.
>2. The two tensors are said to be _compatible_ in a dimension if they have the same size in the dimension, or if one of the tensors has size 1 in that dimension.
>3. The tensors can be broadcast together if they are compatible in all dimensions.
>4. After broadcasting, each tensor behaves as if it had shape equal to the elementwise maximum of shapes of the two input tensors.
>5. In any dimension where one tensor had size 1 and the other tensor had size greater than 1, the first tensor behaves as if it were copied along that dimension

```Python
# use torch.broadcast_tensors to see how the tensors are broadcast
xx, vv = torch.broadcast_tensors(x, v)
```
### Out-of-place vs in-place operators

>Most PyTorch operators are classified into one of two categories:
>- **Out-of-place operators:** return a new tensor. Most PyTorch operators behave this way.
>- **In-place operators:** modify and return the input tensor. Instance methods that end with an underscore (such as `add_()` are in-place. Operators in the `torch` namespace can be made in-place using the `out=` keyword argument.

## Running in GPU
```Python
import torch

if torch.cuda.is_available():
  print('PyTorch can use GPUs!')
else:
  print('PyTorch cannot use GPUs.')
  
a_gpu = a_cpu.cuda()
b_gpu = b_cpu.cuda()
```

