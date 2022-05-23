# Packages
## collections
``` python
import collections
l = [1,1,1,2,3,1,2,3,4,5]
ans1 = collections.Counter(l)
print(ans1)
--- Counter({1: 4, 2: 2, 3: 2, 4: 1, 5: 1})              # 表示1出现4次，2出现2次....
```

## numpy
```python
import numpy as np
array_1d = np.array([61, 52.5, 71, 32.5, 68, 64])        # array数组的形式  

np.arange(4.334)                                         # 默认step为1，并且从0开始，输出的都是整数
--- array([0., 1., 2., 3., 4.])
step=0.1
np.arange(0, 10+step, step)                              # 包含开头，不包含结尾，步长为0.1

np.log(3)                                                # Natural logarithm of 3
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

## math
``` python
math.pi           # π
```

