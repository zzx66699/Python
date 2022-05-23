# Packages
### collections
``` python
import collections
l = [1,1,1,2,3,1,2,3,4,5]
ans1 = collections.Counter(l)
print(l)
```

numpy
import numpy as np
array_1d = np.array([61, 52.5, 71, 32.5, 68, 64])
np包里的function
print(np.log(3))                    # Natural logarithm of 3
print(np.exp(np.arange(4)))          # e的多少次方
print(np.square(np.arange(3)))      # Squares of 0, 1, 2
--- [0 1 4]
print(np.power(np.arange(3), 4))    # Cubes of 0, 1, 2 n次方
--- [0 1 16]
np.sqrt(y)        # 开方 y**0.5
np.arange(0, 5+step, step)    # 等差序列，比起range多了小数
np.sum(distr)      # 求和
np.minimum(order[0], distr[1]))   # 比较取较小的值

matplotlib
import matplotlib.pyplot as plt
plt.plot(x, y)    # 线图
plt.scatter(x,y)  # 点图
plt.bar(x, y)  # 柱状图
plt.xlabel('x')   # 改变x和y的label
plt.show()
plot的attributes matplotlib.pyplot.plot — Matplotlib 2.1.1 documentation 详细用法
plt.plot(x, y, linewidth=3, linestyle=’:’, color=’r’, marker=’o’, markerfacecolor='blue', markersize=12)
常见用法：
linestyle=’:’ dot点线   ‘-.’ 点+线  ‘--’ dashed虚线
marker=‘o’ 圈   ‘v’ 三角   
color=’b’ blue   ‘g’ green   ‘r’ red   ‘m’ magenta洋红   ‘c’ cyan蓝绿   ‘y’ yellow   ‘k’ black   ‘w’ white
scatter的attributes
plt.scatter(size=z*50, alpha=0.2, c=’b’)  
```
