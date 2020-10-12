---
title: "Analyzing Uber Data "
date: 2020-04-01
tags: [big data, data science]
header:
  image: "/images/Uber/Uber1.jpg"
excerpt: "Big Data, Data Science"
mathjax: "true"
---

```python
%pylab inline
import pandas
import seaborn
```

    Populating the interactive namespace from numpy and matplotlib



```python
data = pandas.read_csv('Desktop/uber-raw-data-apr14.txt')
```

# Explore your data


```python
data.head()
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
      <th>Date/Time</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>Base</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4/1/2014 0:11:00</td>
      <td>40.7690</td>
      <td>-73.9549</td>
      <td>B02512</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4/1/2014 0:17:00</td>
      <td>40.7267</td>
      <td>-74.0345</td>
      <td>B02512</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4/1/2014 0:21:00</td>
      <td>40.7316</td>
      <td>-73.9873</td>
      <td>B02512</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4/1/2014 0:28:00</td>
      <td>40.7588</td>
      <td>-73.9776</td>
      <td>B02512</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4/1/2014 0:33:00</td>
      <td>40.7594</td>
      <td>-73.9722</td>
      <td>B02512</td>
    </tr>
  </tbody>
</table>
</div>



# Date and time are string, we need to conver them to integer:


```python
dt = '4/1/2014 0:11:00'
```


```python
dt
```




    '4/1/2014 0:11:00'




```python
d,t = dt.split(' ')
print(d)
print(t)
```

    4/1/2014
    0:11:00



```python
m,d,y=d.split('/')
```


```python
d
```




    '1'




```python
int(d)
```




    1



# Another way to do it:


```python
dt=pandas.to_datetime(dt)
```


```python
dt
```




    Timestamp('2014-04-01 00:11:00')



# here we have nice functions to play with


```python
dt.weekday_name
```

    C:\Users\hamad\AppData\Local\Continuum\anaconda3\lib\site-packages\ipykernel_launcher.py:1: FutureWarning: `weekday_name` is deprecated and will be removed in a future version. Use `day_name` instead
      """Entry point for launching an IPython kernel.





    'Tuesday'




```python
dt.weekday()
```




    1



# Now we convert the whole column:


```python
data['Date/Time']= data['Date/Time'].map(pandas.to_datetime)
```


```python
data.head()
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
      <th>Date/Time</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>Base</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2014-04-01 00:11:00</td>
      <td>40.7690</td>
      <td>-73.9549</td>
      <td>B02512</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014-04-01 00:17:00</td>
      <td>40.7267</td>
      <td>-74.0345</td>
      <td>B02512</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2014-04-01 00:21:00</td>
      <td>40.7316</td>
      <td>-73.9873</td>
      <td>B02512</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2014-04-01 00:28:00</td>
      <td>40.7588</td>
      <td>-73.9776</td>
      <td>B02512</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2014-04-01 00:33:00</td>
      <td>40.7594</td>
      <td>-73.9722</td>
      <td>B02512</td>
    </tr>
  </tbody>
</table>
</div>




```python
data['Date/Time'][0]
```




    Timestamp('2014-04-01 00:11:00')



# Let us create a function to return back date of the month


```python
def getdom(dt):
    return dt.day
data['dom'] = data['Date/Time'].map(getdom)
```


