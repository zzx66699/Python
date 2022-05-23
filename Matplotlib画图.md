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

