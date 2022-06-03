# Pandas
``` python
import pandas as pd                 # Import the pandas package for data frames
```

# pandas.Series
一维的索引数组 one-dimensional array of indexed data  
## 构建转变
可以从其它的一维数组创建而来，包括list和一维numpy.array  
``` python
wage = pd.Series([3.10, 3.24, 3.00])                        # 从list转变
wage ---
0    3.10
1    3.24
2    3.00
dtype: float64

wage = pd.Series([[3.10], [3.24, 3.00], 6.00])              # list可以包含很多子list
wage ---
0          [3.1]
1    [3.24, 3.0]
2            6.0
dtype: object

educ = pd.Series(np.array([11.0, 12.0, 11.0]))              # 从一维的np.array转变
educ ---
0    11.0
1    12.0
2    11.0
dtype: float64

educ = pd.Series(np.array([[11.0, 12.0], [11.0, 8.0]]))     # 不可以用2维的
ValueError: Data must be 1-dimensional

index = ['Mary', 'Ann', 'John', 'David', 'Frank', 'Ben']
exper = pd.Series([2.0, 22.0, 2.0, 44.0, 7.0, 9.0], 
               index = index)                               # 可以构建时指定index
```

## 查看基础特征 value和index
``` python
wage.values        
--- array([3.1 , 3.24, 3.  , 6.  , 5.3 , 8.75])         # The values are formated as a NumPy array

wage.index          
--- RangeIndex(start=0, stop=6, step=1)                 # The data type of index is RangeIndex by default
```

## 切片
pd.Series有两种方法： 
1. label based indice 基于label标签的index/indice索引 series.loc[] 包含  
2. integer-position based indice 基于position位置的索引 series.iloc[] 不包含  
``` python
index = ['Mary', 'Ann', 'John', 'David', 'Frank', 'Ben']
exper = pd.Series([2.0, 22.0, 2.0, 44.0, 7.0, 9.0], index = index) 

print(exper.loc['Mary'])                            # 注意如果是输出单个元素，此时输出结果是string
--- 2.0

print(exper.loc[['Mary']])                          # 两个括号才是Series
--- 
Mary    2.0
dtype: float64

print(exper.loc['John':])                           # 和字典不同的是，可以用切片语法检索多个项目
--- 
John      2.0
David    44.0
Frank     7.0
Ben       9.0
dtype: float64 

print(exper.loc[:'Ann'], '\n')                      # 和position不同，用label定位包含有最后一项
---
Mary     2.0
Ann     22.0
dtype: float64 

print(exper.loc[['John', 'Ann']])
---
John     2.0
Ann     22.0
dtype: float64

print(exper.iloc[:2])                              # 没有position=2的那一项
---
Mary     2.0
Ann     22.0
dtype: float64 

# 注意
print(exper.iloc[::2])
print(exper.loc[::2])                             # 这两个是一模一样的，都是从头到尾，且包含尾巴，间隔2
```
对于没有特别指定index的pd.Series index名字就是position   
需要注意.loc[:x]和.iloc[:x]的最后一项 
``` python
print(wage.loc[:2], '\n')                                      # Index 2 is included
0    3.10
1    3.24
2    3.00
dtype: float64 
print(wage.iloc[:2])                                           # Index 2 is excluded

0    3.10
1    3.24
dtype: float64
```

其他时候loc和iloc是一样的
``` python
wage = pd.Series([3.10, 3.24, 3.00, 6.00, 5.30, 8.75])
print(wage.loc[1])   --- 3.24                                 
print(wage.loc[[0,3,1]])  ---                                  
0    3.10
3    6.00
1    3.24
dtype: float64
print(wage.loc[-2:])                                           # Print the last two items in the series
4    5.30
5    8.75
dtype: float64
```

