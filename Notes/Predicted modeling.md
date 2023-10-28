# Predictive modeling
![image](https://user-images.githubusercontent.com/105503216/172146515-43a4ace0-aa5c-4951-acb8-8d04f0a7c23d.png)
  **核心：scikit_learn这个包在机器学习中经常用到 在后面加上不同的function采用不同的回归方法**  
注意在写的时候 这个包是sklearn.  
## 所有import汇总
``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from numpy import random as rd

pd.set_option("display.max_columns", 10, 
              "display.max_rows", 7)                    # Diaply configuration of Pandas data frame

from sklearn.pipeline import Pipeline 
from sklearn.model_selection import cross_val_score  
from sklearn.model_selection import cross_val_predict
from sklearn.model_selection import ShuffleSplit
from sklearn.model_selection import KFold
from sklearn.model_selection import GridSearchCV        
from sklearn.preprocessing import MinMaxScaler

from sklearn.linear_model import LinearRegression
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC 
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
from sklearn.discriminant_analysis import QuadraticDiscriminantAnalysis
from sklearn.neighbors import KNeighborsClassifier

from sklearn.metrics import confusion_matrix
from sklearn.metrics import auc
from sklearn.metrics import roc_curve
```

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

# Classification
## Logistic regression
和linear regression不一样 linear regression是线性的 没有范围的限制 不适合用于预测概率
![image](https://user-images.githubusercontent.com/105503216/173235673-7f0574c2-980a-4ede-8f85-635a3064e170.png)
### 普通形式 没有验证
``` python
credit = pd.read_csv('default.csv')
credit_num = pd.get_dummies(credit, drop_first=True)       # 依旧是得到dummy variable
credit_num.head(3)
	balance	    income	    default_Yes	student_Yes
0	729.526495	44361.62507 	0         	0
1	817.180407	12106.13470  	0	          1
2	1073.549164	31767.13895 	0	          0

x = credit_num[['balance']]                             # x和y赋值 都是dataframe的形式
y = credit_num[['default_yes']]

logit = LogisticRegression()                            # 这一步就相当于LinearRegression（）
logit.fit(x,y)

x_pred = np.arange(0, 2701).reshape((2701, 1))          # New x values to be predicted 转化成 two dimentional arrary 也可以pd.DataFrame()

y_proba = logit.predict_proba(x_pred)                   # 这个预测方程命名叫做logit function是predict_proba
y_proba                                                 # 第一列是default_yes = 0的概率 第二列是default_yes = 1的概率
array([[9.99976331e-01, 2.36688220e-05],
       [9.99976201e-01, 2.37993302e-05],
       [9.99976069e-01, 2.39305581e-05],
       ...,
       [1.49977179e-02, 9.85002282e-01],
       [1.49166999e-02, 9.85083300e-01],
       [1.48361129e-02, 9.85163887e-01]])
       
y_pred = logit.predict(x_pred)                         # 同样是对已求出的方程logit 但这里的function是predict
y_pred                                                 # default_yes 的值是0还是1
array([0, 0, 0, ..., 1, 1, 1], dtype=uint8)
# As a default setting of the two-class model, 
# the prediction for default_Yes is one if the predicted probability of default is higher than the threshold value 0.5, otherwise the prediction is zero.

plt.hlines(1, xmin=0, xmax=2700, linestyle='--')        # False-level horizontal line
plt.hlines(0, xmin=0, xmax=2700, linestyle='--')        # True-level horizontal line
plt.scatter(x, y, marker='|', c='orange', alpha=0.5)    # Scatter points of sample data
plt.plot(x_pred, y_proba[:, 1],                         # x and y data of the regression curve
         linewidth=2, alpha=0.5, color='b')
plt.plot(x_pred, y_pred, 
         linewidth=3, color='r')

plt.xlabel('balance ($)', fontsize=14)
plt.ylabel('defualt_Yes', fontsize=14)
plt.show()
```
![image](https://user-images.githubusercontent.com/105503216/173237886-a6197d25-f14a-44f3-9d70-be386b8e640b.png)
### cross validation
``` python
x = credit_num[['balance']]                            
y = credit_num[['default_yes']]

from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_predict
regr = LogisticRegression() 
y_pred = cross_val_predict(regr, x, y, cv=4)                   # 用原x y拟合 把原x带回拟合的式子中去预测y_predict


# 可以顺便求一下r*2 但事实上在cross validation中r^2并不重要
from aklearn.model_selection import cross_val_score
cross_val_score(regr, x, y, cv=4)                             
array([0.9708, 0.9728, 0.972 , 0.974 ])

# 重要的是confusion matrix 看正确预测的概率有多少 这里是按照默认0.5的threshold来划分0和1的
from sklearn.metrics import confusion_matrix
confusion = confusion_matrix(y, y_pred, normalize='true')      # 建立confusion matrix
confusion
array([[0.99565532, 0.00434468],
       [0.7027027 , 0.2972973 ]])

pd.DataFrame(confusion, 
             index=['True not default', 'True default'], 
             columns=['Predicted not default', 'Predicted default'])
                  Predicted not default	               Predicted default
True not default	0.995655	               0.004345
True default	        0.702703	               0.297297

# 这里可以看一下事实上的概率是多少
y_proba = cross_val_predict(regr, x, y, cv=4, method='predict_proba')
y_proba                                                       # 依旧第二列是default_yes = 1的概率
array([[9.98645434e-01, 1.35456646e-03],
       [9.97824410e-01, 2.17558988e-03],
       [9.91337533e-01, 8.66246725e-03],
       ...,
       [9.97656620e-01, 2.34337970e-03],
       [8.81030900e-01, 1.18969100e-01],
       [9.99936376e-01, 6.36236742e-05]])
       
```
下面通过交点来确定最优的theshold
``` python
from sklearn.metrics import roc_curve

```
