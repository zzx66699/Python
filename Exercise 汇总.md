# Exercise汇总
## 1
![image](https://user-images.githubusercontent.com/105503216/177705208-d17f13d7-0c21-4b40-9b55-f45342b4bb6f.png)  
1.1 请你统计类型为"Iris-versicolor"与类型为"Iris-setosa"的鸢尾花的最大花萼长度之差
``` python
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
max1 = iris.loc[iris['type']=='Iris-setosa','SepalLen'].max()        # 在table中找type是Iris-setosa的那几行中SepalLen的最大值
max2 = iris.loc[iris['type']=='Iris-versicolor','SepalLen'].max()
result = round(max2 - max1, 1)
print(result)
```

1.2 请你计算类型为“Iris-seosa”的鸢尾花的花萼长度、花萼宽度、花瓣长度、花瓣宽度的均值  
![image](https://user-images.githubusercontent.com/105503216/177733292-3ebbd3e5-a8d9-4ba6-af1d-3896996581a4.png)  
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

1.3 请你计算类型为“Iris-seosa”的鸢尾花的花萼长度、花萼宽度、花瓣长度、花瓣宽度的方差  
![image](https://user-images.githubusercontent.com/105503216/177732999-414d413e-2da6-4ae8-ae56-4fa1b2a11117.png)  
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

1.4 请你统计该数据集中花萼长度、花萼宽度、花瓣长度、花瓣宽度的0.25、0.5、0.75分位数
![image](https://user-images.githubusercontent.com/105503216/177731749-303c5c10-eab0-44b6-9f3f-c3850efae386.png)
``` sql
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
result = iris.iloc[:,0:4]
print(result.quantile([0.25, 0.50, 0.75]))    # 注意这里是0.50 此外 注意多个分位数在一起的写法 
```

1.5 请你统计花萼长度、花萼宽度、花瓣长度、花瓣宽度各列数据的最大值和最小值  
![image](https://user-images.githubusercontent.com/105503216/177739843-4634a967-f2c4-4368-9d75-f7b152f11083.png)
``` python
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
i = iris.iloc[:,:4]
data = pd.concat([i.max(), i.min()], axis=1).T    # 注意 max时生成的是index=column name，value=max的pandas.Series 根据列合并 然后转置
data.index = ['themax','themin']                  # 这里开始改index的名称
print(data)

# 或者用describe来取
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
i = iris.iloc[:,:4]
data = i.describe().loc[['max','min'],]
data.index = ['themax','themin']
print(data)
```

1.6 请你统计SepalLen列的中位数，SepalWid列的最大值和最小值  
![image](https://user-images.githubusercontent.com/105503216/177751561-f7062dc1-dcbc-4b9a-83f9-761269eeab01.png)
``` sql
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
df1 = pd.DataFrame(iris['SepalLen'].median(), index=['median'], columns=['SepalLen'])
df2 = pd.DataFrame([iris['SepalWid'].max(), iris['SepalWid'].min()], 
                   index = ['max','min'], columns=['SepalWid'])
df = pd.concat([df1, df2], axis=1)             # 不相同的index也可以直接合并
print(df)
```

``` sql
f = pd.read_csv(feed.csv)
fc = pd.read_csv(feed_comment.csv)

```