# pandas.DataFrame
二维的索引数组 one-dimensional array of indexed data  
## 构建转变
可以从其它的二维数组创建而来，包括dictionary和二维numpy.array  
``` python
data_dict = {'wage': [3.10, 3.24, 3.00, 6.00, 5.30, 8.75],                      # 字典里的key变成了每一列的index，每一行的index自动生成
             'educ': [11.0, 12.0, 11.0, 8.0, 12.0, 16.0],
             'exper': [2.0, 22.0, 2.0, 44.0, 7.0, 9.0],
             'gender': ['Female', 'Female', 'Male', 'Male', 'Male', 'Male'],
             'married': [False, True, False, True, True, True]}

data_frame = pd.DataFrame(data_dict)    # Create a DataFrame object
data_frame                              # Display the DataFrame


  wage	educ	exper	gender	married
0	3.10	11.0	2.0	Female	False
1	3.24	12.0	22.0	Female	True
2	3.00	11.0	2.0	Male	False
3	6.00	8.0	44.0	Male	True
4	5.30	12.0	7.0	Male	True
5	8.75	16.0	9.0	Male	True

# 也可以在构建时设立index
index = ['Mary', 'Ann', 'John', 'David', 'Frank', 'Ben']
data_frame_new = pd.DataFrame(data_dict, index=index)  

      wage	educ	exper	gender	married
Mary	3.10	11.0	2.0	Female	False
Ann	3.24	12.0	22.0	Female	True
John	3.00	11.0	2.0	Male	False
David	6.00	8.0	44.0	Male	True
Frank	5.30	12.0	7.0	Male	True
Ben	8.75	16.0	9.0	Male	True
```

## 查看基础特征 行名和列名column和index
``` python
price_info.columns       
Index(['count', 'median', 'max', 'min'], dtype='object')    

price_info.index   
Index(['CCR', 'OCR', 'RCR'], dtype='object', name='segment')
```

## 切片
### 只切列
**方法一：直接[]，不用loc/iloc**  
此方法只能单独提取，不可以slicing
``` python
wage = data_frame['wage']                   # Access one column 记住要加''
wage ---
0    3.10
1    3.24
2    3.00
3    6.00
4    5.30
5    8.75
Name: wage, dtype: float64

skills = data_frame[['educ', 'exper']]     # Access two columns
skills ---
  educ	exper
0	11.0	2.0
1	12.0	22.0
2	11.0	2.0
3	8.0	44.0
4	12.0	7.0
5	16.0	9.0

data_frame['wage':'married']                 # 不可以直接用label-based切片
cannot do slice indexing on RangeIndex with these indexers [wage] of type str
```

**方法二：用loc/iloc**  
提取单独几列的时候会比较麻烦 但好在可以切片   
只要切列了 就必须加上,:
``` python
data_frame.loc[:,['wage','educ']]            # 几行 单独几行记得要加括号噢
wage	educ
0	3.10	11.0
1	3.24	12.0
2	3.00	11.0
3	6.00	8.0
4	5.30	12.0
5	8.75	16.0

data_frame.loc[:,'wage':'married']
  wage	educ	exper	gender	married          # 如果想要用label-based切片表达 必须在前面加上行 也就是用,划分 取全部行时要用:
0	3.10	11.0	2.0	Female	False
1	3.24	12.0	22.0	Female	True
2	3.00	11.0	2.0	Male	False
3	6.00	8.0	44.0	Male	True
4	5.30	12.0	7.0	Male	True
5	8.75	16.0	9.0	Male	True

data_frame.iloc[:, :5]                       # position也同理
  wage	educ	exper	gender	married
0	3.10	11.0	2.0	Female	False
1	3.24	12.0	22.0	Female	True
2	3.00	11.0	2.0	Male	False
3	6.00	8.0	44.0	Male	True
4	5.30	12.0	7.0	Male	True
5	8.75	16.0	9.0	Male	True

```

### 只切行
``` python
# 取前3行 用切片方法可以不加列的,:
data_frame.loc[:2]                                           # 用loc表达
data_frame.iloc[0:3]                                         # 用iloc表达
  wage	educ	exper	gender	married
0	3.10	11.0	2.0	Female	False
1	3.24	12.0	22.0	Female	True
2	3.00	11.0	2.0	Male	False
# 为方便记忆 建议都写完整比较好
```

### 有行有列
``` python
# A subset containing the 2nd and the 3rd  columns, and the 2nd and the 3rd rows

data_new_subset = data_frame_new.loc[1:2, 'educ':'exper']   # 用loc表达 注意切片不用加上[]，只有单独提取几列的时候才需要加[]
data_new_subset = data_frame_new.iloc[1:3, 1:3]             # 用iloc表达 注意这里的列也不要用label 而要用position
---
  educ	exper
1	12.0	22.0
2	11.0	2.0
```

