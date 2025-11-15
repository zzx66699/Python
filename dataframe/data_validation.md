## 1. unique
### 1.1 unique: returns unique values  
Return an array  
<img width="1018" alt="image" src="https://user-images.githubusercontent.com/105503216/181669661-6cd518d2-9244-47f3-b3c6-59438b60d374.png">

### 1.2 nunique(): returns the number of unique values for each column.
Return a number
``` python
language = Nowcoder['Language'].unique()
```
----------------

## 2. describe
check the max, min, mean... 
``` python
df["unit_qty"].describe()

#
```
----------------
## 3. isna
check if there is NaN
``` python
df[df["unit_qty"].isna()]
```
### 查看列名
``` python
price_info.columns   

Index(['count', 'median', 'max', 'min'], dtype='object')    
```

#### 2.1.1 改变列名

```python
import pandas as pd
import numpy as np
exam_data = [{'name':'Anastasia', 'score':12.5}, {'name':'Dima','score':9}, {'name':'Katherine','score':16.5}]
df = pd.DataFrame(exam_data)

df = df.rename(columns={'name':'col1','score':'col2'})  # 用字典表示对应的替换
print(df)
```

或者也可以直接替换/赋值列名

```python
Overall_sg.columns = ['Metrics', 'Pro rata May', '5-22 June']
```

### 2.2 查看行名
.index 注意这个常常用于找到xxx条件所对应的行 具体的index是多少！！！

``` python
price_info.index   

Index(['CCR', 'OCR', 'RCR'], dtype='object', name='segment')
```

找到有缺失数据的行  
<img width="1017" alt="image" src="https://user-images.githubusercontent.com/105503216/181672660-19c31228-87c2-4893-9a05-94756aad7767.png">  

#### 2.2.1 隐藏index

``` python
import pandas as pd
import numpy as np
exam_data = {'name': ['Anastasia', 'Dima', 'Katherine', 'James', 'Emily', 'Michael', 'Matthew', 'Laura', 'Kevin', 'Jonas'],
        'score': [12.5, 9, 16.5, np.nan, 9, 20, 14.5, np.nan, 8, 19],
        'attempts': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'qualify': ['yes', 'no', 'yes', 'no', 'no', 'yes', 'yes', 'no', 'no', 'yes']}
df = pd.DataFrame(exam_data)

df.reset_index(inplace=True)
df = df.to_string(index=False)  # 这个函数不能inplace
print(df)
```

<img width="358" alt="image" src="https://user-images.githubusercontent.com/105503216/193439189-ded87630-1dde-4cfa-a411-827e261202cc.png">  


### 2.3 查看行列数目
``` python
price_info.shape

(150, 5)
```

分开来查看行数和列数

``` python
rows = len(df.index)
columns = len(df.columns)
```

### 2.4 查看数据总体分布特征
<img width="1012" alt="image" src="https://user-images.githubusercontent.com/105503216/181668413-525127c7-eb03-40c4-a5f5-1cd17ed9b3e0.png">  

### 2.5 display a summary of the basic information
``` python
df.info()  # 这是有括号的
```
<img width="1007" alt="image" src="https://user-images.githubusercontent.com/105503216/181668249-6e6158c3-5d46-4577-be30-257430f4c49e.png">  


```

### 2.7 统计学值
#### 2.7.1 centers
求每一列的mean平均值和median中位数

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

#### 2.7.2 variations
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

#### 2.7.3 extreme points
包括最大值和最小值
``` python
print(data.select_dtypes(exclude='object').min())
print(dara.select_dtypes(exclude='object').max())
```

EXERCISE
``` python
ne = Nowcoder.loc[Nowcoder['Num_of_exercise']>10,'Num_of_exercise'] 
ns = Nowcoder.loc[Nowcoder['Num_of_exercise']>10,'Number_of_submissions']
max_c = (ne/ns).max()   # 计算结果的max 要把计算过程()  
print(round(max_c,3))
```

#### 2.7.4 几分位点

``` python
print(result.quantile([0.25, 0.50, 0.75]))    # 注意这里是0.50 此外 注意多个分位数在一起的写法 
```

#### 2.7.5 众数
.mode()

### 2.8 遍历行数据

``` python
import pandas as pd
import numpy as np
exam_data = [{'name':'Anastasia', 'score':12.5}, {'name':'Dima','score':9}, {'name':'Katherine','score':16.5}]
df = pd.DataFrame(exam_data)

print(df.iterrows())
print('\n')

for i,m in df.iterrows():
  print(i)
  print(m)
```

<img width="375" alt="image" src="https://user-images.githubusercontent.com/105503216/192964189-eb55a2e3-72e1-4232-8e39-35ed76a68d1a.png">   

Iterate over rows in a DataFrame  

``` python
import pandas as pd
import numpy as np
exam_data = [{'name':'Anastasia', 'score':12.5}, {'name':'Dima','score':9}, {'name':'Katherine','score':16.5}]
df = pd.DataFrame(exam_data)

for i,m in df.iterrows():
  print(m['name'], m['score'])
```
<img width="147" alt="image" src="https://user-images.githubusercontent.com/105503216/192964725-8a1a7682-f81f-4910-8894-f877a3363178.png">
