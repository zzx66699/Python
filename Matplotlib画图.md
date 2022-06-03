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
### 想要看两个变量之间的关系 并且看分布
通过alpha=0.x可以看出分布
``` python
CCR = data_subset.loc[data_subset['segment']=='CCR']
RCR = data_subset.loc[data_subset['segment']=='RCR']
OCR = data_subset.loc[data_subset['segment']=='OCR']
plt.scatter(CCR['area'], CCR['price'], alpha=0.3, label='CCR')
plt.scatter(RCR['area'], RCR['price'], alpha=0.3, label='RCR')
plt.scatter(OCR['area'], OCR['price'], alpha=0.3, label='OCR')
plt.xlabel('area')
plt.ylabel('price')
plt.legend()
plt.show()
```
![image](https://user-images.githubusercontent.com/105503216/171367782-30bdbb45-9579-4b35-966f-5279cabd48f9.png)

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
### x轴是categorical variable 每个variable对应一个y轴的值
``` python
resale_price = data.loc[data['type']=='Resale','price'].mean()            # 结果是一个数字
newsale_price = data.loc[data['type']=='New Sale','price'].mean()         # 结果是一个数字
plt.bar(['Resale','New sale'],                                            # 直接写xticks的名字 要用括号括起来
        [resale_price,newsale_price])                                     # 对应的y轴的值 这样x和y轴都是值 所以可以画出图
```
![image](https://user-images.githubusercontent.com/105503216/171355725-42958dbd-1b45-4bd1-b0fc-30aac603cd1a.png)
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

## 在同一张图中有多种pattern
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

两个量的量纲差别很大 无法用同一种图案表达 变成一个是plot一个是scatter
``` python
o.head(5)  
         date	mean	         count	time
0	Nov-16	1218.390244	41	2016-11-01
1	Dec-16	1141.553191	47	2016-12-01
2	Jan-17	1241.507937	63	2017-01-01
3	Feb-17	1237.646739	184	2017-02-01
4	Mar-17	1219.400000	160	2017-03-01

plt.figure(figsize=(12,5))
plt.plot(o['date'], o['mean'], color='r', label='mean')
# 为了让两个scale不同的放在同一张表上 可以采用scatter的大小表示数量的多少 还可以放大几倍来达到效果
plt.scatter(o['date'], o['mean'], s=o['count']*3, color='r', alpha=0.5, label='count') # 此时注意y轴还是应该用mean的数值 来保证在那一个点上
plt.xlabel('time')
plt.ylabel('unit price')
plt.xticks(rotation=90)  # 把tick旋转
plt.legend()
plt.grid()              # 加点各自背景
plt.show()
```
![image](https://user-images.githubusercontent.com/105503216/171904544-0f129a15-978b-4ad6-bd4e-d6fce119de91.png)

## Configured functions  
``` python
step = 1
x = np.arange(0, 10+step, step)
y = np.exp(x/2)
```
### 图像大小
`plt.figure(figsize=(7,4))`  
### 标题名
`plt.title('Bar graph', fontsize=15)`  
### xy轴名
'plt.xlabel('y', fontsize=15)'
### 显示出label名
`plt.legend(fontsize=15)`
``` python
plt.plot(x, y, linewidth=3, linestyle=':', marker='o', color='r', 
         label=‘line1’)         # Label of the line plot1
plt.plot(x, y, linewidth=2, linestyle='-.', marker='v', color='b',
         label='line2')         # Label of the line plot2
plt.legend(fontsize=13)         # Display the legend
plt.show()
```
### x轴中每一项的项目名称 plt.xticks(原来数值，现在的项名称，fontsize=)  
``` python
distr_dict = {'weather': ['Sunny', 'Cloudy', 'Raining', 'Thunderstorm', 'Haze'],
              'probs': [0.315, 0.226, 0.289, 0.087, 0.083],
              'paper1': [560, 530, 389, 202, 278],
              'paper2': [533, 486, 386, 234, 263]}
weather = distr_dict['weather']
plt.xticks(x, weather, fontsize=13)        # 注意要先写原来的值
```
### 旋转
`plt.xticks(rotation=90)`
### 网格背景 
`plt.grid()`

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
### 两个变量放在同一张histogram图中
要注意统一每一个柱子的粗细 这样才可以比较
``` python
resale = data.loc[data['type']=='Resale','price']
newsale = data.loc[data['type']=='New Sale','price']
plt.hist(resale, bins=15,alpha=0.7, label='resale')
plt.hist(newsale, bins=15,alpha=0.7, label='newsale')
plt.legend()
plt.show()                # 可以看出来每个value的粗细是不一样的 应该统一每个柱形的粗细
```
![image](https://user-images.githubusercontent.com/105503216/171360704-3cb1e83a-2510-436f-9cbf-a97d155b3fee.png)
``` python
# 首先找到总范围
data['price'].describe()
count    3.216800e+04
mean     1.640373e+06
std      1.432406e+06
min      4.880000e+05
25%      9.800000e+05
50%      1.300000e+06
75%      1.774850e+06
max      5.200000e+07
Name: price, dtype: float64

# 把bins的上下限定在这个范围之内
#same value for the bins
bins = np.arange(5e5, 5e6, 1e5)                                        # bins控制的是图例中横轴的起点 结束和间隔  5*10^5 这里的间隔越大 每个柱子越粗
plt.hist(data_resale['price'], bins = bins, alpha=0.4, label='Resale') #要same value for the bins
plt.hist(data_new['price'], bins=bins, alpha=0.4, label='New Sale')
plt.legend()
plt.show()
```
![image](https://user-images.githubusercontent.com/105503216/171362003-9f4d175d-eb5b-4487-8f55-67cf25d640bf.png)
