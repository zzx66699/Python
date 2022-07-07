# Exercise汇总
## 1
![image](https://user-images.githubusercontent.com/105503216/177705208-d17f13d7-0c21-4b40-9b55-f45342b4bb6f.png)
``` python
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
max1 = iris.loc[iris['type']=='Iris-setosa','SepalLen'].max()        # 在table中找type是Iris-setosa的那几行中SepalLen的最大值
max2 = iris.loc[iris['type']=='Iris-versicolor','SepalLen'].max()
result = round(max2 - max1, 1)
print(result)
```

## 2
请你计算类型为“Iris-seosa”的鸢尾花的花萼长度、花萼宽度、花瓣长度、花瓣宽度的均值
``` python
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
print(iris.loc[iris['type']=='Iris-setosa','SepalLen':'PetalWid'].mean()) 
# 或者用numpy中的处理来做
import pandas as pd
import numpy as np
iris = pd.read_csv('iris.csv', sep=',')
print(np.mean(iris.loc[iris['type']=='Iris-setosa','SepalLen':'PetalWid'])) 
```
请你计算类型为“Iris-seosa”的鸢尾花的花萼长度、花萼宽度、花瓣长度、花瓣宽度的方差  
这里需要特别注意  
df.var()默认的ddof=1 是用来算样本方差的 标准差公式根号内除以 n-1   
np.var(xx)默认的ddof=0 是用来算总体标准偏差 标准差公式根号内除以 n  
这里算的是总体标准偏差
``` python
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
v = iris.loc[iris['type']=='Iris-virginica', 'SepalLen':'PetalWid']
print(v.var(ddof=0))  # 如果用df.var() 需要设置ddof为0
## 或者还是用np.var(xx)做更方便
import pandas as pd
import numpy as np
iris = pd.read_csv('iris.csv', sep=',')
v = iris.loc[iris['type']=='Iris-virginica', 'SepalLen':'PetalWid']
print(np.var(v)) 
```


