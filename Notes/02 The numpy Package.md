# Chapter2 array

## 1. 构建赋值转化
``` python
import numpy as np
# 1维矩阵
array_1d = np.array([61, 52.5, 71, 32.5, 68, 64])                           # 这是从list转化成了array

# 2维矩阵
array1 = np.array([[1,2,3],[4,5,6]])                                        # 依然要用list，list中包含了多个子list，每个子list作为一行

# 一些特殊的构建
ones_2d = np.ones(shape=(3, 5))                                             # 全是1
---
array([[1., 1., 1., 1., 1.],
       [1., 1., 1., 1., 1.],
       [1., 1., 1., 1., 1.]])
       
zeros_1d = np.zeros(shape=5)                                                # 全是0 

range_array = np.arange(2, 5, 0.5)                                          # 使用np.arange函数进行构建 start=2, stop=5, and step=0.5
np.arange(3)   --- 0，1，2
```

### 1.1 随机生成
data=np.random.randint(从xx数开始,到xx数不包括, size=(xx,xx))

``` python
# 生成从0-29的数字！！！！一共有2行5列
data=np.random.randint(0,30, size=(2,5))
```
<img width="272" alt="image" src="https://user-images.githubusercontent.com/105503216/184530837-0b8707c1-2113-4439-8c4d-e8c1944e2a62.png">

转化为df

``` python
df = pd.DataFrame(data=np.random.randint(0,100,size=(100,3)),columns=['A','B','C'])
```

#### 1.1.2 随机生成满足标准正态分布的一组数

1. 一个参数表示一行xx列  

``` python
import numpy as np
array = np.random.randn(3)
print(array)
```

<img width="336" alt="image" src="https://user-images.githubusercontent.com/105503216/193440067-f05a072b-09eb-4f38-a03c-f72d760faa35.png">  

2. 两个参数表示 a行b列的矩阵  

``` python
import numpy as np
array = np.random.randn(10, 2)
print(array)
```
<img width="245" alt="image" src="https://user-images.githubusercontent.com/105503216/193440051-d1a882a2-5253-4ada-bcf4-3b1f8b5be52d.png">  

3. 三个参数表示a个小矩阵汇成一个大矩阵 每一个小矩阵都是行为b列为c的  

``` python
import numpy as np
array = np.random.randn(3,4,2)
print(array)
```

<img width="271" alt="image" src="https://user-images.githubusercontent.com/105503216/193440113-51ed095a-61b2-42c4-b7cb-0a12fdaf0bb9.png">  


#### 把0到xx-1的整数乱序排序

``` python
# 把0-29的数据随机排序 不会重复
np.random.permutation(30)  
```
<img width="652" alt="image" src="https://user-images.githubusercontent.com/105503216/184530872-a40e858b-41a7-4c4c-ba9a-2c97ff6c56b4.png">


## 查看基础特征
``` python
data.ndim                                          # 查看维度
array1.size                                        # 多少个数 number of data items
array1.dtype                                       # type类型 一般都是float64

data.shape                                         # 查看行列数 结果用tuple形式展现 
a = np.array([1,2,3,4,5])
a.shape --- (5,)                                   # 特别注意一维的shape表达
b = np.array([[1],[2],[3],[4],[5]])
b.shape --- (5,1)                                  # 先行后列 
``` 

## 替换
```
array1 = np.array([[1,2,3],[4,5,6]])
array1[1,1] = 2                                    # 直接使用index更改，和list一样
array1
---
array([[1, 2, 3],
       [4, 2, 6]])

# 同时element-wise，所有元素同时改变
array_2d = np.array([[1, 2],    
                     [2, 3.5],     
                     [5, 6.5]]) 
print(array_2d + 3) 
---
[[4.  5. ]
 [5.  6.5]
 [8.  9.5]]

# 有broadcasting性，即缺少的位置会自动补全
np.ones((3, 3)) + np.arange(3) 
---
array([[1., 2., 3.],
       [1., 2., 3.],
       [1., 2., 3.]])

np.arange(3).reshape((3, 1)) + np.arange(3)
---
array([[0, 1, 2],
       [1, 2, 3],
       [2, 3, 4]])
```

## 切片
``` python
array_1d = np.array([61, 52.5, 71, 32.5, 68, 64])  # This is a one-dimensional array
array_2d = np.array([[18, 26, 17], 
                     [25, 15.5, 12], 
                     [24, 27, 20],
                     [10, 5.5, 17],
                     [27, 26, 15],
                     [22, 21, 21]])
print(array_2d[0, 1])                              # 某一个值 先行后列
--- 26     
print(array_1d[2:])                                # Slicing for 1d array
---
[71.  32.5 68.  64. ]
print(array_2d[3: , ])                             # Slicing for 2d array 连续的几列 
---
[10, 5.5, 17],
[27, 26, 15],
[22, 21, 21]])
print(array_2d[:-2, ])                             # 不要最后2行 
---
[[18.  26.  17. ]
 [25.  15.5 12. ]
 [24.  27.  20. ]
 [10.   5.5 17. ]]
print(array_2d[[0, 2, 1]])                         # 分开的几列 要用list
---
[[18.  26.  17. ]
 [24.  27.  20. ]
 [25.  15.5 12. ]]
print(array_2d[:, [0,2]])                          # 分开的几行 要用list 且前面要加,
---
[[18. 17.]
 [25. 12.]
 [24. 20.]
 [10. 17.]
 [27. 15.]
 [22. 21.]]
```

## Functions  
``` python
array_2d = np.array([[1, 2],    
                     [2, 3.5],     
                     [5, 6.5]])  
print(array_2d.sum())               # Sum of all item
print(array_2d.max())               # The maximum item in the array
print(array_2d.min())               # The minimum item in the array

# 按axis求值 列0行1
print(array_2d.sum(axis=0))         # 每一列求和
print(array_2d.sum(axis=1))         # 每一行求和
print(array_2d.max(axis=0))         # 每一列的最大值
print(array_2d.min(axis=1))         # 每一行的最小值
---
[ 8. 12.]
[ 3.   5.5 11.5]
[5.  6.5]
[1. 2. 5.]                          # 只要是.sum()的形式，输出都是维度为1的array
```

## numpy包里的一些用法
```python
np.arange(4.334)                                         # 默认step为1，并且从0开始，输出的都是整数
--- array([0., 1., 2., 3., 4.])
step=0.1
np.arange(0, 10+step, step)                              # 包含开头，不包含结尾，步长为0.1

np.log(3)                                                # Natural logarithm of 3
np.log10(1+1/d)                                          # p = log10(1+d) log几就在旁边写几

np.sin(2) & np.cos(4)                                    # sin和cost
print(np.exp(np.arange(4)))                              # e的多少次方
print(np.square(np.arange(3)))                           # Squares of 0, 1, 2 
--- [0 1 4]  
print(np.power(np.arange(3), 4))                         # Cubes of 0, 1, 2 任意n次方，这里的4表示4次方
--- [0 1 16]  
np.sqrt(y)                                               # 开方 y**0.5
np.arange(0, 5+step, step)                               # 等差序列，比起range多了小数
np.sum(distr)                                            # 求和
np.minimum(order[0], distr[1]))                          # 比较取较小的值
```
