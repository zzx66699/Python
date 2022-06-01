# matplotlib
```
import matplotlib.pyplot as plt
```

## plot 线图  
[详细用法](https://matplotlib.org/2.1.1/api/_as_gen/matplotlib.pyplot.plot.html)
```python
plt.plot(x, y, 
         linewidth=3, 
         linestyle=’:’, 
         color=’r’, 
         marker=’o’, 
         markerfacecolor='blue', 
         markersize=12)
```  
| id   |***linestyle=***|***marker=***|***color=***|
|------------|------------|------------|---------------|
| 1          | ’:’ dot 点线     | ‘o’ 圈        |  ’b’ blue   |
| 2          | ‘-.’ 点+线    | ‘v’ 三角       |  ‘g’ green   |
| 3          | ‘--’ dashed 虚线     |      |  ‘r’ red    |
| 4          |      |        |  ‘m’ magenta 洋红    |
| 5          |      |       |  ‘w’ white   |
| 6          |      |       |  ‘y’ yellow    |
| 7          |      |       |  ‘k’ black  |  

## scartter 点图
``` python
step = 1
x = np.arange(0, 5+step, step)   # An array 0, 1, 2, 3, 4, 5
y = np.exp(x)
z = np.sqrt(x)
plt.scatter(x, y, 
            size=z*50,           # Dot sizes are specified by z*50
            alpha=0.2,           # Opacity is set to be 20%
            c=’b’)   
plt.xlabel('x')                  # Label for x data
plt.ylabel('y')                  # Label for y data
plt.show()               
```
## bar 柱图
```python
step = 1
x = np.arange(0, 5+step, step)
y = np.exp(x)
plt.bar(x, y,
        width=0.6,
        color='b',
        alpha=0.3)
plt.xlabel('x')
plt.ylabel('y')
plt.show()
```
## 混合问题
### x值为categorical variable且两个变量
``` python
distr_dict = {'weather': ['Sunny', 'Cloudy', 'Raining', 'Thunderstorm', 'Haze'],
              'probs': [0.315, 0.226, 0.289, 0.087, 0.083],
              'paper1': [560, 530, 389, 202, 278],
              'paper2': [533, 486, 386, 234, 263]}
weather = distr_dict['weather']
paper1 = distr_dict['paper1']
paper2 = distr_dict['paper2']

x = np.arange(len(weather))                 # An array 0, 1, 2, ..., 4
width = 0.3                                 # Width of the bar

plt.figure(figsize=(7.5, 4))
plt.bar(x-0.5*width, paper1,                # x和y的值，这里都是用numerical variable表示的，因为要通过x的大小手动错开x的位置
        color='r', width=width, alpha=0.6,
        label='paper1')                     # Bar chart for paper1
plt.bar(x+0.5*width, paper2, 
        color='b', width=width, alpha=0.6,
        label='paper2')                     # Bar chart for paper2

plt.legend(fontsize=13)
plt.xticks(x, weather, fontsize=13)         # Change the item name
plt.ylabel('Demand', fontsize=13)
plt.show()
```
![image](https://user-images.githubusercontent.com/105503216/169785225-230303f2-768b-43a6-93e6-c3f8c69a50e1.png)
### 在同一张图中有多种pattern
普通列举出来即可  
``` python
import math
step = 0.01
t = np.arange(0, 2*math.pi+step, step)
x = -30*np.sin(t) + 8*np.sin(2*t) - 10*np.sin(3*t) - 60*np.cos(t)          # 画大象的轮廓
y = -50*np.sin(t) - 18*np.sin(2*t) - 12*np.cos(3*t) + 14*np.cos(5*t)
plt.plot(x,y)
plt.scatter(x=20, y=20)                                                    # 画眼睛
plt.show()
```
![image](https://user-images.githubusercontent.com/105503216/169791515-0ef721a2-f9f5-424b-9cd8-5adb564ba0f5.png)


## Configured functions  
``` python
step = 1
x = np.arange(0, 10+step, step)
y = np.exp(x/2)
```
### plt.figure(figsize=(,)) 
`plt.figure(figsize=(7,4))`  
### plt.title('标题名', fontsize=) 
`plt.title('Bar graph', fontsize=15)`  
### plt.xlabel('x轴名', fontsize=) & plt.ylabel('y轴名', fontsize=)
'plt.xlabel('y', fontsize=15)'
### plt.legend(fontsize=)
``` python
plt.plot(x, y, linewidth=3, linestyle=':', marker='o', color='r', 
         label=‘line1’)         # Label of the line plot1
plt.plot(x, y, linewidth=2, linestyle='-.', marker='v', color='b',
         label='line2')         # Label of the line plot2
plt.legend(fontsize=13)         # Display the legend
plt.show()
```
### plt.xticks(原来数值，现在的项名称，fontsize=)  x轴中每一项的项目名称
``` python
distr_dict = {'weather': ['Sunny', 'Cloudy', 'Raining', 'Thunderstorm', 'Haze'],
              'probs': [0.315, 0.226, 0.289, 0.087, 0.083],
              'paper1': [560, 530, 389, 202, 278],
              'paper2': [533, 486, 386, 234, 263]}
weather = distr_dict['weather']
plt.xticks(x, weather, fontsize=13)        # 注意要先写原来的值
```

# visualize在数据分析中的应用
## boxplot
display the distribution of data based on a five-value summary:  
The "minimum" value: Q1 -1.5*IQR, or the actual minimum value  
The first quartile: Q1  
The second quartile, or the median: Q2,  
The third quartile: Q3  
The “maximum” value: Q3 + 1.5*IQR, or the actual maximum value  
![image](https://user-images.githubusercontent.com/105503216/171328629-403460a9-2ed8-43b0-8e4c-b6c835de7933.png)

``` python
plt.boxplot(data['wage'],                  # Create a box plot of the wages
            vert=False)                    # Make the plot horizontal  

plt.xlabel('Hourly wage', fontsize=14)
plt.show()
```
## histogram
``` python
plt.hist(data['wage'],                    # Histogram of wages with 20 bins 
         bins=20,                         # bins可以现解为表示把数据分成多少段，当设置的很大时，说明分的很细，每个列就会细
         color='b', alpha=0.7)            # Color is blue, opacity is 0.7 
```
![image](https://user-images.githubusercontent.com/105503216/171331470-4a6a1b6c-a630-4114-bad2-14cdef62b5b2.png)
``` python
plt.hist(data['wage'], bins=10, color='b', alpha=0.4)
```
![image](https://user-images.githubusercontent.com/105503216/171331572-ce18bf23-a8d8-44b4-80ce-7267492bb30d.png)
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
