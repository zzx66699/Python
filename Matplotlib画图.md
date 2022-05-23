# matplotlib
```
import matplotlib.pyplot as plt
```

## plot 线图  
```
plt.plot(x, y) 
```
**改变attributes** [详细用法](https://matplotlib.org/2.1.1/api/_as_gen/matplotlib.pyplot.plot.html)
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
```
plt.scatter(x,y)
```
**改变attributes**  
``` python
step = 1
x = np.arange(0, 5+step, step)   # An array 0, 1, 2, 3, 4, 5
y = np.exp(x)
z = np.sqrt(x)
plt.scatter(size=z*50,           # Dot sizes are specified by z*50
            alpha=0.2,           # Opacity is set to be 20%
            c=’b’)   
plt.xlabel('x')                  # Label for x data
plt.ylabel('y')                  # Label for y data
plt.show()               
```
![image](https://user-images.githubusercontent.com/105503216/169762287-9d30e37d-02ba-4103-bce2-e57a136c361f.png)


## bar 柱图
```
plt.bar(x, y)
```


plt.xlabel('x')   # 改变x和y的label
plt.show()