### 只筛选一列/一行的情况
用label_based筛选时 bracket数量决定了输出的是series还是dataframe
``` python
# 行
data_frame_new.loc['John']                 # 不加bracket输出的是series
wage         3.0
educ        11.0
exper        2.0
gender      Male
married    False
Name: John, dtype: object

data_frame_new.loc[['John']]               # 加了bracket输出的是dataframe
      wage	educ	exper	gender	married
John	3.0	11.0	2.0	Male	False

# 列
data_frame_new.loc[:,'wage']               # 不加bracket输出的是series
Mary     3.10
Ann      3.24
John     3.00
David    6.00
Frank    5.30
Ben      8.75
Name: wage, dtype: float64

data_frame.loc[:,['wage']]                 # 加了bracket输出的是dataframe 
  wage
0	3.10
1	3.24
2	3.00
3	6.00
4	5.30
5	8.75
``` 
用position筛选时 是单值还是slicing式决定了输出的是series还是dataframe
``` python
# 行
data_frame_new.iloc[1]
wage         3.24
educ         12.0
exper        22.0
gender     Female
married      True
Name: Ann, dtype: object

data_frame_new.iloc[1:2]
wage	educ	exper	gender	married
Ann	3.24	12.0	22.0	Female	True

# 列
data_frame_new.iloc[:,0]
Mary     3.10
Ann      3.24
John     3.00
David    6.00
Frank    5.30
Ben      8.75
Name: wage, dtype: float64

data_frame_new.iloc[:,0:1]
      wage
Mary	3.10
Ann  	3.24
John	3.00
David	6.00
Frank	5.30
Ben	  8.75
```

## 替换modify&增加
直接选取 赋值
``` python
data_frame
  wage	educ	exper	gender	married
0	3.10	11.0	2.0	Female	False
1	3.24	12.0	22.0	Female	True
2	3.00	11.0	2.0	Male	False
3	6.00	8.0	44.0	Male	True
4	5.30	12.0	7.0	Male	True
5	8.75	16.0	9.0	Male	True

data_frame.loc[2,'educ'] = 9
data_frame
  wage	educ	exper	gender	married
0	3.10	11.0	2.0	Female	False
1	3.24	12.0	22.0	Female	True
2	3.00	9.0	2.0	Male	False
3	6.00	8.0	44.0	Male	True
4	5.30	12.0	7.0	Male	True
5	8.75	16.0	9.0	Male	True

data_frame.loc[:,'happiness'] = 'none'
data_frame
  wage	educ	exper	gender	married	happiness
0	3.10	11.0	2.0	Female	False	none
1	3.24	12.0	22.0	Female	True	none
2	3.00	9.0	2.0	Male	False	none
3	6.00	8.0	44.0	Male	True	none
4	5.30	12.0	7.0	Male	True	none
5	8.75	16.0	9.0	Male	True	none
```

## boolean pandas.Series
### 单一判断
``` python
is_female = data_frame['gender'] == 'Female'      
is_female
0     True
1     True
2    False
3    False
4    False
5    False
Name: gender, dtype: bool

is_high_wage = data_frame['wage'] > 4
is_high_wage
0    False
1    False
2    False
3     True
4     True
5     True
Name: wage, dtype: bool
```
### 多条件判断 | 和 &
``` python
# Intersection of two conditions: True if both of them are True
is_wife = (data_frame['gender']=='Female') & (data_frame['married'])            # 注意一定要加()把独立的判断条件分开来
0    False
1     True
2    False
3    False
4    False
5    False
dtype: bool

# Union of two conditions: True if either of them is True
is_skillful = (data_frame['educ']>9) | (data_frame['exper']>3)
0     True
1     True
2    False
3     True
4     True
5     True
dtype: bool

not_female = data_frame['gender'] != 'Female'                         # not equal to 
not_female
0    False
1    False
2     True
3     True
4     True
5     True
Name: gender, dtype: bool
```
### 和loc一起使用 筛选目标列
boolean pandas.Series最重要的用法就是去Select rows indicated by True
``` python
is_female = data_frame['gender'] == 'Female'    # A boolean Series 
female = data_frame.loc[is_female]              # Select rows indicated by True
female
```
同理 筛选出的也可以赋值  
The value for married male is changed to the string 'Husband';  
The value for unmarried male or female is changed to the string 'Single'  
``` python
data_frame.loc[(data_frame['married']==True) & (data_frame['gender']=='Male'),'married']='Husband'
data_frame.loc[data_frame['married']==False, 'married']='Single'
data_frame
```

