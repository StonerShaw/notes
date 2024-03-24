# Tensor Basis
## Creating and Accessing tensors
``` Python
a = torch.tensor([1, 2, 3])
print('type(a): ', type(a))
print('rank of a: ', a.dim()) #a的维度
print('a.shape: ', a.shape) #a的形状
```

```Python
#Accessing two-dimensional tensor
print('b[0, 1]:', b[0, 1])
print('b[1, 2]:', b[1, 2])
b[1, 1] = 100
```
## Tensor Constructors
创建Tensor:
```Python
#Zero-Tensor
x = torch.zeros((3, 2))
x = torch.zeros(3, 2)
x = torch.zeros_like(x0) #保持x0的dtype和device,以及shape
x = x0.new_zeros(4, 5) #保持x0的dtype和device

#One-Tensor
x = torch.ones((3, 2))
x = torch.ones(3, 2)

x = torch.random(...)

x = eye(3) #Create a 3*3 identity matrix

x = torch.full((3, 2), 3.14) # Create a 3*2 matrix with 3.14 as elements

x =torch.arange(start, stop, dtype=torch.float64)
```
访问原始类型的元素：
```Python
x[0, 0].item()
```
指定dtype:
```Python
y0 = torch.tensor([1, 2], dtype=torch.float32)  # 32-bit float
y1 = torch.tensor([1, 2], dtype=torch.int32)    # 32-bit (signed) integer
y2 = torch.tensor([1, 2], dtype=torch.int64)    # 64-bit (signed) integer
```
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

# Tensor Indexing
## Slice indexing
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
## Integer tensor indexing
>More generally, given index arrays `idx0` and `idx1` with `N` elements each, `a[idx0, idx1]` is equivalent to:

```
torch.tensor([  a[idx0[0], idx1[0]],  a[idx0[1], idx1[1]],  ...,  a[idx0[N - 1], idx1[N - 1]]])
```
## Boolean tensor indexing
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
# Reshaping operations
## View
```Python
def flatten(x):
    return x.view(-1)

def make_row_vec(x):
    return x.view(1, -1)
```
>As its name implies, a tensor returned by `.view()` shares the same data as the input, so changes to one will affect the other and vice-versa

## Swapping axes
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
## Contiguous tensors
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
# Tensor operations
 
