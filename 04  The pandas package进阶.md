# Chapter4 The pandas package进阶
# Descriptive analytics 
## Read data from files
``` python
import pandas as pd
data = pd.read_csv('wage.csv')  # Read data from a file "wage.csv"
data.head(6)                    # Return the first six rows of data
```
## (1)获得总体数据的基础特征
### .describe()
一键获取基础信息
``` python
data.describe()
      wage	      educ	       exper
count	526.000000	526.000000	526.00000
mean	5.896103	  12.562738	  17.01711
std	3.693086	    2.769022	  13.57216
min	0.530000	    0.000000	  1.00000
25%	3.330000	    12.000000	  5.00000
50%	4.650000  	  12.000000	  13.50000
75%	6.880000	    14.000000	  26.00000
max	24.980000	    18.000000	  51.00000
```
### centers
包括mean平均值和median中位数
``` python 
print(data.select_dtypes(exclude='object').mean())      # 求每一列的平均值 这里的select_dtypes(exclude='object')是把categorical variable去掉
wage        5.896103
educ       12.562738
exper      17.017110
married     0.608365
dtype: float64 

print(data.select_dtypes(exclude='object').median()))   # 每一列的中位数
wage        4.65
educ       12.00
exper      13.50
married     1.00
dtype: float64 

print(type(data.select_dtypes(exclude='object').mean()))
<class 'pandas.core.series.Series'>                     # 输出类型都是pandas.Series
#每一行的indice是原来的列名称variable name
```

### variations
包括方差和标准差
``` python
print(data.select_dtypes(exclude='object').std())             # 标准差
print(data.select_dtypes(exclude='object').var())             # 方差
```
这里需要特别注意
df.var()默认的ddof=1 是用来算样本方差的 标准差公式根号内除以 n-1  
np.var(xx)默认的ddof=0 是用来算总体标准偏差 标准差公式根号内除以 n   
EXERCISE:  
请你计算类型为“Iris-seosa”的鸢尾花的花萼长度、花萼宽度、花瓣长度、花瓣宽度的方差  
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

### extreme points
包括最大值和最小值
``` python
print(data.select_dtypes(exclude='object').min())
print(dara.select_dtypes(exclude='object').max())
```
注意需要把前面的部分()起来  !!!!! 

``` python
ne = Nowcoder.loc[Nowcoder['Num_of_exercise']>10,'Num_of_exercise'] 
ns = Nowcoder.loc[Nowcoder['Num_of_exercise']>10,'Number_of_submissions']
max_c = (ne/ns).max()   # 计算结果的max 要把计算过程()  
print(round(max_c,3))
```

### 几分位点
df.quantile(xx) 
``` python
print(result.quantile([0.25, 0.50, 0.75]))    # 注意这里是0.50 此外 注意多个分位数在一起的写法 
```

### 众数
.mode()


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

### 使用nunique()获取不重复值的数量 结果是int
``` python
Nowcoder = pd.read_csv('Nowcoder.csv', sep=',')
language = Nowcoder['Language'].unique()
count = Nowcoder['Language'].nunique()
print(count, '\n', language)
```

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


