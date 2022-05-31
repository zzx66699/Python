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
