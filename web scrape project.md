# Youtube data scrape using Python

## Scrape data of popular Solo Camping channels in Youtube (data include:Channel name, publish date, Subscriber count, total views and videos uploaded


```python
#importing libraries
from googleapiclient.discovery import build
import pandas as pd
import seaborn as sns
```


```python
#channel details &API
api_key = 'AIzaSyDpdiDrCjDVml3Gy3ryYGH56eKs4W2JrCo'
channel_ids = ['UCOtCKIoHcQvBl1GzRo7Z2SA',
            'UCkamwny0IKatfz82LXcLTHQ',
            'UCRzS5VIxchdzjY3FOW1wIDQ',
            'UCDoJIpPvtEwFvfNXsZvCqKw',
            'UCzAt3mI6v03D8vRR5LECvJg',
            'UC6k05MGcGTTWZY6NS3kDFEg',
            'UClCj1O3_-B3io9f8tyCK08w',
            'UCLNXh0PAGMS7vHH1gWusm5w',
            'UCwn6vsKSyDJUCUPrpad-dDw',
            'UCNCLmBtGIE0oXxZ0KY33KUA']
youtube = build('youtube','v3',developerKey=api_key)
```

## Function to get channel statistics


```python
#Function
def get_channel_stats(youtube, channel_ids):
    all_data = []
    request = youtube.channels().list(
                part='snippet,contentDetails,statistics',
                 id = ','.join (channel_ids))
    response = request.execute()
    
    for i in range(len(response['items'])):
        
        data = dict(channel_name = response['items'][i]['snippet']['title'],
                    publish_date = response['items'][i]['snippet']['publishedAt'],
                     subscribers = response['items'][i]['statistics']['subscriberCount'],
                           views = response['items'][i]['statistics']['viewCount'],
                 videos_uploaded = response['items'][i]['statistics']['videoCount'])
        all_data.append(data)
    return  all_data
```


```python
channel_statistics = get_channel_stats(youtube, channel_ids)
```


```python
channel_data = pd.DataFrame(channel_statistics)
```


```python
#Data Table
channel_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>channel_name</th>
      <th>publish_date</th>
      <th>subscribers</th>
      <th>views</th>
      <th>videos_uploaded</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aqib Ignacy</td>
      <td>2014-10-15T17:59:57Z</td>
      <td>312000</td>
      <td>57617576</td>
      <td>17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Lil Red Heidi Hood</td>
      <td>2016-08-18T22:04:51Z</td>
      <td>473000</td>
      <td>54169618</td>
      <td>156</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Go4x4</td>
      <td>2012-07-31T05:27:16Z</td>
      <td>1080000</td>
      <td>88049315</td>
      <td>105</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Xander Budnick</td>
      <td>2007-09-10T21:44:49Z</td>
      <td>497000</td>
      <td>57518486</td>
      <td>84</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Hike Camp Climb</td>
      <td>2021-04-03T17:31:12.479936Z</td>
      <td>456000</td>
      <td>73914863</td>
      <td>41</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Swedwoods</td>
      <td>2011-06-07T20:48:46Z</td>
      <td>514000</td>
      <td>89109231</td>
      <td>131</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mr Wild Nature</td>
      <td>2020-02-11T20:14:38.579809Z</td>
      <td>224000</td>
      <td>36622183</td>
      <td>110</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Dentan</td>
      <td>2018-04-26T23:02:09Z</td>
      <td>1130000</td>
      <td>144991508</td>
      <td>104</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Tahta Rizky Origma</td>
      <td>2021-10-02T06:08:31.99766Z</td>
      <td>554000</td>
      <td>58716408</td>
      <td>15</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Lost Lakes</td>
      <td>2017-04-28T00:34:46Z</td>
      <td>182000</td>
      <td>34889876</td>
      <td>343</td>
    </tr>
  </tbody>
</table>
</div>




```python
channel_data['subscribers'] = pd.to_numeric(channel_data['subscribers'])
channel_data['views'] = pd.to_numeric(channel_data['views'])
channel_data['videos_uploaded'] = pd.to_numeric(channel_data['videos_uploaded'])
channel_data.dtypes
```




    channel_name       object
    publish_date       object
    subscribers         int64
    views               int64
    videos_uploaded     int64
    dtype: object



## Visualize the data


```python
#Comparision of channels with subcribers count
sns.set(rc={'figure.figsize':(16,8)})
ax = sns.barplot(x ='channel_name',y = 'subscribers',data =channel_data)
```


    
![png](output_11_0.png)
    



```python
#Comparision of channels with Total video Views
ax = sns.barplot(x ='channel_name',y = 'views',data =channel_data)
```


    
![png](output_12_0.png)
    



```python
#Comparision of channels with Number of Videos Uploaded
ax = sns.barplot(x ='channel_name',y = 'videos_uploaded',data =channel_data)
```


    
![png](output_13_0.png)
    



```python

```