## (5) 按分组将多列同时进行多种操作
### aggregate 每一组只输出一个值 所以输出的行数=组别数
**单一column分组 单一column中对应的mean和std**  
1.普通写法
``` python
# 找出每一个unique值 -- 分别做出subset -- 求mean和std -- 合并
mean = []
std = [] 
for segment in data['segment'].unique():            # Iterate all values of segments
    subset = data.loc[data['segment'] == segment]   # Take a subset for one segment
    means.append(subset['price'].mean())            # Append the mean of the subset
    stds.append(subset['price'].std())              # Append the std of the subset

print('Average prices:      ', means)
print('Standard deviations: ', stds)
```
2.使用groupby以及agg()函数的普通形式
.groupby
``` python
price_means = data.groupby('segment')['price'].mean()        # groupby相当于把原数据按照unique的值分成了多个subset
price_means
segment
CCR    3.299744e+06
OCR    1.129063e+06
RCR    1.644209e+06
Name: price, dtype: float64
```
agg()可以同时使用多种算法 相当于.mean() .max() .min()等函数的集合
对于一个column用多个function
``` python
funs = ['count', 'median', 'max', 'min']
condo.groupby('segment')['price'].agg(funs)                  # groupby后面用的是() 不是[]

    count	 median	 max	    min
segment				
CCR	5107	2450000	52000000	560088
OCR	16652	1069590	4881708	488000
RCR	10409	1490000	19000000	570000

price_info.columns
Index(['count', 'median', 'max', 'min'], dtype='object')

price_info.index
Index(['CCR', 'OCR', 'RCR'], dtype='object', name='segment')

# 可以把index转化成第一列 然后生成新的index
price_info.reset_index()                        # 但原dataset不变
  segment	count	  median	max	     min
0	CCR	    5107	2450000	52000000	560088
1	OCR	    16652	1069590	4881708	  488000
2	RCR	    10409	1490000	19000000	570000
```
3.使用def定义需要的function
例如要求四分位距 即0.75-0.25
``` python
def func(x):
    return x.quantile(0.75) - x.quantile(0.25)
condo.groupby('segment')['price'].agg(func)    # 注意这里把用function的那一列写在前面 func后面不加()了
segment
CCR    2010000
OCR     470000
RCR     708800
Name: price, dtype: int64
```
4.使用lambda简化function
typically used to return the result expressed by a single-line statement 单一行的计算结果
``` python
condo.groupby('segment')['price'].agg(lambda x : x.quantile(0.75) - x.quantile(0.25)).reset_index()
  segment	price
0	CCR	    2010000
1	OCR	    470000
2	RCR	    708800
```
**单一column分组 多个column用function**
``` python
funs = ['count', 'median', 'max', 'min']
f = condo.groupby('segment')[['price','unit_price']].agg(funs)
f
    price	                          unit_price
    count	median	max	min	          count	median	max	min
segment								
CCR	5107	2450000	52000000	560088	5107	1937	4913	684
OCR	16652	1069590	4881708	488000	16652	1078	2285	485
RCR	10409	1490000	19000000	570000	10409	1560	2908	597

f.columns
MultiIndex([(     'price',  'count'),
            (     'price', 'median'),
            (     'price',    'max'),
            (     'price',    'min'),
            ('unit_price',  'count'),
            ('unit_price', 'median'),
            ('unit_price',    'max'),
            ('unit_price',    'min')],
           )
           
f.index
Index(['CCR', 'OCR', 'RCR'], dtype='object', name='segment')

# 同样可以把index转化成第一列 然后生成新的index
a.reset_index()
 segment price	            unit_price
      count	median	max	min	count	median	max	min
0	CCR	5107	2450000	52000000	560088	5107	1937	4913	684
1	OCR	16652	1069590	4881708	488000	16652	1078	2285	485
2	RCR	10409	1490000	19000000	570000	10409	1560	2908	597
```
**单一column分组 多个column用不同的function**  
用dictionary 不同的column对应不同的function
``` python
d = {'price':['max','min'],
     'unit_price':['count','mean']}               # 注意这里前面的key只能是单一值 不能是[‘xx’,‘xx’]
condo.groupby('segment').agg(d).reset_index()
  segment	price	        unit_price
       max	    min	    count	mean
0	CCR	52000000	560088	5107	2047.080674
1	OCR	4881708	488000	16652	1098.835275
2	RCR	19000000	570000	10409	1544.190220
```
**多个column分组**
简单方法可以参考pivot_table
Explore the percentage of survival by sex and classes.
``` python
d = data_titan.groupby(['sex','class'])['survived'].mean()
d
sex     class 
female  First     0.968085
        Second    0.921053
        Third     0.500000
male    First     0.368852
        Second    0.157407
        Third     0.135447
Name: survived, dtype: float64

d.index            # 两层index row indices of the series above have two layers, containing all combinations of values of sex and class.
MultiIndex([('female',  'First'),
            ('female', 'Second'),
            ('female',  'Third'),
            (  'male',  'First'),
            (  'male', 'Second'),
            (  'male',  'Third')],
           names=['sex', 'class'])
	   
# 通过unstack()可以把inner layer转化成column name
d = d.unstack()   # 这是有返回值的
d
class	First	        Second	        Third
sex			
female	0.968085	0.921053	0.500000
male	0.368852	0.157407	0.135447
```
### transformation 行数永远和原数据的行数一样
例如把每个值normalize 标准化处理 去除量纲dimension 使不同数量级scale的数据能够比较
``` python
# def
def func(x):
    return (x - x.min()) / (x.max() - x.min())
condo.groupby('segment')['price'].transform(func)
# 或者lambda
condo.groupby('segment')['price'].transform(lambda x : (x - x.min()) / (x.max() - x.min()))
0        0.074415
1        0.039609
2        0.288367
3        0.082203
4        0.069392
           ...   
32163    0.177527
32164    0.076473
32165    0.173430
32166    0.135193
32167    0.027767
Name: price, Length: 32168, dtype: float64
```
### apply
apply既可以当成agg也可以当成transform 根据后面的函数来定 但是一般不用 因为不明确
``` python
price_iqr = data.groupby('segment')['price'].apply(lambda x: 
                                                   x.quantile(0.75) - x.quantile(0.25))

price_norm = data.groupby('segment')['price'].apply(lambda x: 
                                                    (x-x.min())/(x.max()-x.min()))
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

## (6) 关于时间的处理方法
pd.to_datetime()把str变成Time series data  
这个网页是年月日表示:https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior
``` python
time = pd.to_datetime(condo['date'], format='%b-%y')
time
0       2019-11-01
1       2019-11-01
2       2019-11-01
3       2019-11-01
4       2019-11-01
           ...    
