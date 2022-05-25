# Pandas
``` python
import pandas as pd                 # Import the pandas package for data frames
```

# pandas.Series
## 构建转变
一维的索引数组 one-dimensional array of indexed data  
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

educ = pd.Series(np.array([[11.0, 12.0], [11.0, 8.0]]))     #不可以用2维的
ValueError: Data must be 1-dimensional
```

## 查看基础特征 value和index
``` python
wage.values        
--- array([3.1 , 3.24, 3.  , 6.  , 5.3 , 8.75])         # The values are formated as a NumPy array

wage.index          
--- RangeIndex(start=0, stop=6, step=1)                 # The data type of index is RangeIndex by default
```


