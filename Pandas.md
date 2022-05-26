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
``` python
wage = data_frame['wage']              # Access one column
wage ---
0    3.10
1    3.24
2    3.00
3    6.00
4    5.30
5    8.75
Name: wage, dtype: float64

wage = data_frame['']
```
