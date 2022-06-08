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

# Predictive modeling
![image](https://user-images.githubusercontent.com/105503216/172146515-43a4ace0-aa5c-4951-acb8-8d04f0a7c23d.png)
  **核心：scikit_learn这个包在机器学习中经常用到 在后面加上不同的function采用不同的回归方法**  
注意在写的时候 这个包是sklearn.  

## 普通步骤
### 建立多项式（这一步是因为本数据集x太少了 正常数据集可以掠过这一步）
``` python
from sklearn.preprocessing import PolynomialFeatures  # 这个function用来建立从1到k的多项式
data = pd.read_csv('polydata.csv')
data.head(5)
  y	        x
0	4.903104	0.082443
1	2.555793	0.437323
2	9.459503	0.943338
3	10.881539	0.600545
4	12.071988	0.979821

k = 4
poly = PolynomialFeatures(k, include_bias=False)  # k是建立从1到k的多项式 
poly                                              # 现在是PolynomialFeatures的形式 还没有具体数值 只知道要从1到k
PolynomialFeatures(degree=4)

x_fit = poly.fit_transform(data[['x']])          # 把数填入1到k的框架里 此处注意要用dataframe 而不能是series 所以有两个[]
x_fit                                            # 此时的columns是个array 
array([[1.00000000e+00, 8.24425124e-02, 6.79676785e-03, 5.60342618e-04,
        4.61960532e-05],
       [1.00000000e+00, 4.37322716e-01, 1.91251158e-01, 8.36384759e-02,
        3.65770055e-02],
       [1.00000000e+00, 9.43337580e-01, 8.89885789e-01, 8.39462707e-01,
        7.91896718e-01],
       [1.00000000e+00, 6.00545374e-01, 3.60654746e-01, 2.16589539e-01,
        1.30071846e-01],
        ...

```
### 1.只划分一次train和test data
``` python
from sklearn.model_selection import train_test_split                                # 在sklearn.model_selection这里包里的function
x_train, x_test, y_train, y_test = train_test_split(df, data['y'], test_size=0.25)  # 先写x 再写y test_size是test的占比

# 此时的x_train, x_test都是array的形式 因为有多列
x_train
array([[2.54708595e-01, 6.48764685e-02, 1.65245942e-02, 4.20895617e-03],
       [3.59478780e-01, 1.29224993e-01, 4.64536430e-02, 1.66990989e-02],
       [5.91659431e-01, 3.50060882e-01, 2.07116822e-01, 1.22542621e-01],
       ...
       [9.82940254e-01, 9.66171542e-01, 9.49688901e-01, 9.33487449e-01]])
       
# 此时的y_train, y_test都是series的形式 因为只有一列
y_train
24     5.439070
21     3.921794
28    10.124501
        ...    
36     4.299542
34    14.550314
26    11.064624
Name: y, Length: 30, dtype: float64
```
### 2.train the data
``` python
from sklearn.linear_model import LinearRegression
regr = LinearRegression()           # 这里和上面的Polynomial很像 都是先建object 再带入
regr.fit(x_train, y_train)          # 把x y带入 做完这一步 regr本身已经是一个带有系数和截距项的回归方程了
--- LinearRegression()              # 这里是返回值

np.round(regr.intercept_, 2)        # 截距 保留两位小数
1.42

np.round(regr.coef_, 2)             # 系数 输出是array形式
array([  73.68, -366.81,  670.95, -371.98])
```
### 3.看performance
``` python
regr.score(x_train, y_train)        # 把train中的数据带入 regr 看r2
0.8818274897328483
regr.score(x_test, y_test)          # 把test中的数据带入 regr 看r2 这里与其说是test 不如说是validation验证
0.5979013376227942
```
### 4.带入新数据 把预测的所有值都求出来
``` python
# 假设新数据是changed from 0 to 1 (with step to be 0.01)
a = np.arange(0,1.01,0.01)
x_pre = poly.fit_transform(pd.DataFrame(a))     # 再次注意.fit_transform后面必须是DataFrame
y_pre = regr.predict(x_pre)                     # 这样就求出了每一个预测的值
```
### 5.画出图像 包括train test predict
注意只有predict用曲线 其他都用点  
可以看出predict和train应该很接近 因为就是用原来的train data拟合出来的
``` python
import matplotlib.pyplot as plt
plt.scatter(x_train[:,0], y_train, label='train data')    # 用于拟合的值 都是实际值 注意这里只取第一列 因为是x的值
plt.scatter(x_test[:,0], y_test, label='test data')       # 用于测试的值 也都是实际值
plt.plot(a, y_pre, label='predict data')                  # 用于预测的值 y是预测出来的
plt.legend()
plt.show()
```
![image](https://user-images.githubusercontent.com/105503216/172162050-f5d4142a-be0b-42eb-b55c-adf65c33e9fb.png)
  
  
## cross validation
把整个数据分成n个folder 每次取一份作为test 其他作为train 重复
``` python
# 这里还是预处理 一般数据不需要
k = 5
poly = PolynomialFeatures(k, include_bias=False)

from sklearn.model_selection import cross_val_score      # 这个和前面的train_test_split都属于model_selection里面的 都是分层
x = poly.fit_transform(data[['x']])                      # 这里的x是所有数据 包含了train和test在一起的overall data
regr = LinearRegression()                                # 需要预先设立回归的形式

cross_val_score(regr, x, data['y'], cv=4)                # 并不需要特别有train test分离那一步 在这里直接做了
array([0.89938951, 0.92439518, 0.9663815 , 0.91419212])  # 这个系数的结果也是直接出来的 不需要像train_test_split那样再fit

# 可以求平均R^2
cross_val_score(regr, x, data['y'], cv=4).mean()
```
除了这种复杂的写法 可以用`Pipeline`函数简化
``` python
from sklearn.pipeline import Pipeline                   # Import the pipeline
k = 5
steps = [
    ('poly', PolynomialFeatures(k, include_bias=False)),    # Step 1: polynormial transformation
    ('lr', LinearRegression())                              # Step 2: linear regression
]                                                           # 在step里面 都是构建没有数据的结构

pipe = Pipeline(steps)                                      # Create a pipeline
cross_val_score(pipe, data[['x']], data['y'], cv=4)         # Perform cross-validation 在这里明确是对什么样的数据构建 注意x一定要是2维的 （DadaFrame或2维array）
```

## 使用`GridSearchCV`网格搜索来调参确定poly的最优解
为了找到poly的最优解 即从 x `x**2` `x**3` ... `x**k `中的k是多少  
``` python
from sklearn.model_selection import GridSearchCV                 # Import the grid search tool 和split cross-validation一样

param = {'poly__degree': np.arange(1, 16)}                       # Vary the parameter degree of poly 建字典 选出poly的范围
steps = [
    ('poly', PolynomialFeatures(include_bias=False)),            # Step 1: polynormial transformation 这里也不用写k了
    ('lr', LinearRegression())                                   # Step 2: linear regression
]                        

pipe = Pipeline(steps)                                           # Create a pipline
search = GridSearchCV(pipe, param, cv=4)                         # folder是4 cross-validation的层数是4
search.fit(data[['x']], data['y'])                               # Fit the model with the x and y data  这里又像普通分层了

k_best = search.best_params_['poly__degree']                   
print('Best parameter: {0}'.format(k_best))                      # Best paramter
Best parameter: 5

print('Best score: {0:0.3f}'.format(search.best_score_))         # R-squared value for the best parameter
Best score: 0.926

print(search.cv_results_['mean_test_score'])                     # 在不同的poly系数下的r-squared 
array([  0.53389763,   0.51298326,   0.66893669,   0.84719287,   # 因为param的范围是np.arange(1,16) 所以对应了15个
         0.92608958,   0.91787972,   0.91146481,   0.90729571,
         0.9103041 ,   0.9250095 ,   0.84647308,   0.42031482,
        -1.60123746, -23.24696963, -11.09599076])
```

## Regularized linear models 正则化
防止过度拟合  
正如ols的原则是让ssr最小一样 ridge和lasso回归的原则分别是
![image](https://user-images.githubusercontent.com/105503216/172585550-f3fe78d3-3e39-4721-8ac3-91acccd36c02.png)

注意一开始处理数据时 要特别处理categorical variable
``` python
data = pd.read_csv('credit.csv')
data
  Income	Limit	Rating	Cards	Age	...	Gender	Student	Married	  Ethnicity	Balance
0	14.891	3606	283	     2	  34	...	Male	  No    	Yes     	Caucasian	333
1	106.025	6645	483	     3	  82	...	Female	Yes	    Yes     	Asian	    903
2	104.593	7075	514	     4	  71	...	Male	  No	    No      	Asian	    580
3	148.924	9504	681	     3	  36	...	Female	No	    No      	Asian	    964
4	55.882	4897	357	     2	  68	...	Male	  No	    Yes     	Caucasian	331

# 手动添加dummy variable ols可以自动改变categorical ridge这里好像不可以
# 如果没有drop_first=True 每列是：Gender_ Male	Gender_Female	Student_No	Student_Yes	Married_No	Married_Yes	..
# 有了drop_first=True之后 每列是：Gender_Female	Student_Yes	Married_Yes	
data_num = pd.get_dummies(data, drop_first=True)                       # 这里的drop_first=True是把base column删去
data_num
	Income	Limit	Rating	Cards	Age	...	Gender_Female	Student_Yes	Married_Yes	Ethnicity_Asian	Ethnicity_Caucasian
0	14.891	3606	283	    2	    34	...	0	              0	         1	          0	             1  
1	106.025	6645	483	    3	    82	...	1	              1	         1	          1	             0
2	104.593	7075	514	    4	    71	...	0	              0	         0	          1              0
3	148.924	9504	681	    3	    36	...	1	              0	         0	          1	             0
4	55.882	4897	357	    2	    68	...	0	              0	         1	          0	             1
```

### Ridge regression
在ridge regression使得b1 b2 ... bj的系数接近0 但不会=0 也就是说不会把variable筛选掉
``` python
# 下面的方法grid search类似 都是寻找最优的alpha param： 
# 1.对于每一个alpha 找到最拟合的b1 b2 ... bj 让上面的那个min的式子最小 
# 2.对于每一个alpha 求出拟合之后的R2
# 3.选出R2最大的时候的alpha
param = {'ridge__alpha': 10**np.arange(-5, 4.5, 0.5)}           # Vary the alpha parameter
steps = [
    ('scaler', StandardScaler()),                               # Step 1: Standardized scaler
    ('ridge', Ridge()),                                         # Step 2: ridge regression
]

pipe = Pipeline(steps)                                          # Create a pipline
search = GridSearchCV(pipe, param, cv=4)                        # Create a grid search for the best parameter 
x, y = data_num.drop(columns='Balance'), data_num['Balance']    # Predictor and predicted variables
search.fit(x, y)                                                # Fit the model with the x and y data

alpha_best = search.best_params_['ridge__alpha']                  
print('Best parameter: {0:0.5f}'.format(alpha_best))            # Best paramter
print('Best score: {0:0.3f}'.format(search.best_score_))        # R-squared value for the best parameter
Best parameter: 0.03162
Best score: 0.951
```
### Lasso
lasso会让某些b1 b2 ... bj的值=0 所以会把一些variable给筛选掉 适合多变量的取舍
``` python
param = {'lasso__alpha': 10**np.arange(-5, 5.1, 0.1)}           # Vary the alpha parameter
steps = [
    ('scaler', StandardScaler()),                               # Step 1: Standardized scaler
    ('lasso', Lasso(max_iter=1e5))                              # Step 2: LASSO
]                        

pipe = Pipeline(steps)                                          # Create a pipline
search = GridSearchCV(pipe, param, cv=4)                        # Create a grid search for the best parameter 
x, y = data_num.drop(columns='Balance'), data_num['Balance']    # Predictor and predicted variables
search.fit(x, y)                                                # Fit the model with the x and y data

alpha_best = search.best_params_['lasso__alpha']                  
print('Best parameter: {0}'.format(np.round(alpha_best, 3)))    # Best paramter
print('Best score: {0:0.3f}'.format(search.best_score_))        # R-squared value for the best parameter
Best parameter: 0.501
Best score: 0.951
```
## Dimension reduction 降维
原来很多变量之间有很强的相关性highly correlated
principal components are linear combinations of the original independent variables 主成分是原来独立变量的线性combination  
and the variances along the selected dimensions are maximized. 让剩下来的维度的方差最大
![image](https://user-images.githubusercontent.com/105503216/172603714-da806348-47c3-4a3b-88c7-ade69c14b214.png)
本来每一个点需要用assault和murder两个维度来表示 现在只需要用一个维度来表示了
``` python
from sklearn.preprocessing import StandardScaler        # Import standardization tool 这个function直接standardize
from sklearn.decomposition import PCA                   # Import the PCA tool 主成分

usa = pd.read_csv('USArrests.csv')
usa.head(5)
  States	Murder	Assault	UrbanPop	Rape
0	Alabama	13.2	236	58	21.2
1	Alaska	10.0	263	48	44.5
2	Arizona	8.1	294	80	31.0
3	Arkansas	8.8	190	50	19.5
4	California	9.0	276	91	40.6

x = usa.drop(columns='States')              # All columns except 'States'
x_std = StandardScaler().fit_transform(x)   # Standardization of the variable

nc = 2                                      # Number of principal components  只要2个主成分
pca = PCA(n_components=nc)                  # Create a PCA object and specify the number of components

z = pd.DataFrame(pca.fit_transform(x_std),  # Derive the principal components using the PCA object
                 columns=['PC1', 'PC2'])    # Convert the array to a data frame 这是在命名
z.head(5)                                   # 每一个点 都在新生成的两个主成分的维度下有了数值
  PC1	      PC2
0	0.985566	1.133392
1	1.950138	1.073213
2	1.763164	-0.745957
3	-0.141420	1.119797
4	2.523980	-1.542934
```
![image](https://user-images.githubusercontent.com/105503216/172611245-dba01f71-9bcb-4175-a73b-68e25bfd9e02.png)
可以求出每个系数的值
``` python
pd.DataFrame(pca.components_.T,         # Transpose of the components_ attribute
             columns=['PC1', 'PC2'],    # Column labels
             index=x.columns)           # Row indices
        PC1	      PC2
Murder	0.535899	0.418181
Assault	0.583184	0.187986
UrbanPop	0.278191	-0.872806
Rape	0.543432	-0.167319
```
举一个例子 先PCA 再进行cross validation
``` python
x = hitters_num.drop(columns='Salary')
y = hitters_num['Salary']

nc = 4
steps = [
    ('scaler', StandardScaler()),           # Step 1: Standardize all predictor variables
    ('pca', PCA(n_components=nc)),          # Step 2: PCA transformation
    ('lr', LinearRegression())              # Step 3: linear regression
]        
pipe = Pipeline(steps)                      # Create a pipeline
cross_val_score(pipe, x, y, cv=10).mean()   # Average R^2 from 10-fold cross-validation
```
