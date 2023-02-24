# Numpy

# Create np array

a = np.array([1,2,3,4])

b = np.array([[1,2,3,4], [5.0,6,7,8])

c = np.zeros(2,3)

d = np.ones(2,3)

e = np.arange(1,10,2)

f = np.linspace(0,10, 5)

g = np.full((2,3), 8)

h = np.eye(2)

i = np.random.random((2,3))

# I /O
```python
>>> import numpy as np
>>> original_array = np.array([1, 2, 3])
>>> np.save('my_array', original_array)
>>> np.savetxt('my_array.txt', original_array)
>>> array_from_npy = np.load('my_array.npy')
>>> print(array_from_npy)
[1 2 3]
>>> array_from_txtfile = np.loadtxt('my_array.txt')
>>> print(array_from_txtfile)
[1. 2. 3.]
```