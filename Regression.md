# Explanatory modeling
注重parameter
## Linear regression
General expression of linear regression
  ![image](https://user-images.githubusercontent.com/105503216/172052557-8736f48a-69a6-4628-9dfc-6637d4ac05bf.png)
![image](https://user-images.githubusercontent.com/105503216/172053084-3bc65d36-6587-4bd1-aec5-1b339439b2c4.png)
![image](https://user-images.githubusercontent.com/105503216/172053098-951b7062-a3b1-489a-bf45-1796f6e82797.png)
![image](https://user-images.githubusercontent.com/105503216/172054524-7ce10fe6-5414-484b-ab45-9b5c891a89fa.png)

### The ordinary least squares (OLS) method 最小二乘法
为了保证残差的平方和最小 sum of squared residuals(SSR)
![image](https://user-images.githubusercontent.com/105503216/172053237-d696b6b0-12bb-45cd-ab0d-a57b9d9617c7.png)
**simple regression 只有一个x值**
``` python
import statsmodels.formula.api as smf   # 调用这个module模块
data = pd.read_csv('xydata.csv')
  	y	       x
0	-0.030648	0.015198
1	1.981215	0.098450
2	1.292085	0.164420
3	2.835588	0.268800
4	3.613336	0.310132

model = smf.ols('y ~ x', data=data)      # smf包中调用ols这个算法 ' ~ '包含了x和y变量 data表明用的哪一个数据库
result = model.fit()                     # 开始拟合 把拟合的结果赋值给result
print(result.summary())                  # 导出result 注意一定要加.summary()

print(result)                            # 如果不加.summary()
<statsmodels.regression.linear_model.RegressionResultsWrapper object at 0x000001550CB1BB80>
```
**multiple regression 多个x值**
可以用来做Ceteris paribus analysis其他条件不变分析 因为可以清晰地控制其他变量不变  
在simple regression中 一个x的改变 可能会影响到其他x的变化
``` python
model = smf.ols('sales ~ TV + radio + newspaper', data = data)       # 多个x值用+相连接
result = model.fit()
print(result.summary())

# 只要系数
print(result.params)       # 可以把所有x地paramster求出来 注意这里是result的固有属性 就像dataframe的index 所以不加()
Intercept    2.938889
TV           0.045765
radio        0.188530
newspaper   -0.001037
dtype: float64

# 系数的标准误差 standard error
# 标准误差(Standard error),也称均方根误差(Root mean squared error) 是每一个观察值和每一个真实值之间的差距
print(result.bse)
Intercept    0.311908
TV           0.001395
radio        0.008611
newspaper    0.005871
dtype: float64
```
# Predictive modeling