```python
data.tail()
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
      <th>Date/Time</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>Base</th>
      <th>dom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>564511</th>
      <td>2014-04-30 23:22:00</td>
      <td>40.7640</td>
      <td>-73.9744</td>
      <td>B02764</td>
      <td>30</td>
    </tr>
    <tr>
      <th>564512</th>
      <td>2014-04-30 23:26:00</td>
      <td>40.7629</td>
      <td>-73.9672</td>
      <td>B02764</td>
      <td>30</td>
    </tr>
    <tr>
      <th>564513</th>
      <td>2014-04-30 23:31:00</td>
      <td>40.7443</td>
      <td>-73.9889</td>
      <td>B02764</td>
      <td>30</td>
    </tr>
    <tr>
      <th>564514</th>
      <td>2014-04-30 23:32:00</td>
      <td>40.6756</td>
      <td>-73.9405</td>
      <td>B02764</td>
      <td>30</td>
    </tr>
    <tr>
      <th>564515</th>
      <td>2014-04-30 23:48:00</td>
      <td>40.6880</td>
      <td>-73.9608</td>
      <td>B02764</td>
      <td>30</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.head()
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
      <th>Date/Time</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>Base</th>
      <th>dom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2014-04-01 00:11:00</td>
      <td>40.7690</td>
      <td>-73.9549</td>
      <td>B02512</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014-04-01 00:17:00</td>
      <td>40.7267</td>
      <td>-74.0345</td>
      <td>B02512</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2014-04-01 00:21:00</td>
      <td>40.7316</td>
      <td>-73.9873</td>
      <td>B02512</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2014-04-01 00:28:00</td>
      <td>40.7588</td>
      <td>-73.9776</td>
      <td>B02512</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2014-04-01 00:33:00</td>
      <td>40.7594</td>
      <td>-73.9722</td>
      <td>B02512</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



# Create another function for the weekday


```python
def getwkday(dt):
    return dt.weekday()
data['weekday']= data['Date/Time'].map(getwkday)
```

# Create a function to get the hour of the day


```python
def gethr(dt):
    return dt.hour
data['hour']=data['Date/Time'].map(gethr)
```


```python
data.head()
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
      <th>Date/Time</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>Base</th>
      <th>dom</th>
      <th>weekday</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2014-04-01 00:11:00</td>
      <td>40.7690</td>
      <td>-73.9549</td>
      <td>B02512</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014-04-01 00:17:00</td>
      <td>40.7267</td>
      <td>-74.0345</td>
      <td>B02512</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2014-04-01 00:21:00</td>
      <td>40.7316</td>
      <td>-73.9873</td>
      <td>B02512</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2014-04-01 00:28:00</td>
      <td>40.7588</td>
      <td>-73.9776</td>
      <td>B02512</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2014-04-01 00:33:00</td>
      <td>40.7594</td>
      <td>-73.9722</td>
      <td>B02512</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.tail()
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
      <th>Date/Time</th>
      <th>Lat</th>
      <th>Lon</th>
      <th>Base</th>
      <th>dom</th>
      <th>weekday</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>564511</th>
      <td>2014-04-30 23:22:00</td>
      <td>40.7640</td>
      <td>-73.9744</td>
      <td>B02764</td>
      <td>30</td>
      <td>2</td>
      <td>23</td>
    </tr>
    <tr>
      <th>564512</th>
      <td>2014-04-30 23:26:00</td>
      <td>40.7629</td>
      <td>-73.9672</td>
      <td>B02764</td>
      <td>30</td>
      <td>2</td>
      <td>23</td>
    </tr>
    <tr>
      <th>564513</th>
      <td>2014-04-30 23:31:00</td>
      <td>40.7443</td>
      <td>-73.9889</td>
      <td>B02764</td>
      <td>30</td>
      <td>2</td>
      <td>23</td>
    </tr>
    <tr>
      <th>564514</th>
      <td>2014-04-30 23:32:00</td>
      <td>40.6756</td>
      <td>-73.9405</td>
      <td>B02764</td>
      <td>30</td>
      <td>2</td>
      <td>23</td>
    </tr>
    <tr>
      <th>564515</th>
      <td>2014-04-30 23:48:00</td>
      <td>40.6880</td>
      <td>-73.9608</td>
      <td>B02764</td>
      <td>30</td>
      <td>2</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>



# Analysis:

# Analyze the date of the month:


```python
hist(data.dom)
```




    (array([52721., 59680., 52581., 58631., 45427., 56764., 38781., 60673.,
            64697., 74561.]),
     array([ 1. ,  3.9,  6.8,  9.7, 12.6, 15.5, 18.4, 21.3, 24.2, 27.1, 30. ]),
     <a list of 10 Patch objects>)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_33_1.png" alt="linearly separable data">



