## Boolean Filtering in Pandas

### 🔹 Select rows where a column is **not None / NaN**
```python
df = df[df["current_price"].notna()]
```

---

### 🔹 Multiple Conditions: `|` (OR) and `&` (AND)

```python
# Intersection of two conditions (AND): True only if both are True
is_wife = (data_frame["gender"] == "Female") & (data_frame["married"])

# Result example:
# 0    False
# 1     True
# 2    False
# 3    False
# 4    False
# 5    False
# dtype: bool
```

```python
# Union of two conditions (OR): True if either condition is True
is_skillful = (data_frame["educ"] > 9) | (data_frame["exper"] > 3)

# Result example:
# 0     True
# 1     True
# 2    False
# 3     True
# 4     True
# 5     True
# dtype: bool
```
💡 **Tips**
- Always wrap each condition in parentheses `()` when using `&` or `|`.
- Avoid using Python’s `and` / `or` with pandas Series — they’ll raise an error.
---

### 🔹 “Not Equal To” Condition
```python
not_female = data_frame["gender"] != "Female"
```

or equivalently:
```python
not_female = ~(data_frame["gender"] == "Female")
```

---
## Select columns
### ["column_name"]

``` python
df = pd.DataFrame(exam_data , index=labels)
```
 
``` python
df = pd.DataFrame(exam_data , index=labels)
result = df[['name','attempts']]
```

``` python
# can't do slicing using label-based []
data_frame['wage':'married']          
cannot do slice indexing on RangeIndex with these indexers [wage] of type str
```

### 3.4.2 loc/iloc
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

--------------
## drop()
Remove a certain row / column on the original dataframe   
OR   
Get a new df without a certain row / column  

``` python
data_num
400 rows × 12 columns

x = data_num.drop(columns='Balance')     # get the data_num without the column balance


data_num.drop(columns='Balance', axis=1, inplace=True)   # Directly remove the column balance in data_num
```

切除对应的行 

``` python
import pandas as pd
import numpy as np
d = {'col1': [1, 4, 3, 4, 5], 'col2': [4, 5, 6, 7, 8], 'col3': [7, 8, 9, 0, 1]}
df = pd.DataFrame(d, index=['a','b','c','d','e'])

df.drop(index=['c','e'], axis=0, inplace=True)
print(df)
```

