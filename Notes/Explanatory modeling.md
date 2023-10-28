# Explanatory modeling
注重parameter
## Linear regression
**关于linear regression的定义**  
不用管variable不linear，但是parameter是linear就行了 下面的都是linear regression
![image](https://user-images.githubusercontent.com/105503216/172055899-c08cf177-e727-4f6f-82d0-4bbe82c35396.png)
  
**General expression of linear regression**
  ![image](https://user-images.githubusercontent.com/105503216/172052557-8736f48a-69a6-4628-9dfc-6637d4ac05bf.png)
  
![image](https://user-images.githubusercontent.com/105503216/172053084-3bc65d36-6587-4bd1-aec5-1b339439b2c4.png)
标准误差standard error  
这里可以理解为 是PRF所求的系数 和SRF所求的系数 之间的差距
  
  
![image](https://user-images.githubusercontent.com/105503216/172053098-951b7062-a3b1-489a-bf45-1796f6e82797.png)
  
![image](https://user-images.githubusercontent.com/105503216/172054524-7ce10fe6-5414-484b-ab45-9b5c891a89fa.png)


### The ordinary least squares (OLS) method 最小二乘法
为了保证残差的平方和最小 sum of squared residuals(SSR)
![image](https://user-images.githubusercontent.com/105503216/172053237-d696b6b0-12bb-45cd-ab0d-a57b9d9617c7.png)
#### simple regression 只有一个x值
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
#### multiple regression 多个x值
**可以用来做Ceteris paribus analysis其他条件不变分析**  
因为可以清晰地控制其他变量不变  
在simple regression中 一个x的改变 可能会影响到其他x的变化
``` python
model = smf.ols('sales ~ TV + radio + newspaper', data = data)       # 多个x值用+相连接
result = model.fit()
print(result.summary())
``` 
![image](https://user-images.githubusercontent.com/105503216/172055777-968a3b21-e59d-4b2a-98ab-28cdad8e45aa.png)
  
对于结果的每一列的拆分 进行单独输出
``` python
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
**对于dataframe中的某一列进行改变 并放入回归中**
``` python
wage['rooted_exper'] = np.sqrt(wage['exper'])                # 虽然wage['exper']输出的是series 但也可以用numpy包里的function来开方
model2 = smf.ols('wage ~ exper + rooted_exper', data = wage)
result2 = model2.fit()
print(result2.summary())

# 更简单的方法是直接在回归的时候改变形式
model2 = smf.ols('wage ~ exper + np.sqrt(exper)', data = wage)  # 注意np.sqrt后面直接是列明 因为用的data已经写在后面了
```
**关于categorical variables**
categorical information is captured by a binary variable or a dummy variable 会新建dummy variable  
以alphabetically顺序 在前面的当作base 其他是dummy
``` python
subset = condo.loc[(condo['district_code']==5) & (condo['area']<1500) & (condo['remaining_years']<=99)]
model = smf.ols('price ~ type + area', data=subset)   # type是categorical area是numerical
result = model.fit()
print(result.summary())  # 自动生成一列T.Resale 1就是resale
```
![image](https://user-images.githubusercontent.com/105503216/172107107-993a7b43-c2e4-45c0-a9b1-de0aafd33c16.png)
  
有时候虽然某一列是数字形式 但是不应该把它当作数字处理 比如街区号  
这时候需要把数字转化为categorical来处理  
``` python
model = smf.ols('price ~ C(district_code) + area', data=subset)  # 通过function C()来实现转化
result = model.fit()
print(result.summary())
```
**关于interaction term**
``` python
model = smf.ols('price ~ type*area', condo_subset)                # *是直接create3个terms
# 或者
model = smf.ols('price ~ type + area + type:area', condo_subset)  # 用:可以只创造交叉项

result = model.fit()
print(result.summary())
```
![image](https://user-images.githubusercontent.com/105503216/172127509-884e2ba2-f95a-41d8-b68d-7bc61468ff6f.png)