```python
hist(data.dom, bins = 30 )
```




    (array([14546., 17474., 20701., 26714., 19521., 13445., 19550., 16188.,
            16843., 20041., 20420., 18170., 12112., 12674., 20641., 17717.,
            20973., 18074., 14602., 11017., 13162., 16975., 20346., 23352.,
            25095., 24925., 14677., 15475., 22835., 36251.]),
     array([ 1.        ,  1.96666667,  2.93333333,  3.9       ,  4.86666667,
             5.83333333,  6.8       ,  7.76666667,  8.73333333,  9.7       ,
            10.66666667, 11.63333333, 12.6       , 13.56666667, 14.53333333,
            15.5       , 16.46666667, 17.43333333, 18.4       , 19.36666667,
            20.33333333, 21.3       , 22.26666667, 23.23333333, 24.2       ,
            25.16666667, 26.13333333, 27.1       , 28.06666667, 29.03333333,
            30.        ]),
     <a list of 30 Patch objects>)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_34_1.png" alt="linearly separable data">



```python
hist(data.dom, bins = 30, rwidth=0.8)
```




    (array([14546., 17474., 20701., 26714., 19521., 13445., 19550., 16188.,
            16843., 20041., 20420., 18170., 12112., 12674., 20641., 17717.,
            20973., 18074., 14602., 11017., 13162., 16975., 20346., 23352.,
            25095., 24925., 14677., 15475., 22835., 36251.]),
     array([ 1.        ,  1.96666667,  2.93333333,  3.9       ,  4.86666667,
             5.83333333,  6.8       ,  7.76666667,  8.73333333,  9.7       ,
            10.66666667, 11.63333333, 12.6       , 13.56666667, 14.53333333,
            15.5       , 16.46666667, 17.43333333, 18.4       , 19.36666667,
            20.33333333, 21.3       , 22.26666667, 23.23333333, 24.2       ,
            25.16666667, 26.13333333, 27.1       , 28.06666667, 29.03333333,
            30.        ]),
     <a list of 30 Patch objects>)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_35_1.png" alt="linearly separable data">




```python
hist(data.dom, bins= 30, rwidth=0.8, range=(0.5, 30.5))
xlabel('Date of the Month')
ylabel('Frequency')
title('Frquency by Date of Month - Uber - April 2014')
```




    Text(0.5,1,'Frquency by Date of Month - Uber - April 2014')




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_36_1.png" alt="linearly separable data">




```python
for i, rows in data.groupby('dom'):
    print((i,len(rows)))

```

    (1, 14546)
    (2, 17474)
    (3, 20701)
    (4, 26714)
    (5, 19521)
    (6, 13445)
    (7, 19550)
    (8, 16188)
    (9, 16843)
    (10, 20041)
    (11, 20420)
    (12, 18170)
    (13, 12112)
    (14, 12674)
    (15, 20641)
    (16, 17717)
    (17, 20973)
    (18, 18074)
    (19, 14602)
    (20, 11017)
    (21, 13162)
    (22, 16975)
    (23, 20346)
    (24, 23352)
    (25, 25095)
    (26, 24925)
    (27, 14677)
    (28, 15475)
    (29, 22835)
    (30, 36251)


# Another way of doing it


```python
def count_rows(rows):
    return len(rows)
by_date = data.groupby('dom').apply(count_rows)
by_date
```




    dom
    1     14546
    2     17474
    3     20701
    4     26714
    5     19521
    6     13445
    7     19550
    8     16188
    9     16843
    10    20041
    11    20420
    12    18170
    13    12112
    14    12674
    15    20641
    16    17717
    17    20973
    18    18074
    19    14602
    20    11017
    21    13162
    22    16975
    23    20346
    24    23352
    25    25095
    26    24925
    27    14677
    28    15475
    29    22835
    30    36251
    dtype: int64




```python
plot(by_date)
```




    [<matplotlib.lines.Line2D at 0x1e85e54eac8>]




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_40_1.png" alt="linearly separable data">


# let sort the data by date of the month and frequency of trips


```python
by_date_sorted = by_date.sort_values()
by_date_sorted

```




    dom
    20    11017
    13    12112
    14    12674
    21    13162
    6     13445
    1     14546
    19    14602
    27    14677
    28    15475
    8     16188
    9     16843
    22    16975
    2     17474
    16    17717
    18    18074
    12    18170
    5     19521
    7     19550
    10    20041
    23    20346
    11    20420
    15    20641
    3     20701
    17    20973
    29    22835
    24    23352
    26    24925
    25    25095
    4     26714
    30    36251
    dtype: int64




