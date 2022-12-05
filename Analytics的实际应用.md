

### 从数据库中读取
``` python
# 连接数据库 获取连接对象 (这一步不同的数据库创建连接对象不一样)
import sqlite3 as sqlite3
conn = sqlite3.connect('data.sqlite')

# 读取库表中的数据值
df = pd.read_sql('select * from weather_2012', conn)
```


## (2) 获得每一列数据的特征
### 使用.unique()获取不重复的值 结果是array表示
``` python
condo['segment'].unique()

---
array(['CCR', 'RCR', 'OCR'], dtype=object)
```
可以把结果采用其他形式展现
``` python
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
types = iris['type'].unique()
for i in types:
    print(i)
```
![image](https://user-images.githubusercontent.com/105503216/177712737-df38f549-a319-4f13-a09a-a89d1d284b71.png)  



### 求某个categorical variable各value的占比!!数量！！！
``` python
# value=某个值的占比
print((data['gender']=='male').mean())                # 记得要加括号
0.5209125475285171                                    # 输出的是单独的string

# 各个value的数量
print(data['gender'].value_counts())                  # 输出的是pd.Series
Male      274
Female    252
Name: gender, dtype: int64

# 各个value的占比
print(data['gender'].value_counts(normalize=True))
Male      0.520913
Female    0.479087
Name: gender, dtype: float64
```

### 也可以把用于总数据的方法max\min\median\var....单独用在一列或者选取的多列上
#### 对多列用一种输出方法 结果是一列的series   
<img width="493" alt="image" src="https://user-images.githubusercontent.com/105503216/180693469-b0bd1a37-6d02-400e-8959-7dac30bfdd5f.png">  

EXERCEISE:  
你计算类型为“Iris-seosa”的鸢尾花的花萼长度、花萼宽度、花瓣长度、花瓣宽度的均值  
``` python
import pandas as pd
iris = pd.read_csv('iris.csv', sep=',')
print(iris.loc[iris['type']=='Iris-setosa','SepalLen':'PetalWid'].mean())  # 这里不要最后一列 用切片的方法

# 或者用numpy中的处理来做
import pandas as pd
import numpy as np
iris = pd.read_csv('iris.csv', sep=',')
print(np.mean(iris.loc[iris['type']=='Iris-setosa','SepalLen':'PetalWid']))  # np.mean()
```

#### 对一列用多种输出方法 必须一个一个写（每一个输出的是数字） 之后一般用pd.DataFrame整合 
每一个： <img width="169" alt="image" src="https://user-images.githubusercontent.com/105503216/180692027-cd30eb47-5035-4e54-a15d-16950543f1fe.png">   
整合起来也是一列 此时index是每种输出方法 column是列名   
<img width="448" alt="image" src="https://user-images.githubusercontent.com/105503216/180692233-f76d2540-7e41-43fe-8b35-8465a5af67d3.png">  
按照相应的格式修改  
<img width="806" alt="image" src="https://user-images.githubusercontent.com/105503216/180692754-583c1292-8ada-4433-8e22-4b7b4557f8c2.png">  


## (3) correlation
``` python
data.corr()
         wage	educ	 exper	  married
wage	1.000000	0.405903	 0.112903  0.228817
educ	0.405903	1.000000	 -0.299542 0.068881
exper	0.112903	-0.299542 1.000000  0.316984
married	0.228817	0.068881	 0.316984  1.000000
```
还可以通过图像来比对每两个变量之间的关系
``` python
import seaborn as sns
pplt = sns.pairplot(data)       # 这一步就已经可以画出图形了
pplt.fig.set_size_inches(6,6)   # 这里是调整图形大小
```




### filtering 只有先分组了 之后采用filter
在分组的基础上再按照条件筛选组别
``` python
# keep all groups in which the mean price is lower than 1800 dollars per square feet
outcome = condo.groupby('segment').filter(lambda x : x['unit_price'].mean() < 1800)
outcome.reset_index(drop=True, inplace=True) 
outcome
#drop是把原来的index去掉（因为reset_ndex会把原来的row index变成一个new column，所以要把它去掉）
#inplace是指make change to the original dataframe，而不是新建一个dataframe
# 特别注意这里不能写成outcome = condo.groupby('segment').filter(lambda x : x['unit_price'].mean() < 1800).reset_index(drop=True, inplace=True) 
# 因为这个返回值是空 所以不能在前面给它outcome=的赋值
```
![image](https://user-images.githubusercontent.com/105503216/171838747-2224a4b5-e0fa-484e-b527-266311cc71b6.png)

## (6) 关于时间的处理方法 pd.to_datetime(xx, '想要转变成为的格式')
把str变成Time series data   
这个网页是年月日表示:https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior    
<img width="728" alt="image" src="https://user-images.githubusercontent.com/105503216/184271993-afc17562-e1fa-4d96-8440-6cced72e5e4e.png">  


实例 Create a new dataset with all condo transactions from January 2018 to June 2018
``` python
# 首先转变成datetime的格式 并加入dataframe中
condo['time'] = pd.to_datetime(condo['date'], format='%b-%y')
# 接下来确定range的范围 注意range也要是datetime的形式
r = pd.to_datetime(['Jun-18','Jan-18'], format='%b-%y')
r
DatetimeIndex(['2018-06-01', '2018-01-01'], dtype='datetime64[ns]', freq=None)
# 下面开始根据range筛选 把头和尾拆开来分别判断
outcome = condo.loc[(condo['time'] <= r[0]) & (condo['time'] >= r[1])]   # 再次注意要()&()
outcome.reset_index(drop=True,inplace=True)
outcome
```
![image](https://user-images.githubusercontent.com/105503216/171841923-056dc369-532d-467c-ac09-0de9f99b584d.png) 

### 把有时间日期的变成只有日期的 .dt.date
<img width="327" alt="image" src="https://user-images.githubusercontent.com/105503216/183033909-72ad0dac-04a6-4e96-adad-b1e49ff57b75.png">  

``` python
b['date_x'] = pd.to_datetime(b['date']).dt.date  
```
<img width="224" alt="image" src="https://user-images.githubusercontent.com/105503216/183034085-a25e23c6-cdfa-4dda-89bc-838fdc8234c7.png">

### 时间差 from datetime import timedelta
相当于sql里的interval xx days
``` python
df['date_1'] = df['date_2'] + timedelta(day=1)
```

## (7) 矢量化字符串操作Vectorized string operations
通过`str`矢量化字符串  
### 1.lower（）变成小写
``` python
condo['name'].str.lower()
0                    seascape
1         commonwealth towers
2                 the trilinq
3                   the crest
4               the anchorage
                 ...         
32163          skies miltonia
32164         symphony suites
32165               seletaris
32166    riverbank @ fernvale
32167             the estuary
Name: name, Length: 32168, dtype: object
```
### 2.把一列中得每一项都截取xx部分
Notice that the condo level is given as a string "XX to YY".   
Create two columns level_from and level_to, that are the level numbers "XX" and "YY", as int type values.
``` python
condo['level_from'] = condo['level'].str[:2].astype(int)    # 如果不加.str就会只能截取第一行的前2个，后面都是NaN
condo['level_to'] = condo['level'].str[-2:].astype(int)     # astype()可以改变type astype在使用的时候前面不用加str 这里是为了切片才加的str 
# 同时有返回值 不是在原dataframe上的改变 所以需要赋值
```
### 3.求每个字符串的长度
Nowcoder['Name'].str.len()


## (9) 创建interval
``` python
age = pd.cut(data_titian['age'], [0,18,50,80])  # 按照4个节点分成3个interval
age.head(10)
0    (18.0, 50.0]
1    (18.0, 50.0]
2    (18.0, 50.0]
3    (18.0, 50.0]
4    (18.0, 50.0]
5             NaN
6    (50.0, 80.0]
7     (0.0, 18.0]
8    (18.0, 50.0]
9     (0.0, 18.0]
Name: age, dtype: category
Categories (3, interval[int64]): [(0, 18] < (18, 50] < (50, 80]]
```

## 按照几分位点划分 pd.qcut()
pd.qcut(被切的一列或者多列, q, labels=每一组的名字, retbins=False, precision=3, duplicates='raise') #最后一个参数  
EXERCISE:
<img width="635" alt="image" src="https://user-images.githubusercontent.com/105503216/183863467-07b5f9c8-acb7-47a5-a119-7725746f8405.png">

``` python
import pandas as pd
sales = pd.read_csv('sales.csv')
sales['R_Quartile'] = pd.qcut(sales['recency'], [0, 0.25, 0.5, 0.75, 1],[4,3,2,1]).astype("int")
sales['F_Quartile'] = pd.qcut(sales['frequency'], [0, 0.25, 0.5, 0.75, 1],[1,2,3,4]).astype("int")
sales['M_Quartile'] = pd.qcut(sales['monetary'], [0, 0.25, 0.5, 0.75, 1],[1,2,3,4]).astype("int")
print(sales.head())
```

<img width="664" alt="image" src="https://user-images.githubusercontent.com/105503216/183874156-fb2c2c21-ec9f-4d65-90ed-33c9663e3fd7.png">

``` python
sales['RFMClass'] = sales['R_Quartile'] + sales['F_Quartile'] + sales['M_Quartile'] 
df = sales[['user_id','recency','frequency','monetary','RFMClass']]
print(df.head(5))
print('\n')
df = df.loc(sales['RFMClass']=='444').sort_values(by='monetary', ascending=False).iloc[:5].reset_index(drop=True)
print(df)
```
