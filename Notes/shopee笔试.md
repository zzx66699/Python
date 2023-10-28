# shopee笔试
## 01
![IMG_6065](https://user-images.githubusercontent.com/105503216/184270338-cc5ba0f1-9c30-4b45-87da-2c65b838f6a2.JPG)
![IMG_6066](https://user-images.githubusercontent.com/105503216/184270345-2c44378e-e803-4084-a6c0-da6c5dce4c24.JPG)

``` python
import pandas as pd
from datetime import timedelta

feed = pd.read_csv('feed.csv')
feed_comments = pd.read_csv('feed_comments.csv')
df = pd.merge(feed, feed_comments, on='feed_id')
result = df.loc[df['ctime_y'] - df['ctime_x'] <= timedelta(day=3),'feed_id_x'].nunique()
print(result)
```

## 02
![IMG_6067](https://user-images.githubusercontent.com/105503216/184271136-de9bcbef-a508-4e6a-9d7a-64d5cf4842ec.JPG)

``` python
df1 = pd.read_csv('xx.csv')
df2 = df1.groupby('shopid')['itemid'].nuique()
cnt = df2[df2>100].count()
print(cnt)
```

## 03
![IMG_6067](https://user-images.githubusercontent.com/105503216/184271574-67d1d153-7271-49c1-b8aa-14503ac935e3.JPG)

``` python
df2 = df1[df1['cb_option']==1]
cnt = df2['shopid'].nunique()
print(cnt)
```

## 04
<img width="371" alt="image" src="https://user-images.githubusercontent.com/105503216/184272210-8d55db51-8a29-4b2a-8170-99d6e0bff149.png">

``` python
df1['dt'] = pd.to_datetime(df1['item_creation_date'], format='%Y-%m')
cnt = df1[df1['dt']=='2018-01']['itemid'].nunique()
print(cnt)
```

## 05
<img width="390" alt="image" src="https://user-images.githubusercontent.com/105503216/184272277-ea8e731e-9f41-41bc-9dd3-72d0eb3fe710.png">

``` python
df2 = df1[df1['main_category'] == 'Mobile & Gadgets']
cnt = df2.groupby('shopid')['itemid'].nunique()
print(cnt.max().index)
```