```python
bar(range(1,31), by_date_sorted)
xticks(range(1,31),by_date_sorted.index)
xlabel('Date of the Month')
ylabel('Frequency')
title('Frquency by Date of Month - Uber - April 2014')
;
```




    ''




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_43_1.png" alt="linearly separable data">


# Analysis of hours:


```python
hist(data.hour, bins=24, range=(0.5,24))
```




    (array([ 7769.,  4935.,  5040.,  6095.,  9476., 18498., 24924., 22843.,
            17939., 17865., 18774., 19425., 22603., 27190., 35324., 42003.,
            45475., 43003., 38923., 36244., 36964., 30645., 20649.,     0.]),
     array([ 0.5       ,  1.47916667,  2.45833333,  3.4375    ,  4.41666667,
             5.39583333,  6.375     ,  7.35416667,  8.33333333,  9.3125    ,
            10.29166667, 11.27083333, 12.25      , 13.22916667, 14.20833333,
            15.1875    , 16.16666667, 17.14583333, 18.125     , 19.10416667,
            20.08333333, 21.0625    , 22.04166667, 23.02083333, 24.        ]),
     <a list of 24 Patch objects>)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_45_1.png" alt="linearly separable data">


# Analysis of weekday:


```python
hist(data.weekday, bins = 7, range=(-0.5,6.5), rwidth= 0.8, color= 'green')
xticks(range(7), 'Mon Tue Wed Thurs Fri Sat Sun'.split())
```




    ([<matplotlib.axis.XTick at 0x1e85e996630>,
      <matplotlib.axis.XTick at 0x1e85e9a8e48>,
      <matplotlib.axis.XTick at 0x1e85e9a8860>,
      <matplotlib.axis.XTick at 0x1e85e923160>,
      <matplotlib.axis.XTick at 0x1e85e923588>,
      <matplotlib.axis.XTick at 0x1e85e923a58>,
      <matplotlib.axis.XTick at 0x1e85e923f28>],
     <a list of 7 Text xticklabel objects>)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_47_1.png" alt="linearly separable data">


# Analyzing hours and day of the week


```python
count_rows(data)
```




    564516




```python
data.groupby('hour weekday'.split()).apply(count_rows)
```




    hour  weekday
    0     0           518
          1           765
          2           899
          3           792
          4          1367
          5          3027
          6          4542
    1     0           261
          1           367
          2           507
          3           459
          4           760
          5          2479
          6          2936
    2     0           238
          1           304
          2           371
          3           342
          4           513
          5          1577
          6          1590
    3     0           571
          1           516
          2           585
          3           567
          4           736
          5          1013
          6          1052
    4     0          1021
          1           887
                     ...
    19    5          5529
          6          2579
    20    0          3573
          1          6310
          2          7783
          3          6345
          4          5165
          5          4792
          6          2276
    21    0          3079
          1          5993
          2          6921
          3          6585
          4          6265
          5          5811
          6          2310
    22    0          1976
          1          3614
          2          4845
          3          5370
          4          6708
          5          6493
          6          1639
    23    0          1091
          1          1948
          2          2571
          3          2909
          4          5393
          5          5719
          6          1018
    Length: 168, dtype: int64