32163   2016-11-01
32164   2016-11-01
32165   2016-11-01
32166   2016-11-01
32167   2016-11-01
Name: date, Length: 32168, dtype: datetime64[ns]
```
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

## (8) 按照某一列重新排序 .sort_values()
Considering Singapore condos in the district 5, show the monthly trends of  
1) the average unit prices; and 2) the number of transactions, in recent years.
``` python
subset = condo.loc[condo['district_code']==5]
fun = ['mean','count']
o = subset.groupby('time')['unit_price'].agg(fun)
o.reset_index(inplace=True)
o.head(5)                                # 此时是按照alphabetical的顺序排序的 我们希望按照时间顺序
	date	mean	        count	
0	Apr-17	1162.106195	113	
1	Apr-18	1151.673913	46	
2	Apr-19	1197.903226	31	
3	Aug-17	1174.113924	79	
4	Aug-18	1244.913043	23	

# 可以新建一列datetime 按照datetime排序
o['time']=pd.to_datetime(o['date'], format='%b-%y')  # 新建一列datetime的
o.sort_values(by='time',inplace=True)                # 注意这里不要o.sort_values(by='time',inplace=True).reset_index(drop=True, inplace=True) 连着写
o.reset_index(drop=True, inplace=True)               # 因为每一步都不会有返回值的 都是在对原序列操作
o.head(5)
date	mean	count	time
0	Nov-16	1218.390244	41	2016-11-01
1	Dec-16	1141.553191	47	2016-12-01
2	Jan-17	1241.507937	63	2017-01-01
3	Feb-17	1237.646739	184	2017-02-01
4	Mar-17	1219.400000	160	2017-03-01
```

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

## (10) 数据透视表 Pivot table 
Explore the percentage of survival by sex and classes.
``` python
d = data_titan.pivot_table('survived', columns='class', index='sex')
d           # 这里可以参考agg里面的多个column分组 简化写法
class	First	        Second	        Third
sex			
female	0.968085	0.921053	0.500000
male	0.368852	0.157407	0.135447

d.index.name = None   # 把index和column的名字删去
d.columns.name = None
d
        First	        Second	        Third
female	0.968085	0.921053	0.500000
male	0.368852	0.157407	0.135447
```
index也可以是multi layer
``` python
d = data_titan.pivot_table('survived', index=[age, 'sex'], columns='class') 
# 注意这里的age都不是dataframe中的一列 是独立的variable 默认一一对应起来了
```
![image](https://user-images.githubusercontent.com/105503216/171981570-d42874ad-a548-4439-a5f0-113061ea748a.png)