# Descriptive analytics 
## Read data from files
``` python
import pandas as pd
data = pd.read_csv('wage.csv')  # Read data from a file "wage.csv"
data.head(6)                    # Return the first six rows of data
```
## 获得总体数据的基础特征
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
### extreme points
包括最大值和最小值
``` python
print(data.select_dtypes(exclude='object').min())
print(dara.select_dtypes(exclude='object').max())
```
## 获得每一列数据的特征
使用.unique()获取不重复的值 结果是array表示
``` python
condo['segment'].unique()
array(['CCR', 'RCR', 'OCR'], dtype=object)
```
也可以把用于总数据的方法单独用在一列上
``` python
print(data['wage'].min())
print(data.loc[data['gender']=='male','wage'].var())
```
求某个categorical variable各value的占比
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
## .describe()
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

## correlation
``` python
data.corr()
         wage	educ	 exper	  married
wage	1.000000	0.405903	 0.112903  0.228817
educ	0.405903	1.000000	 -0.299542 0.068881
exper	0.112903	-0.299542 1.000000  0.316984
married	0.228817	0.068881	 0.316984  1.000000
```

## 处理missing value
missing value在python中表示为`NaN`
``` python
pd.options.display.max_columns = 8         # 最多展示8列 但依然是读取全部
gdp = pd.read_csv('gdp.csv')
gdp
```
![image](https://user-images.githubusercontent.com/105503216/171332892-a63a59e2-b2d8-41e0-9474-9d8d17275176.png)
### detect 甄别 isnull()
``` python
gdp.loc[:,'1960':'1961'].isnull()     # 返回的是boolean
         1960	1961
0	True	True
1	False	False
2	True	True
3	True	True
4	True	True
...	...	...
210	True	True
211	True	True
212	False	False
213	False	False
214	False	False
```
### select 筛选 notnull()
``` python
gdp_1960 = gdp.loc[gdp['1960'].notnull(),       # Select rows
                   ['Country Name', '1960']]    # Select columns
gdp_1960

         Country Name	1960
1	Afghanistan	5.377778e+08
10	Australia	1.857767e+10
11	Austria	6.592694e+09
13	Burundi	1.960000e+08
14	Belgium	1.165872e+10
...	...	...
204	St. Vincent and the Grenadines	1.306656e+07
205	Venezuela, RB	7.779091e+09
212	South Africa	7.575397e+09
213	Zambia	7.130000e+08
214	Zimbabwe	1.052990e+09
```
### drop 去除 dropna()
``` python
gdp_subset = gdp.loc[:6, '1981':'1986']
gdp_subset
         1981	1982	1983	1984	1985	1986
0	NaN	NaN	NaN	NaN	NaN	4.054634e+08
1	3.478788e+09	NaN	NaN	NaN	NaN	NaN
2	5.550483e+09	5.550483e+09	5.784342e+09	6.131475e+09	7.553560e+09	7.072063e+09
3	NaN	NaN	NaN	1.857338e+09	1.897050e+09	2.097326e+09
4	3.889587e+08	3.758960e+08	3.278618e+08	3.300707e+08	3.467380e+08	4.820006e+08
5	4.933342e+10	4.662272e+10	4.280332e+10	4.180795e+10	4.060365e+10	3.394361e+10
6	7.867684e+10	8.430749e+10	1.039791e+11	7.909200e+10	8.841667e+10	1.109344e+11

temp = gdp_subset.dropna()      # 把所有带有NaN的行都删去了
temp
         1981	         1982	         1983	         1984	         1985	         1986
2	5.550483e+09	5.550483e+09	5.784342e+09	6.131475e+09	7.553560e+09	7.072063e+09
4	3.889587e+08	3.758960e+08	3.278618e+08	3.300707e+08	3.467380e+08	4.820006e+08
5	4.933342e+10	4.662272e+10	4.280332e+10	4.180795e+10	4.060365e+10	3.394361e+10
6	7.867684e+10	8.430749e+10	1.039791e+11	7.909200e+10	8.841667e+10	1.109344e+11

# 需要注意的是原来的data并没有改变
gdp_subset      # The original data frame remains unchanged
         1981	1982	1983	1984	1985	1986
0	NaN	NaN	NaN	NaN	NaN	4.054634e+08
1	3.478788e+09	NaN	NaN	NaN	NaN	NaN
2	5.550483e+09	5.550483e+09	5.784342e+09	6.131475e+09	7.553560e+09	7.072063e+09
3	NaN	NaN	NaN	1.857338e+09	1.897050e+09	2.097326e+09
4	3.889587e+08	3.758960e+08	3.278618e+08	3.300707e+08	3.467380e+08	4.820006e+08
5	4.933342e+10	4.662272e+10	4.280332e+10	4.180795e+10	4.060365e+10	3.394361e+10
6	7.867684e+10	8.430749e+10	1.039791e+11	7.909200e+10	8.841667e+10	1.109344e+11

# 如果要直接在原data上面改变 可以加上inplace=True
temp = gdp_subset.dropna(inplace=True)
print(temp)     # The output of the dropna method is None 此时返回不出值
None

gdp_subset      # The original data frame is overwritten 但原来的改变了
         1981	         1982	         1983	         1984	         1985	         1986
2	5.550483e+09	5.550483e+09	5.784342e+09	6.131475e+09	7.553560e+09	7.072063e+09
4	3.889587e+08	3.758960e+08	3.278618e+08	3.300707e+08	3.467380e+08	4.820006e+08
5	4.933342e+10	4.662272e+10	4.280332e+10	4.180795e+10	4.060365e+10	3.394361e+10
6	7.867684e+10	8.430749e+10	1.039791e+11	7.909200e+10	8.841667e+10	1.109344e+11
```
### replace 替换 fillna()
``` python
temp = gdp_subset.fillna('Unknown')      # Fill all NaN items with 'Unknown'
temp                                     # 同样这里的原序列并没有改变
           1981	1982	1983	1984	1985	1986
0	Unknown	Unknown	Unknown	Unknown	Unknown	405463417.11746
1	3478787909.09091	Unknown	Unknown	Unknown	Unknown	Unknown
2	5550483035.90815	5550483035.90815	5784341596.36339	6131475065.23832	7553560459.104279	7072063345.44786
3	Unknown	Unknown	Unknown	1857338011.85488	1897050133.42015	2097326250.0
4	388958731.302938	375895956.383462	327861832.946636	330070689.298282	346737964.774951	482000594.03588
5	49333424135.113098	46622718605.284698	42803323345.137604	41807954235.903	40603650231.544502	33943612094.7971
6	78676842366.421295	84307486836.723999	103979106777.910995	79092001998.031998	88416668900.259598	110934442762.694

gdp_subset.fillna(0, inplace=True)  # 这样原data才会改变
```

## 按分组将多列同时进行多种操作
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
## 关于时间的处理方法
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
## 矢量化字符串操作Vectorized string operations
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
condo['level_to'] = condo['level'].str[-2:].astype(int)     # astype()可以改变type
```
## 按照某一列重新排序 .sort_values()
Considering Singapore condos in the district 5, show the monthly trends of  
1) the average unit prices; and 2) the number of transactions, in recent years.
``` python
subset = condo.loc[condo['district_code']==5]
fun = ['mean','count']
o = subset.groupby('time')['unit_price'].agg(fun)
o.reset_index(inplace=True)
o.head(5)                                # 此时是按照alphabetical的顺序排序的 我们希望按照时间顺序
	date	mean	count	time
0	Apr-17	1162.106195	113	2017-04-01
1	Apr-18	1151.673913	46	2018-04-01
2	Apr-19	1197.903226	31	2019-04-01
3	Aug-17	1174.113924	79	2017-08-01
4	Aug-18	1244.913043	23	2018-08-01

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
## 数据透视表 Pivot table 