```python
data.groupby('hour weekday'.split()).apply(count_rows).unstack()
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
      <th>weekday</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
    </tr>
    <tr>
      <th>hour</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>518</td>
      <td>765</td>
      <td>899</td>
      <td>792</td>
      <td>1367</td>
      <td>3027</td>
      <td>4542</td>
    </tr>
    <tr>
      <th>1</th>
      <td>261</td>
      <td>367</td>
      <td>507</td>
      <td>459</td>
      <td>760</td>
      <td>2479</td>
      <td>2936</td>
    </tr>
    <tr>
      <th>2</th>
      <td>238</td>
      <td>304</td>
      <td>371</td>
      <td>342</td>
      <td>513</td>
      <td>1577</td>
      <td>1590</td>
    </tr>
    <tr>
      <th>3</th>
      <td>571</td>
      <td>516</td>
      <td>585</td>
      <td>567</td>
      <td>736</td>
      <td>1013</td>
      <td>1052</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1021</td>
      <td>887</td>
      <td>1003</td>
      <td>861</td>
      <td>932</td>
      <td>706</td>
      <td>685</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1619</td>
      <td>1734</td>
      <td>1990</td>
      <td>1454</td>
      <td>1382</td>
      <td>704</td>
      <td>593</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2974</td>
      <td>3766</td>
      <td>4230</td>
      <td>3179</td>
      <td>2836</td>
      <td>844</td>
      <td>669</td>
    </tr>
    <tr>
      <th>7</th>
      <td>3888</td>
      <td>5304</td>
      <td>5647</td>
      <td>4159</td>
      <td>3943</td>
      <td>1110</td>
      <td>873</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3138</td>
      <td>4594</td>
      <td>5242</td>
      <td>3616</td>
      <td>3648</td>
      <td>1372</td>
      <td>1233</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2211</td>
      <td>2962</td>
      <td>3846</td>
      <td>2654</td>
      <td>2732</td>
      <td>1764</td>
      <td>1770</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1953</td>
      <td>2900</td>
      <td>3844</td>
      <td>2370</td>
      <td>2599</td>
      <td>2086</td>
      <td>2113</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1929</td>
      <td>2949</td>
      <td>3889</td>
      <td>2516</td>
      <td>2816</td>
      <td>2315</td>
      <td>2360</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1945</td>
      <td>2819</td>
      <td>3988</td>
      <td>2657</td>
      <td>2978</td>
      <td>2560</td>
      <td>2478</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2294</td>
      <td>3556</td>
      <td>4469</td>
      <td>3301</td>
      <td>3535</td>
      <td>2685</td>
      <td>2763</td>
    </tr>
    <tr>
      <th>14</th>
      <td>3117</td>
      <td>4489</td>
      <td>5438</td>
      <td>4083</td>
      <td>4087</td>
      <td>3042</td>
      <td>2934</td>
    </tr>
    <tr>
      <th>15</th>
      <td>3818</td>
      <td>6042</td>
      <td>7071</td>
      <td>5182</td>
      <td>5354</td>
      <td>4457</td>
      <td>3400</td>
    </tr>
    <tr>
      <th>16</th>
      <td>4962</td>
      <td>7521</td>
      <td>8213</td>
      <td>6149</td>
      <td>6259</td>
      <td>5410</td>
      <td>3489</td>
    </tr>
    <tr>
      <th>17</th>
      <td>5574</td>
      <td>8297</td>
      <td>9151</td>
      <td>6951</td>
      <td>6790</td>
      <td>5558</td>
      <td>3154</td>
    </tr>
    <tr>
      <th>18</th>
      <td>4725</td>
      <td>7089</td>
      <td>8334</td>
      <td>6637</td>
      <td>7258</td>
      <td>6165</td>
      <td>2795</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4386</td>
      <td>6459</td>
      <td>7794</td>
      <td>5929</td>
      <td>6247</td>
      <td>5529</td>
      <td>2579</td>
    </tr>
    <tr>
      <th>20</th>
      <td>3573</td>
      <td>6310</td>
      <td>7783</td>
      <td>6345</td>
      <td>5165</td>
      <td>4792</td>
      <td>2276</td>
    </tr>
    <tr>
      <th>21</th>
      <td>3079</td>
      <td>5993</td>
      <td>6921</td>
      <td>6585</td>
      <td>6265</td>
      <td>5811</td>
      <td>2310</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1976</td>
      <td>3614</td>
      <td>4845</td>
      <td>5370</td>
      <td>6708</td>
      <td>6493</td>
      <td>1639</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1091</td>
      <td>1948</td>
      <td>2571</td>
      <td>2909</td>
      <td>5393</td>
      <td>5719</td>
      <td>1018</td>
    </tr>
  </tbody>
</table>
</div>




```python
by_cross1 = data.groupby('hour weekday'.split()).apply(count_rows).unstack()
```


```python
seaborn.heatmap(by_cross1)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1e85dcdb9b0>




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_53_1.png" alt="linearly separable data">




```python
data.groupby('weekday hour'.split()).apply(count_rows).unstack()
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
      <th>hour</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>14</th>
      <th>15</th>
      <th>16</th>
      <th>17</th>
      <th>18</th>
      <th>19</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
    </tr>
    <tr>
      <th>weekday</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>518</td>
      <td>261</td>
      <td>238</td>
      <td>571</td>
      <td>1021</td>
      <td>1619</td>
      <td>2974</td>
      <td>3888</td>
      <td>3138</td>
      <td>2211</td>
      <td>...</td>
      <td>3117</td>
      <td>3818</td>
      <td>4962</td>
      <td>5574</td>
      <td>4725</td>
      <td>4386</td>
      <td>3573</td>
      <td>3079</td>
      <td>1976</td>
      <td>1091</td>
    </tr>
    <tr>
      <th>1</th>
      <td>765</td>
      <td>367</td>
      <td>304</td>
      <td>516</td>
      <td>887</td>
      <td>1734</td>
      <td>3766</td>
      <td>5304</td>
      <td>4594</td>
      <td>2962</td>
      <td>...</td>
      <td>4489</td>
      <td>6042</td>
      <td>7521</td>
      <td>8297</td>
      <td>7089</td>
      <td>6459</td>
      <td>6310</td>
      <td>5993</td>
      <td>3614</td>
      <td>1948</td>
    </tr>
    <tr>
      <th>2</th>
      <td>899</td>
      <td>507</td>
      <td>371</td>
      <td>585</td>
      <td>1003</td>
      <td>1990</td>
      <td>4230</td>
      <td>5647</td>
      <td>5242</td>
      <td>3846</td>
      <td>...</td>
      <td>5438</td>
      <td>7071</td>
      <td>8213</td>
      <td>9151</td>
      <td>8334</td>
      <td>7794</td>
      <td>7783</td>
      <td>6921</td>
      <td>4845</td>
      <td>2571</td>
    </tr>
    <tr>
      <th>3</th>
      <td>792</td>
      <td>459</td>
      <td>342</td>
      <td>567</td>
      <td>861</td>
      <td>1454</td>
      <td>3179</td>
      <td>4159</td>
      <td>3616</td>
      <td>2654</td>
      <td>...</td>
      <td>4083</td>
      <td>5182</td>
      <td>6149</td>
      <td>6951</td>
      <td>6637</td>
      <td>5929</td>
      <td>6345</td>
      <td>6585</td>
      <td>5370</td>
      <td>2909</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1367</td>
      <td>760</td>
      <td>513</td>
      <td>736</td>
      <td>932</td>
      <td>1382</td>
      <td>2836</td>
      <td>3943</td>
      <td>3648</td>
      <td>2732</td>
      <td>...</td>
      <td>4087</td>
      <td>5354</td>
      <td>6259</td>
      <td>6790</td>
      <td>7258</td>
      <td>6247</td>
      <td>5165</td>
      <td>6265</td>
      <td>6708</td>
      <td>5393</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3027</td>
      <td>2479</td>
      <td>1577</td>
      <td>1013</td>
      <td>706</td>
      <td>704</td>
      <td>844</td>
      <td>1110</td>
      <td>1372</td>
      <td>1764</td>
      <td>...</td>
      <td>3042</td>
      <td>4457</td>
      <td>5410</td>
      <td>5558</td>
      <td>6165</td>
      <td>5529</td>
      <td>4792</td>
      <td>5811</td>
      <td>6493</td>
      <td>5719</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4542</td>
      <td>2936</td>
      <td>1590</td>
      <td>1052</td>
      <td>685</td>
      <td>593</td>
      <td>669</td>
      <td>873</td>
      <td>1233</td>
      <td>1770</td>
      <td>...</td>
      <td>2934</td>
      <td>3400</td>
      <td>3489</td>
      <td>3154</td>
      <td>2795</td>
      <td>2579</td>
      <td>2276</td>
      <td>2310</td>
      <td>1639</td>
      <td>1018</td>
    </tr>
  </tbody>
</table>
<p>7 rows × 24 columns</p>
</div>




```python
by_cross = data.groupby('weekday hour'.split()).apply(count_rows).unstack()
```


```python
seaborn.heatmap(by_cross)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1e85ddb2be0>




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_56_1.png" alt="linearly separable data">



```python
seaborn.clustermap(by_cross)
```




    <seaborn.matrix.ClusterGrid at 0x1e85dc35710>




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_57_1.png" alt="linearly separable data">



# Analysis of lat and long:


```python
hist(data['Lat'], bins= 100, range =(40.5,41))
;
```




    ''




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_59_1.png" alt="linearly separable data">



```python
hist(data['Lon'], bins = 100, range=(-74.5, -73.5))
;
```




    ''




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_60_1.png" alt="linearly separable data">


# we can combine both of them


```python
hist(data['Lat'], bins= 100, range =(40.5,41), color='r')
twiny()
hist(data['Lon'], bins = 100, range=(-74.5, -73.5), color ='g')
;
```




    ''




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_62_1.png" alt="linearly separable data">



```python
hist(data['Lon'], bins = 100, range=(-74.5, -73.5), color ='g', alpha=0.5 )
twiny()
hist(data['Lat'], bins= 100, range =(40.5,41), color='r', alpha = 0.5)
;
```




    ''




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_63_1.png" alt="linearly separable data">




```python
hist(data['Lon'], bins = 100, range=(-74.5, -73.5), color ='g', alpha=0.5, label="Longitudinal" )
grid()
legend(loc= 'upper right')
twiny()
hist(data['Lat'], bins= 100, range =(40.5,41), color='r', alpha = 0.5, label='Latitude')
grid()
legend(loc = 'upper left')
;
```




    ''




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_64_1.png" alt="linearly separable data">




```python
plot(data['Lat'])
```




    [<matplotlib.lines.Line2D at 0x1e85cbdabe0>]




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_65_1.png" alt="linearly separable data">




```python
plot(data['Lat'])
xlim(0,100)
```




    (0, 100)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_66_1.png" alt="linearly separable data">




```python
plot(data['Lat'], '.')
xlim(0,100)
```




    (0, 100)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_67_1.png" alt="linearly separable data">




```python
plot(data['Lon'], data['Lat'])
```




    [<matplotlib.lines.Line2D at 0x1e86149eac8>]




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_68_1.png" alt="linearly separable data">



```python
plot(data['Lon'], data['Lat'],'.')
```




    [<matplotlib.lines.Line2D at 0x1e86119d550>]




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_69_1.png" alt="linearly separable data">



```python
plot(data['Lon'], data['Lat'],'.', ms=1)
```




    [<matplotlib.lines.Line2D at 0x1e8614bdb38>]




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_70_1.png" alt="linearly separable data">




```python
plot(data['Lon'], data['Lat'],'.', ms=1, alpha= 0.5)
```




    [<matplotlib.lines.Line2D at 0x1e85c717748>]




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_71_1.png" alt="linearly separable data">




```python
plot(data['Lon'], data['Lat'],'.', ms=1, alpha= 0.5)
xlim(-74.1,-73.4)
```




    (-74.1, -73.4)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_72_1.png" alt="linearly separable data">



```python
plot(data['Lon'], data['Lat'],'.', ms=1, alpha= 0.5)
xlim(-74.1,-73.4)
ylim(40.3,41.2)
```




    (40.3, 41.2)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_73_1.png" alt="linearly separable data">




```python
figure(figsize=(20,20))
plot(data['Lon'], data['Lat'],'.', ms=0.1, alpha= 0.5)
xlim(-74.2,-73.7)
ylim(40.5,41)
```




    (40.5, 41)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_74_1.png" alt="linearly separable data">




```python
figure(figsize=(20,20))
plot(data['Lon'], data['Lat'],'.', ms=1, alpha= 0.5)
xlim(-74.05,-73.80)
ylim(40.65,40.80)
```




    (40.65, 40.8)




<img src="{{ site.url }}{{ site.baseurl }}/images/Uber/output_75_1.png" alt="linearly separable data">




```python

```


```python

```


```python

```


```python

```


```python

```
