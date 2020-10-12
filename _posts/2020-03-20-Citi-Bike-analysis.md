---
title: "Using SQL in Python, Citi Bike Analysis, as a Case Study "
date: 2020-03-20
tags: [big data, data science, messy data]
header:
  image: "/images/Citi/1_gCCYznoWeo7Hyd2Kx14o4w.png"
excerpt: "Big Data, Data Science, Messy Data"
mathjax: "true"
---

```python
import pandas as pd
```


```python
import sqlite3 as sql
```

# read csv from url



```python
pd.read_csv('https://s3.amazonaws.com/tripdata/JC-201705-citibike-tripdata.csv.zip',nrows=10)
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
      <th>tripduration</th>
      <th>starttime</th>
      <th>stoptime</th>
      <th>start station id</th>
      <th>start station name</th>
      <th>start station latitude</th>
      <th>start station longitude</th>
      <th>end station id</th>
      <th>end station name</th>
      <th>end station latitude</th>
      <th>end station longitude</th>
      <th>bikeid</th>
      <th>usertype</th>
      <th>birth year</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>347</td>
      <td>2017-05-01 00:03:19</td>
      <td>2017-05-01 00:09:06</td>
      <td>3276</td>
      <td>Marin Light Rail</td>
      <td>40.714584</td>
      <td>-74.042817</td>
      <td>3214</td>
      <td>Essex Light Rail</td>
      <td>40.712774</td>
      <td>-74.036486</td>
      <td>26177</td>
      <td>Subscriber</td>
      <td>1958</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>108</td>
      <td>2017-05-01 00:16:04</td>
      <td>2017-05-01 00:17:53</td>
      <td>3275</td>
      <td>Columbus Drive</td>
      <td>40.718355</td>
      <td>-74.038914</td>
      <td>3187</td>
      <td>Warren St</td>
      <td>40.721124</td>
      <td>-74.038051</td>
      <td>26266</td>
      <td>Subscriber</td>
      <td>1963</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>165</td>
      <td>2017-05-01 00:29:59</td>
      <td>2017-05-01 00:32:44</td>
      <td>3267</td>
      <td>Morris Canal</td>
      <td>40.712419</td>
      <td>-74.038526</td>
      <td>3275</td>
      <td>Columbus Drive</td>
      <td>40.718355</td>
      <td>-74.038914</td>
      <td>26168</td>
      <td>Subscriber</td>
      <td>1988</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>289</td>
      <td>2017-05-01 00:30:47</td>
      <td>2017-05-01 00:35:36</td>
      <td>3209</td>
      <td>Brunswick St</td>
      <td>40.724176</td>
      <td>-74.050656</td>
      <td>3205</td>
      <td>JC Medical Center</td>
      <td>40.716540</td>
      <td>-74.049638</td>
      <td>26273</td>
      <td>Subscriber</td>
      <td>1986</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>115</td>
      <td>2017-05-01 00:41:14</td>
      <td>2017-05-01 00:43:09</td>
      <td>3185</td>
      <td>City Hall</td>
      <td>40.717733</td>
      <td>-74.043845</td>
      <td>3213</td>
      <td>Van Vorst Park</td>
      <td>40.718489</td>
      <td>-74.047727</td>
      <td>26293</td>
      <td>Subscriber</td>
      <td>1983</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>285</td>
      <td>2017-05-01 00:47:59</td>
      <td>2017-05-01 00:52:44</td>
      <td>3205</td>
      <td>JC Medical Center</td>
      <td>40.716540</td>
      <td>-74.049638</td>
      <td>3209</td>
      <td>Brunswick St</td>
      <td>40.724176</td>
      <td>-74.050656</td>
      <td>26273</td>
      <td>Subscriber</td>
      <td>1986</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>163</td>
      <td>2017-05-01 04:46:48</td>
      <td>2017-05-01 04:49:32</td>
      <td>3267</td>
      <td>Morris Canal</td>
      <td>40.712419</td>
      <td>-74.038526</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>29645</td>
      <td>Subscriber</td>
      <td>1980</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>876</td>
      <td>2017-05-01 05:02:30</td>
      <td>2017-05-01 05:17:06</td>
      <td>3207</td>
      <td>Oakland Ave</td>
      <td>40.737604</td>
      <td>-74.052478</td>
      <td>3185</td>
      <td>City Hall</td>
      <td>40.717733</td>
      <td>-74.043845</td>
      <td>29272</td>
      <td>Subscriber</td>
      <td>1984</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>394</td>
      <td>2017-05-01 05:17:09</td>
      <td>2017-05-01 05:23:43</td>
      <td>3202</td>
      <td>Newport PATH</td>
      <td>40.727224</td>
      <td>-74.033759</td>
      <td>3272</td>
      <td>Jersey &amp; 3rd</td>
      <td>40.723332</td>
      <td>-74.045953</td>
      <td>26211</td>
      <td>Subscriber</td>
      <td>1976</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>164</td>
      <td>2017-05-01 05:31:28</td>
      <td>2017-05-01 05:34:12</td>
      <td>3214</td>
      <td>Essex Light Rail</td>
      <td>40.712774</td>
      <td>-74.036486</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>19318</td>
      <td>Subscriber</td>
      <td>1979</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



# read csv into sqlite


```python
db = sql.connect('citibike.db')
```


```python
pd.DataFrame.to_sql?
```


```python
url = 'https://s3.amazonaws.com/tripdata/20170%s-citibike-tripdata.csv.zip'
for i in [5,6,7,8,9]:
    print(url % i)
    chunks = pd.read_csv(url %i, chunksize=100_000)
    for chunk in chunks:
        chunk.columns = [column.replace(' ','_') for column in chunk.columns]
        chunk.to_sql('tripdata', db,if_exists='append')
```

    https://s3.amazonaws.com/tripdata/201705-citibike-tripdata.csv.zip
    https://s3.amazonaws.com/tripdata/201706-citibike-tripdata.csv.zip
    https://s3.amazonaws.com/tripdata/201707-citibike-tripdata.csv.zip
    https://s3.amazonaws.com/tripdata/201708-citibike-tripdata.csv.zip
    https://s3.amazonaws.com/tripdata/201709-citibike-tripdata.csv.zip



```python
pd.read_sql_query('SELECT count(*) FROM tripdata',db)
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
      <th>count(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8685057</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.read_sql_query('SELECT * FROM tripdata LIMIT 10',db)
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
      <th>index</th>
      <th>tripduration</th>
      <th>starttime</th>
      <th>stoptime</th>
      <th>start_station_id</th>
      <th>start_station_name</th>
      <th>start_station_latitude</th>
      <th>start_station_longitude</th>
      <th>end_station_id</th>
      <th>end_station_name</th>
      <th>end_station_latitude</th>
      <th>end_station_longitude</th>
      <th>bikeid</th>
      <th>usertype</th>
      <th>birth_year</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>254</td>
      <td>2017-05-01 00:00:13</td>
      <td>2017-05-01 00:04:27</td>
      <td>511</td>
      <td>E 14 St &amp; Avenue B</td>
      <td>40.729387</td>
      <td>-73.977724</td>
      <td>394</td>
      <td>E 9 St &amp; Avenue C</td>
      <td>40.725213</td>
      <td>-73.977688</td>
      <td>27695</td>
      <td>Subscriber</td>
      <td>1996.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>248</td>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01 00:04:28</td>
      <td>511</td>
      <td>E 14 St &amp; Avenue B</td>
      <td>40.729387</td>
      <td>-73.977724</td>
      <td>394</td>
      <td>E 9 St &amp; Avenue C</td>
      <td>40.725213</td>
      <td>-73.977688</td>
      <td>15869</td>
      <td>Subscriber</td>
      <td>1996.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1120</td>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01 00:19:00</td>
      <td>242</td>
      <td>Carlton Ave &amp; Flushing Ave</td>
      <td>40.697787</td>
      <td>-73.973736</td>
      <td>3083</td>
      <td>Bushwick Ave &amp; Powers St</td>
      <td>40.712477</td>
      <td>-73.941000</td>
      <td>18700</td>
      <td>Subscriber</td>
      <td>1985.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>212</td>
      <td>2017-05-01 00:00:24</td>
      <td>2017-05-01 00:03:56</td>
      <td>168</td>
      <td>W 18 St &amp; 6 Ave</td>
      <td>40.739713</td>
      <td>-73.994564</td>
      <td>116</td>
      <td>W 17 St &amp; 8 Ave</td>
      <td>40.741776</td>
      <td>-74.001497</td>
      <td>24981</td>
      <td>Subscriber</td>
      <td>1993.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>686</td>
      <td>2017-05-01 00:00:29</td>
      <td>2017-05-01 00:11:55</td>
      <td>494</td>
      <td>W 26 St &amp; 8 Ave</td>
      <td>40.747348</td>
      <td>-73.997236</td>
      <td>527</td>
      <td>E 33 St &amp; 2 Ave</td>
      <td>40.744023</td>
      <td>-73.976056</td>
      <td>25407</td>
      <td>Subscriber</td>
      <td>1964.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>577</td>
      <td>2017-05-01 00:00:35</td>
      <td>2017-05-01 00:10:12</td>
      <td>334</td>
      <td>W 20 St &amp; 7 Ave</td>
      <td>40.742388</td>
      <td>-73.997262</td>
      <td>504</td>
      <td>1 Ave &amp; E 16 St</td>
      <td>40.732219</td>
      <td>-73.981656</td>
      <td>28713</td>
      <td>Subscriber</td>
      <td>1956.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>523</td>
      <td>2017-05-01 00:00:41</td>
      <td>2017-05-01 00:09:24</td>
      <td>335</td>
      <td>Washington Pl &amp; Broadway</td>
      <td>40.729039</td>
      <td>-73.994046</td>
      <td>487</td>
      <td>E 20 St &amp; FDR Drive</td>
      <td>40.733143</td>
      <td>-73.975739</td>
      <td>15385</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>419</td>
      <td>2017-05-01 00:00:45</td>
      <td>2017-05-01 00:07:44</td>
      <td>336</td>
      <td>Sullivan St &amp; Washington Sq</td>
      <td>40.730477</td>
      <td>-73.999061</td>
      <td>369</td>
      <td>Washington Pl &amp; 6 Ave</td>
      <td>40.732241</td>
      <td>-74.000264</td>
      <td>18295</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>518</td>
      <td>2017-05-01 00:00:47</td>
      <td>2017-05-01 00:09:25</td>
      <td>335</td>
      <td>Washington Pl &amp; Broadway</td>
      <td>40.729039</td>
      <td>-73.994046</td>
      <td>487</td>
      <td>E 20 St &amp; FDR Drive</td>
      <td>40.733143</td>
      <td>-73.975739</td>
      <td>15608</td>
      <td>Subscriber</td>
      <td>1995.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>1296</td>
      <td>2017-05-01 00:00:48</td>
      <td>2017-05-01 00:22:25</td>
      <td>291</td>
      <td>Madison St &amp; Montgomery St</td>
      <td>40.713126</td>
      <td>-73.984844</td>
      <td>291</td>
      <td>Madison St &amp; Montgomery St</td>
      <td>40.713126</td>
      <td>-73.984844</td>
      <td>17351</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.read_sql_query('SELECT min(birth_year), max(birth_year) FROM tripdata',db)
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
      <th>min(birth_year)</th>
      <th>max(birth_year)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1874.0</td>
      <td>2001.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.read_sql_query('SELECT min(tripduration),max(tripduration) FROM tripdata',db)
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
      <th>min(tripduration)</th>
      <th>max(tripduration)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>61</td>
      <td>5807661</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.read_sql_query('SELECT avg(tripduration) FROM tripdata',db)
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
      <th>avg(tripduration)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1055.770104</td>
    </tr>
  </tbody>
</table>
</div>




```python
res =pd.read_sql_query('SELECT * FROM tripdata',db,chunksize=100_000)
```


```python
next(res)
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
      <th>index</th>
      <th>tripduration</th>
      <th>starttime</th>
      <th>stoptime</th>
      <th>start_station_id</th>
      <th>start_station_name</th>
      <th>start_station_latitude</th>
      <th>start_station_longitude</th>
      <th>end_station_id</th>
      <th>end_station_name</th>
      <th>end_station_latitude</th>
      <th>end_station_longitude</th>
      <th>bikeid</th>
      <th>usertype</th>
      <th>birth_year</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>254</td>
      <td>2017-05-01 00:00:13</td>
      <td>2017-05-01 00:04:27</td>
      <td>511</td>
      <td>E 14 St &amp; Avenue B</td>
      <td>40.729387</td>
      <td>-73.977724</td>
      <td>394</td>
      <td>E 9 St &amp; Avenue C</td>
      <td>40.725213</td>
      <td>-73.977688</td>
      <td>27695</td>
      <td>Subscriber</td>
      <td>1996.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>248</td>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01 00:04:28</td>
      <td>511</td>
      <td>E 14 St &amp; Avenue B</td>
      <td>40.729387</td>
      <td>-73.977724</td>
      <td>394</td>
      <td>E 9 St &amp; Avenue C</td>
      <td>40.725213</td>
      <td>-73.977688</td>
      <td>15869</td>
      <td>Subscriber</td>
      <td>1996.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1120</td>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01 00:19:00</td>
      <td>242</td>
      <td>Carlton Ave &amp; Flushing Ave</td>
      <td>40.697787</td>
      <td>-73.973736</td>
      <td>3083</td>
      <td>Bushwick Ave &amp; Powers St</td>
      <td>40.712477</td>
      <td>-73.941000</td>
      <td>18700</td>
      <td>Subscriber</td>
      <td>1985.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>212</td>
      <td>2017-05-01 00:00:24</td>
      <td>2017-05-01 00:03:56</td>
      <td>168</td>
      <td>W 18 St &amp; 6 Ave</td>
      <td>40.739713</td>
      <td>-73.994564</td>
      <td>116</td>
      <td>W 17 St &amp; 8 Ave</td>
      <td>40.741776</td>
      <td>-74.001497</td>
      <td>24981</td>
      <td>Subscriber</td>
      <td>1993.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>686</td>
      <td>2017-05-01 00:00:29</td>
      <td>2017-05-01 00:11:55</td>
      <td>494</td>
      <td>W 26 St &amp; 8 Ave</td>
      <td>40.747348</td>
      <td>-73.997236</td>
      <td>527</td>
      <td>E 33 St &amp; 2 Ave</td>
      <td>40.744023</td>
      <td>-73.976056</td>
      <td>25407</td>
      <td>Subscriber</td>
      <td>1964.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>577</td>
      <td>2017-05-01 00:00:35</td>
      <td>2017-05-01 00:10:12</td>
      <td>334</td>
      <td>W 20 St &amp; 7 Ave</td>
      <td>40.742388</td>
      <td>-73.997262</td>
      <td>504</td>
      <td>1 Ave &amp; E 16 St</td>
      <td>40.732219</td>
      <td>-73.981656</td>
      <td>28713</td>
      <td>Subscriber</td>
      <td>1956.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>523</td>
      <td>2017-05-01 00:00:41</td>
      <td>2017-05-01 00:09:24</td>
      <td>335</td>
      <td>Washington Pl &amp; Broadway</td>
      <td>40.729039</td>
      <td>-73.994046</td>
      <td>487</td>
      <td>E 20 St &amp; FDR Drive</td>
      <td>40.733143</td>
      <td>-73.975739</td>
      <td>15385</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>419</td>
      <td>2017-05-01 00:00:45</td>
      <td>2017-05-01 00:07:44</td>
      <td>336</td>
      <td>Sullivan St &amp; Washington Sq</td>
      <td>40.730477</td>
      <td>-73.999061</td>
      <td>369</td>
      <td>Washington Pl &amp; 6 Ave</td>
      <td>40.732241</td>
      <td>-74.000264</td>
      <td>18295</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>518</td>
      <td>2017-05-01 00:00:47</td>
      <td>2017-05-01 00:09:25</td>
      <td>335</td>
      <td>Washington Pl &amp; Broadway</td>
      <td>40.729039</td>
      <td>-73.994046</td>
      <td>487</td>
      <td>E 20 St &amp; FDR Drive</td>
      <td>40.733143</td>
      <td>-73.975739</td>
      <td>15608</td>
      <td>Subscriber</td>
      <td>1995.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>1296</td>
      <td>2017-05-01 00:00:48</td>
      <td>2017-05-01 00:22:25</td>
      <td>291</td>
      <td>Madison St &amp; Montgomery St</td>
      <td>40.713126</td>
      <td>-73.984844</td>
      <td>291</td>
      <td>Madison St &amp; Montgomery St</td>
      <td>40.713126</td>
      <td>-73.984844</td>
      <td>17351</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>418</td>
      <td>2017-05-01 00:00:59</td>
      <td>2017-05-01 00:07:58</td>
      <td>336</td>
      <td>Sullivan St &amp; Washington Sq</td>
      <td>40.730477</td>
      <td>-73.999061</td>
      <td>369</td>
      <td>Washington Pl &amp; 6 Ave</td>
      <td>40.732241</td>
      <td>-74.000264</td>
      <td>28237</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>589</td>
      <td>2017-05-01 00:01:00</td>
      <td>2017-05-01 00:10:50</td>
      <td>415</td>
      <td>Pearl St &amp; Hanover Square</td>
      <td>40.704718</td>
      <td>-74.009260</td>
      <td>340</td>
      <td>Madison St &amp; Clinton St</td>
      <td>40.712690</td>
      <td>-73.987763</td>
      <td>15289</td>
      <td>Subscriber</td>
      <td>1952.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>735</td>
      <td>2017-05-01 00:01:03</td>
      <td>2017-05-01 00:13:18</td>
      <td>330</td>
      <td>Reade St &amp; Broadway</td>
      <td>40.714505</td>
      <td>-74.005628</td>
      <td>312</td>
      <td>Allen St &amp; Stanton St</td>
      <td>40.722055</td>
      <td>-73.989111</td>
      <td>27270</td>
      <td>Subscriber</td>
      <td>1993.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>950</td>
      <td>2017-05-01 00:01:00</td>
      <td>2017-05-01 00:16:50</td>
      <td>439</td>
      <td>E 4 St &amp; 2 Ave</td>
      <td>40.726281</td>
      <td>-73.989780</td>
      <td>293</td>
      <td>Lafayette St &amp; E 8 St</td>
      <td>40.730207</td>
      <td>-73.991026</td>
      <td>18740</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>2205</td>
      <td>2017-05-01 00:01:03</td>
      <td>2017-05-01 00:37:49</td>
      <td>306</td>
      <td>Cliff St &amp; Fulton St</td>
      <td>40.708235</td>
      <td>-74.005301</td>
      <td>306</td>
      <td>Cliff St &amp; Fulton St</td>
      <td>40.708235</td>
      <td>-74.005301</td>
      <td>17746</td>
      <td>Subscriber</td>
      <td>1986.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15</td>
      <td>391</td>
      <td>2017-05-01 00:01:10</td>
      <td>2017-05-01 00:07:41</td>
      <td>3429</td>
      <td>Hanson Pl &amp; Ashland Pl</td>
      <td>40.685068</td>
      <td>-73.977908</td>
      <td>419</td>
      <td>Carlton Ave &amp; Park Ave</td>
      <td>40.695807</td>
      <td>-73.973556</td>
      <td>26118</td>
      <td>Subscriber</td>
      <td>1984.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>16</td>
      <td>320</td>
      <td>2017-05-01 00:01:11</td>
      <td>2017-05-01 00:06:32</td>
      <td>387</td>
      <td>Centre St &amp; Chambers St</td>
      <td>40.712733</td>
      <td>-74.004607</td>
      <td>2008</td>
      <td>Little West St &amp; 1 Pl</td>
      <td>40.705693</td>
      <td>-74.016777</td>
      <td>26691</td>
      <td>Subscriber</td>
      <td>1992.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>17</td>
      <td>273</td>
      <td>2017-05-01 00:01:11</td>
      <td>2017-05-01 00:05:45</td>
      <td>3090</td>
      <td>N 8 St &amp; Driggs Ave</td>
      <td>40.717746</td>
      <td>-73.956001</td>
      <td>3111</td>
      <td>Norman Ave &amp; Leonard St - 2</td>
      <td>40.725848</td>
      <td>-73.950649</td>
      <td>28209</td>
      <td>Subscriber</td>
      <td>1988.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>18</th>
      <td>18</td>
      <td>461</td>
      <td>2017-05-01 00:01:40</td>
      <td>2017-05-01 00:09:21</td>
      <td>357</td>
      <td>E 11 St &amp; Broadway</td>
      <td>40.732618</td>
      <td>-73.991580</td>
      <td>301</td>
      <td>E 2 St &amp; Avenue B</td>
      <td>40.722174</td>
      <td>-73.983688</td>
      <td>27536</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19</th>
      <td>19</td>
      <td>345</td>
      <td>2017-05-01 00:01:46</td>
      <td>2017-05-01 00:07:32</td>
      <td>350</td>
      <td>Clinton St &amp; Grand St</td>
      <td>40.715595</td>
      <td>-73.987030</td>
      <td>511</td>
      <td>E 14 St &amp; Avenue B</td>
      <td>40.729387</td>
      <td>-73.977724</td>
      <td>29347</td>
      <td>Subscriber</td>
      <td>1974.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>20</th>
      <td>20</td>
      <td>616</td>
      <td>2017-05-01 00:02:01</td>
      <td>2017-05-01 00:12:17</td>
      <td>3074</td>
      <td>Montrose Ave &amp; Bushwick Ave</td>
      <td>40.707678</td>
      <td>-73.940162</td>
      <td>3107</td>
      <td>Bedford Ave &amp; Nassau Ave</td>
      <td>40.723117</td>
      <td>-73.952123</td>
      <td>27667</td>
      <td>Subscriber</td>
      <td>1980.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>21</th>
      <td>21</td>
      <td>552</td>
      <td>2017-05-01 00:02:04</td>
      <td>2017-05-01 00:11:16</td>
      <td>446</td>
      <td>W 24 St &amp; 7 Ave</td>
      <td>40.744876</td>
      <td>-73.995299</td>
      <td>368</td>
      <td>Carmine St &amp; 6 Ave</td>
      <td>40.730386</td>
      <td>-74.002150</td>
      <td>28913</td>
      <td>Subscriber</td>
      <td>1985.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>22</th>
      <td>22</td>
      <td>628</td>
      <td>2017-05-01 00:02:14</td>
      <td>2017-05-01 00:12:42</td>
      <td>434</td>
      <td>9 Ave &amp; W 18 St</td>
      <td>40.743174</td>
      <td>-74.003664</td>
      <td>236</td>
      <td>St Marks Pl &amp; 2 Ave</td>
      <td>40.728419</td>
      <td>-73.987140</td>
      <td>29346</td>
      <td>Subscriber</td>
      <td>1996.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>23</th>
      <td>23</td>
      <td>156</td>
      <td>2017-05-01 00:02:18</td>
      <td>2017-05-01 00:04:55</td>
      <td>3428</td>
      <td>8 Ave &amp; W 16 St</td>
      <td>40.740983</td>
      <td>-74.001702</td>
      <td>509</td>
      <td>9 Ave &amp; W 22 St</td>
      <td>40.745497</td>
      <td>-74.001971</td>
      <td>27170</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>24</th>
      <td>24</td>
      <td>305</td>
      <td>2017-05-01 00:02:27</td>
      <td>2017-05-01 00:07:33</td>
      <td>504</td>
      <td>1 Ave &amp; E 16 St</td>
      <td>40.732219</td>
      <td>-73.981656</td>
      <td>403</td>
      <td>E 2 St &amp; 2 Ave</td>
      <td>40.725029</td>
      <td>-73.990697</td>
      <td>21322</td>
      <td>Subscriber</td>
      <td>1997.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>25</td>
      <td>372</td>
      <td>2017-05-01 00:02:47</td>
      <td>2017-05-01 00:08:59</td>
      <td>387</td>
      <td>Centre St &amp; Chambers St</td>
      <td>40.712733</td>
      <td>-74.004607</td>
      <td>2008</td>
      <td>Little West St &amp; 1 Pl</td>
      <td>40.705693</td>
      <td>-74.016777</td>
      <td>25564</td>
      <td>Subscriber</td>
      <td>1982.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>26</th>
      <td>26</td>
      <td>161</td>
      <td>2017-05-01 00:02:54</td>
      <td>2017-05-01 00:05:35</td>
      <td>399</td>
      <td>Lafayette Ave &amp; St James Pl</td>
      <td>40.688515</td>
      <td>-73.964763</td>
      <td>384</td>
      <td>Fulton St &amp; Washington Ave</td>
      <td>40.683048</td>
      <td>-73.964915</td>
      <td>20545</td>
      <td>Subscriber</td>
      <td>1987.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>27</th>
      <td>27</td>
      <td>227</td>
      <td>2017-05-01 00:02:55</td>
      <td>2017-05-01 00:06:42</td>
      <td>508</td>
      <td>W 46 St &amp; 11 Ave</td>
      <td>40.763414</td>
      <td>-73.996674</td>
      <td>450</td>
      <td>W 49 St &amp; 8 Ave</td>
      <td>40.762272</td>
      <td>-73.987882</td>
      <td>26622</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>28</th>
      <td>28</td>
      <td>1636</td>
      <td>2017-05-01 00:03:12</td>
      <td>2017-05-01 00:30:29</td>
      <td>477</td>
      <td>W 41 St &amp; 8 Ave</td>
      <td>40.756405</td>
      <td>-73.990026</td>
      <td>410</td>
      <td>Suffolk St &amp; Stanton St</td>
      <td>40.720664</td>
      <td>-73.985180</td>
      <td>24814</td>
      <td>Subscriber</td>
      <td>1962.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>29</th>
      <td>29</td>
      <td>1667</td>
      <td>2017-05-01 00:03:16</td>
      <td>2017-05-01 00:31:03</td>
      <td>173</td>
      <td>Broadway &amp; W 49 St</td>
      <td>40.760683</td>
      <td>-73.984527</td>
      <td>358</td>
      <td>Christopher St &amp; Greenwich St</td>
      <td>40.732916</td>
      <td>-74.007114</td>
      <td>26887</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>99970</th>
      <td>99970</td>
      <td>1606</td>
      <td>2017-05-02 18:04:12</td>
      <td>2017-05-02 18:30:59</td>
      <td>168</td>
      <td>W 18 St &amp; 6 Ave</td>
      <td>40.739713</td>
      <td>-73.994564</td>
      <td>360</td>
      <td>William St &amp; Pine St</td>
      <td>40.707179</td>
      <td>-74.008873</td>
      <td>25697</td>
      <td>Subscriber</td>
      <td>1988.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99971</th>
      <td>99971</td>
      <td>325</td>
      <td>2017-05-02 18:04:15</td>
      <td>2017-05-02 18:09:41</td>
      <td>533</td>
      <td>Broadway &amp; W 39 St</td>
      <td>40.752996</td>
      <td>-73.987216</td>
      <td>533</td>
      <td>Broadway &amp; W 39 St</td>
      <td>40.752996</td>
      <td>-73.987216</td>
      <td>26634</td>
      <td>Subscriber</td>
      <td>1980.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99972</th>
      <td>99972</td>
      <td>468</td>
      <td>2017-05-02 18:04:23</td>
      <td>2017-05-02 18:12:11</td>
      <td>3440</td>
      <td>Fulton St &amp; Adams St</td>
      <td>40.692418</td>
      <td>-73.989495</td>
      <td>467</td>
      <td>Dean St &amp; 4 Ave</td>
      <td>40.683125</td>
      <td>-73.978951</td>
      <td>25152</td>
      <td>Subscriber</td>
      <td>1963.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99973</th>
      <td>99973</td>
      <td>974</td>
      <td>2017-05-02 18:04:16</td>
      <td>2017-05-02 18:20:31</td>
      <td>3165</td>
      <td>Central Park West &amp; W 72 St</td>
      <td>40.775794</td>
      <td>-73.976206</td>
      <td>3137</td>
      <td>5 Ave &amp; E 73 St</td>
      <td>40.772828</td>
      <td>-73.966853</td>
      <td>20869</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>99974</th>
      <td>99974</td>
      <td>243</td>
      <td>2017-05-02 18:04:16</td>
      <td>2017-05-02 18:08:20</td>
      <td>3093</td>
      <td>N 6 St &amp; Bedford Ave</td>
      <td>40.717452</td>
      <td>-73.958509</td>
      <td>3102</td>
      <td>Driggs Ave &amp; Lorimer St</td>
      <td>40.721791</td>
      <td>-73.950415</td>
      <td>28112</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99975</th>
      <td>99975</td>
      <td>486</td>
      <td>2017-05-02 18:04:16</td>
      <td>2017-05-02 18:12:22</td>
      <td>3443</td>
      <td>W 52 St &amp; 6 Ave</td>
      <td>40.761330</td>
      <td>-73.979820</td>
      <td>305</td>
      <td>E 58 St &amp; 3 Ave</td>
      <td>40.760958</td>
      <td>-73.967245</td>
      <td>20010</td>
      <td>Subscriber</td>
      <td>1988.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99976</th>
      <td>99976</td>
      <td>125</td>
      <td>2017-05-02 18:04:17</td>
      <td>2017-05-02 18:06:23</td>
      <td>2008</td>
      <td>Little West St &amp; 1 Pl</td>
      <td>40.705693</td>
      <td>-74.016777</td>
      <td>2008</td>
      <td>Little West St &amp; 1 Pl</td>
      <td>40.705693</td>
      <td>-74.016777</td>
      <td>29367</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>99977</th>
      <td>99977</td>
      <td>432</td>
      <td>2017-05-02 18:04:17</td>
      <td>2017-05-02 18:11:30</td>
      <td>3390</td>
      <td>E 109 St &amp; 3 Ave</td>
      <td>40.793297</td>
      <td>-73.943208</td>
      <td>3312</td>
      <td>1 Ave &amp; E 94 St</td>
      <td>40.781721</td>
      <td>-73.945940</td>
      <td>27005</td>
      <td>Subscriber</td>
      <td>1985.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99978</th>
      <td>99978</td>
      <td>1980</td>
      <td>2017-05-02 18:04:17</td>
      <td>2017-05-02 18:37:17</td>
      <td>440</td>
      <td>E 45 St &amp; 3 Ave</td>
      <td>40.752554</td>
      <td>-73.972826</td>
      <td>426</td>
      <td>West St &amp; Chambers St</td>
      <td>40.717548</td>
      <td>-74.013221</td>
      <td>27077</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99979</th>
      <td>99979</td>
      <td>1013</td>
      <td>2017-05-02 18:04:19</td>
      <td>2017-05-02 18:21:13</td>
      <td>3057</td>
      <td>Kosciuszko St &amp; Tompkins Ave</td>
      <td>40.691283</td>
      <td>-73.945242</td>
      <td>3102</td>
      <td>Driggs Ave &amp; Lorimer St</td>
      <td>40.721791</td>
      <td>-73.950415</td>
      <td>27805</td>
      <td>Subscriber</td>
      <td>1992.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99980</th>
      <td>99980</td>
      <td>2273</td>
      <td>2017-05-02 18:04:19</td>
      <td>2017-05-02 18:42:13</td>
      <td>3002</td>
      <td>South End Ave &amp; Liberty St</td>
      <td>40.711512</td>
      <td>-74.015756</td>
      <td>530</td>
      <td>11 Ave &amp; W 59 St</td>
      <td>40.771522</td>
      <td>-73.990541</td>
      <td>17279</td>
      <td>Subscriber</td>
      <td>1955.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99981</th>
      <td>99981</td>
      <td>353</td>
      <td>2017-05-02 18:04:21</td>
      <td>2017-05-02 18:10:14</td>
      <td>252</td>
      <td>MacDougal St &amp; Washington Sq</td>
      <td>40.732264</td>
      <td>-73.998522</td>
      <td>445</td>
      <td>E 10 St &amp; Avenue A</td>
      <td>40.727408</td>
      <td>-73.981420</td>
      <td>27765</td>
      <td>Subscriber</td>
      <td>1992.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99982</th>
      <td>99982</td>
      <td>1642</td>
      <td>2017-05-02 18:04:22</td>
      <td>2017-05-02 18:31:44</td>
      <td>359</td>
      <td>E 47 St &amp; Park Ave</td>
      <td>40.755103</td>
      <td>-73.974987</td>
      <td>3147</td>
      <td>E 85 St &amp; 3 Ave</td>
      <td>40.778012</td>
      <td>-73.954071</td>
      <td>20809</td>
      <td>Subscriber</td>
      <td>1961.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99983</th>
      <td>99983</td>
      <td>1402</td>
      <td>2017-05-02 18:04:21</td>
      <td>2017-05-02 18:27:44</td>
      <td>3158</td>
      <td>W 63 St &amp; Broadway</td>
      <td>40.771639</td>
      <td>-73.982614</td>
      <td>402</td>
      <td>Broadway &amp; E 22 St</td>
      <td>40.740343</td>
      <td>-73.989551</td>
      <td>27577</td>
      <td>Subscriber</td>
      <td>1978.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99984</th>
      <td>99984</td>
      <td>515</td>
      <td>2017-05-02 18:04:22</td>
      <td>2017-05-02 18:12:57</td>
      <td>382</td>
      <td>University Pl &amp; E 14 St</td>
      <td>40.734927</td>
      <td>-73.992005</td>
      <td>446</td>
      <td>W 24 St &amp; 7 Ave</td>
      <td>40.744876</td>
      <td>-73.995299</td>
      <td>25648</td>
      <td>Subscriber</td>
      <td>1986.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99985</th>
      <td>99985</td>
      <td>424</td>
      <td>2017-05-02 18:04:23</td>
      <td>2017-05-02 18:11:28</td>
      <td>478</td>
      <td>11 Ave &amp; W 41 St</td>
      <td>40.760301</td>
      <td>-73.998842</td>
      <td>529</td>
      <td>W 42 St &amp; 8 Ave</td>
      <td>40.757570</td>
      <td>-73.990985</td>
      <td>27017</td>
      <td>Subscriber</td>
      <td>1968.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99986</th>
      <td>99986</td>
      <td>816</td>
      <td>2017-05-02 18:04:24</td>
      <td>2017-05-02 18:18:01</td>
      <td>3002</td>
      <td>South End Ave &amp; Liberty St</td>
      <td>40.711512</td>
      <td>-74.015756</td>
      <td>358</td>
      <td>Christopher St &amp; Greenwich St</td>
      <td>40.732916</td>
      <td>-74.007114</td>
      <td>17504</td>
      <td>Subscriber</td>
      <td>1971.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99987</th>
      <td>99987</td>
      <td>1712</td>
      <td>2017-05-02 18:04:33</td>
      <td>2017-05-02 18:33:05</td>
      <td>3137</td>
      <td>5 Ave &amp; E 73 St</td>
      <td>40.772828</td>
      <td>-73.966853</td>
      <td>3282</td>
      <td>5 Ave &amp; E 88 St</td>
      <td>40.783070</td>
      <td>-73.959390</td>
      <td>28558</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99988</th>
      <td>99988</td>
      <td>1194</td>
      <td>2017-05-02 18:04:26</td>
      <td>2017-05-02 18:24:21</td>
      <td>253</td>
      <td>W 13 St &amp; 5 Ave</td>
      <td>40.735439</td>
      <td>-73.994539</td>
      <td>3002</td>
      <td>South End Ave &amp; Liberty St</td>
      <td>40.711512</td>
      <td>-74.015756</td>
      <td>25335</td>
      <td>Subscriber</td>
      <td>1974.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99989</th>
      <td>99989</td>
      <td>598</td>
      <td>2017-05-02 18:04:27</td>
      <td>2017-05-02 18:14:25</td>
      <td>268</td>
      <td>Howard St &amp; Centre St</td>
      <td>40.719105</td>
      <td>-73.999733</td>
      <td>3002</td>
      <td>South End Ave &amp; Liberty St</td>
      <td>40.711512</td>
      <td>-74.015756</td>
      <td>28662</td>
      <td>Subscriber</td>
      <td>1984.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99990</th>
      <td>99990</td>
      <td>408</td>
      <td>2017-05-02 18:04:27</td>
      <td>2017-05-02 18:11:16</td>
      <td>501</td>
      <td>FDR Drive &amp; E 35 St</td>
      <td>40.744219</td>
      <td>-73.971212</td>
      <td>498</td>
      <td>Broadway &amp; W 32 St</td>
      <td>40.748549</td>
      <td>-73.988084</td>
      <td>27307</td>
      <td>Subscriber</td>
      <td>1983.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99991</th>
      <td>99991</td>
      <td>972</td>
      <td>2017-05-02 18:04:26</td>
      <td>2017-05-02 18:20:39</td>
      <td>3158</td>
      <td>W 63 St &amp; Broadway</td>
      <td>40.771639</td>
      <td>-73.982614</td>
      <td>490</td>
      <td>8 Ave &amp; W 33 St</td>
      <td>40.751551</td>
      <td>-73.993934</td>
      <td>18888</td>
      <td>Subscriber</td>
      <td>1963.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99992</th>
      <td>99992</td>
      <td>1345</td>
      <td>2017-05-02 18:04:28</td>
      <td>2017-05-02 18:26:54</td>
      <td>494</td>
      <td>W 26 St &amp; 8 Ave</td>
      <td>40.747348</td>
      <td>-73.997236</td>
      <td>457</td>
      <td>Broadway &amp; W 58 St</td>
      <td>40.766953</td>
      <td>-73.981693</td>
      <td>28880</td>
      <td>Subscriber</td>
      <td>1980.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99993</th>
      <td>99993</td>
      <td>1178</td>
      <td>2017-05-02 18:04:29</td>
      <td>2017-05-02 18:24:08</td>
      <td>3178</td>
      <td>Riverside Dr &amp; W 78 St</td>
      <td>40.784145</td>
      <td>-73.983625</td>
      <td>388</td>
      <td>W 26 St &amp; 10 Ave</td>
      <td>40.749718</td>
      <td>-74.002950</td>
      <td>18646</td>
      <td>Subscriber</td>
      <td>1988.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99994</th>
      <td>99994</td>
      <td>1582</td>
      <td>2017-05-02 18:04:28</td>
      <td>2017-05-02 18:30:51</td>
      <td>3117</td>
      <td>Franklin St &amp; Dupont St</td>
      <td>40.735640</td>
      <td>-73.958660</td>
      <td>414</td>
      <td>Pearl St &amp; Anchorage Pl</td>
      <td>40.702819</td>
      <td>-73.987658</td>
      <td>28792</td>
      <td>Subscriber</td>
      <td>1976.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99995</th>
      <td>99995</td>
      <td>2530</td>
      <td>2017-05-02 18:04:29</td>
      <td>2017-05-02 18:46:39</td>
      <td>3169</td>
      <td>Riverside Dr &amp; W 82 St</td>
      <td>40.787209</td>
      <td>-73.981281</td>
      <td>515</td>
      <td>W 43 St &amp; 10 Ave</td>
      <td>40.760094</td>
      <td>-73.994618</td>
      <td>26463</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>99996</th>
      <td>99996</td>
      <td>1760</td>
      <td>2017-05-02 18:04:27</td>
      <td>2017-05-02 18:33:48</td>
      <td>382</td>
      <td>University Pl &amp; E 14 St</td>
      <td>40.734927</td>
      <td>-73.992005</td>
      <td>497</td>
      <td>E 17 St &amp; Broadway</td>
      <td>40.737050</td>
      <td>-73.990093</td>
      <td>29094</td>
      <td>Subscriber</td>
      <td>1987.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99997</th>
      <td>99997</td>
      <td>944</td>
      <td>2017-05-02 18:04:30</td>
      <td>2017-05-02 18:20:15</td>
      <td>402</td>
      <td>Broadway &amp; E 22 St</td>
      <td>40.740343</td>
      <td>-73.989551</td>
      <td>524</td>
      <td>W 43 St &amp; 6 Ave</td>
      <td>40.755273</td>
      <td>-73.983169</td>
      <td>28542</td>
      <td>Subscriber</td>
      <td>1987.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99998</th>
      <td>99998</td>
      <td>352</td>
      <td>2017-05-02 18:04:30</td>
      <td>2017-05-02 18:10:22</td>
      <td>498</td>
      <td>Broadway &amp; W 32 St</td>
      <td>40.748549</td>
      <td>-73.988084</td>
      <td>442</td>
      <td>W 27 St &amp; 7 Ave</td>
      <td>40.746647</td>
      <td>-73.993915</td>
      <td>16621</td>
      <td>Subscriber</td>
      <td>1989.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99999</th>
      <td>99999</td>
      <td>227</td>
      <td>2017-05-02 18:04:31</td>
      <td>2017-05-02 18:08:18</td>
      <td>3150</td>
      <td>E 85 St &amp; York Ave</td>
      <td>40.775369</td>
      <td>-73.948034</td>
      <td>3305</td>
      <td>E 91 St &amp; 2 Ave</td>
      <td>40.781122</td>
      <td>-73.949656</td>
      <td>28116</td>
      <td>Subscriber</td>
      <td>1969.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>100000 rows × 16 columns</p>
</div>




```python
def Q(sql):
    res= pd.read_sql_query(sql,db, chunksize=100_000)
    return next(res)
```


```python
Q('SELECT * FROM tripdata')
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
      <th>index</th>
      <th>tripduration</th>
      <th>starttime</th>
      <th>stoptime</th>
      <th>start_station_id</th>
      <th>start_station_name</th>
      <th>start_station_latitude</th>
      <th>start_station_longitude</th>
      <th>end_station_id</th>
      <th>end_station_name</th>
      <th>end_station_latitude</th>
      <th>end_station_longitude</th>
      <th>bikeid</th>
      <th>usertype</th>
      <th>birth_year</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>254</td>
      <td>2017-05-01 00:00:13</td>
      <td>2017-05-01 00:04:27</td>
      <td>511</td>
      <td>E 14 St &amp; Avenue B</td>
      <td>40.729387</td>
      <td>-73.977724</td>
      <td>394</td>
      <td>E 9 St &amp; Avenue C</td>
      <td>40.725213</td>
      <td>-73.977688</td>
      <td>27695</td>
      <td>Subscriber</td>
      <td>1996.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>248</td>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01 00:04:28</td>
      <td>511</td>
      <td>E 14 St &amp; Avenue B</td>
      <td>40.729387</td>
      <td>-73.977724</td>
      <td>394</td>
      <td>E 9 St &amp; Avenue C</td>
      <td>40.725213</td>
      <td>-73.977688</td>
      <td>15869</td>
      <td>Subscriber</td>
      <td>1996.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1120</td>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01 00:19:00</td>
      <td>242</td>
      <td>Carlton Ave &amp; Flushing Ave</td>
      <td>40.697787</td>
      <td>-73.973736</td>
      <td>3083</td>
      <td>Bushwick Ave &amp; Powers St</td>
      <td>40.712477</td>
      <td>-73.941000</td>
      <td>18700</td>
      <td>Subscriber</td>
      <td>1985.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>212</td>
      <td>2017-05-01 00:00:24</td>
      <td>2017-05-01 00:03:56</td>
      <td>168</td>
      <td>W 18 St &amp; 6 Ave</td>
      <td>40.739713</td>
      <td>-73.994564</td>
      <td>116</td>
      <td>W 17 St &amp; 8 Ave</td>
      <td>40.741776</td>
      <td>-74.001497</td>
      <td>24981</td>
      <td>Subscriber</td>
      <td>1993.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>686</td>
      <td>2017-05-01 00:00:29</td>
      <td>2017-05-01 00:11:55</td>
      <td>494</td>
      <td>W 26 St &amp; 8 Ave</td>
      <td>40.747348</td>
      <td>-73.997236</td>
      <td>527</td>
      <td>E 33 St &amp; 2 Ave</td>
      <td>40.744023</td>
      <td>-73.976056</td>
      <td>25407</td>
      <td>Subscriber</td>
      <td>1964.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>577</td>
      <td>2017-05-01 00:00:35</td>
      <td>2017-05-01 00:10:12</td>
      <td>334</td>
      <td>W 20 St &amp; 7 Ave</td>
      <td>40.742388</td>
      <td>-73.997262</td>
      <td>504</td>
      <td>1 Ave &amp; E 16 St</td>
      <td>40.732219</td>
      <td>-73.981656</td>
      <td>28713</td>
      <td>Subscriber</td>
      <td>1956.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>523</td>
      <td>2017-05-01 00:00:41</td>
      <td>2017-05-01 00:09:24</td>
      <td>335</td>
      <td>Washington Pl &amp; Broadway</td>
      <td>40.729039</td>
      <td>-73.994046</td>
      <td>487</td>
      <td>E 20 St &amp; FDR Drive</td>
      <td>40.733143</td>
      <td>-73.975739</td>
      <td>15385</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>419</td>
      <td>2017-05-01 00:00:45</td>
      <td>2017-05-01 00:07:44</td>
      <td>336</td>
      <td>Sullivan St &amp; Washington Sq</td>
      <td>40.730477</td>
      <td>-73.999061</td>
      <td>369</td>
      <td>Washington Pl &amp; 6 Ave</td>
      <td>40.732241</td>
      <td>-74.000264</td>
      <td>18295</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>518</td>
      <td>2017-05-01 00:00:47</td>
      <td>2017-05-01 00:09:25</td>
      <td>335</td>
      <td>Washington Pl &amp; Broadway</td>
      <td>40.729039</td>
      <td>-73.994046</td>
      <td>487</td>
      <td>E 20 St &amp; FDR Drive</td>
      <td>40.733143</td>
      <td>-73.975739</td>
      <td>15608</td>
      <td>Subscriber</td>
      <td>1995.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>1296</td>
      <td>2017-05-01 00:00:48</td>
      <td>2017-05-01 00:22:25</td>
      <td>291</td>
      <td>Madison St &amp; Montgomery St</td>
      <td>40.713126</td>
      <td>-73.984844</td>
      <td>291</td>
      <td>Madison St &amp; Montgomery St</td>
      <td>40.713126</td>
      <td>-73.984844</td>
      <td>17351</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>418</td>
      <td>2017-05-01 00:00:59</td>
      <td>2017-05-01 00:07:58</td>
      <td>336</td>
      <td>Sullivan St &amp; Washington Sq</td>
      <td>40.730477</td>
      <td>-73.999061</td>
      <td>369</td>
      <td>Washington Pl &amp; 6 Ave</td>
      <td>40.732241</td>
      <td>-74.000264</td>
      <td>28237</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>589</td>
      <td>2017-05-01 00:01:00</td>
      <td>2017-05-01 00:10:50</td>
      <td>415</td>
      <td>Pearl St &amp; Hanover Square</td>
      <td>40.704718</td>
      <td>-74.009260</td>
      <td>340</td>
      <td>Madison St &amp; Clinton St</td>
      <td>40.712690</td>
      <td>-73.987763</td>
      <td>15289</td>
      <td>Subscriber</td>
      <td>1952.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>735</td>
      <td>2017-05-01 00:01:03</td>
      <td>2017-05-01 00:13:18</td>
      <td>330</td>
      <td>Reade St &amp; Broadway</td>
      <td>40.714505</td>
      <td>-74.005628</td>
      <td>312</td>
      <td>Allen St &amp; Stanton St</td>
      <td>40.722055</td>
      <td>-73.989111</td>
      <td>27270</td>
      <td>Subscriber</td>
      <td>1993.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>950</td>
      <td>2017-05-01 00:01:00</td>
      <td>2017-05-01 00:16:50</td>
      <td>439</td>
      <td>E 4 St &amp; 2 Ave</td>
      <td>40.726281</td>
      <td>-73.989780</td>
      <td>293</td>
      <td>Lafayette St &amp; E 8 St</td>
      <td>40.730207</td>
      <td>-73.991026</td>
      <td>18740</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>2205</td>
      <td>2017-05-01 00:01:03</td>
      <td>2017-05-01 00:37:49</td>
      <td>306</td>
      <td>Cliff St &amp; Fulton St</td>
      <td>40.708235</td>
      <td>-74.005301</td>
      <td>306</td>
      <td>Cliff St &amp; Fulton St</td>
      <td>40.708235</td>
      <td>-74.005301</td>
      <td>17746</td>
      <td>Subscriber</td>
      <td>1986.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15</td>
      <td>391</td>
      <td>2017-05-01 00:01:10</td>
      <td>2017-05-01 00:07:41</td>
      <td>3429</td>
      <td>Hanson Pl &amp; Ashland Pl</td>
      <td>40.685068</td>
      <td>-73.977908</td>
      <td>419</td>
      <td>Carlton Ave &amp; Park Ave</td>
      <td>40.695807</td>
      <td>-73.973556</td>
      <td>26118</td>
      <td>Subscriber</td>
      <td>1984.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>16</td>
      <td>320</td>
      <td>2017-05-01 00:01:11</td>
      <td>2017-05-01 00:06:32</td>
      <td>387</td>
      <td>Centre St &amp; Chambers St</td>
      <td>40.712733</td>
      <td>-74.004607</td>
      <td>2008</td>
      <td>Little West St &amp; 1 Pl</td>
      <td>40.705693</td>
      <td>-74.016777</td>
      <td>26691</td>
      <td>Subscriber</td>
      <td>1992.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>17</td>
      <td>273</td>
      <td>2017-05-01 00:01:11</td>
      <td>2017-05-01 00:05:45</td>
      <td>3090</td>
      <td>N 8 St &amp; Driggs Ave</td>
      <td>40.717746</td>
      <td>-73.956001</td>
      <td>3111</td>
      <td>Norman Ave &amp; Leonard St - 2</td>
      <td>40.725848</td>
      <td>-73.950649</td>
      <td>28209</td>
      <td>Subscriber</td>
      <td>1988.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>18</th>
      <td>18</td>
      <td>461</td>
      <td>2017-05-01 00:01:40</td>
      <td>2017-05-01 00:09:21</td>
      <td>357</td>
      <td>E 11 St &amp; Broadway</td>
      <td>40.732618</td>
      <td>-73.991580</td>
      <td>301</td>
      <td>E 2 St &amp; Avenue B</td>
      <td>40.722174</td>
      <td>-73.983688</td>
      <td>27536</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19</th>
      <td>19</td>
      <td>345</td>
      <td>2017-05-01 00:01:46</td>
      <td>2017-05-01 00:07:32</td>
      <td>350</td>
      <td>Clinton St &amp; Grand St</td>
      <td>40.715595</td>
      <td>-73.987030</td>
      <td>511</td>
      <td>E 14 St &amp; Avenue B</td>
      <td>40.729387</td>
      <td>-73.977724</td>
      <td>29347</td>
      <td>Subscriber</td>
      <td>1974.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>20</th>
      <td>20</td>
      <td>616</td>
      <td>2017-05-01 00:02:01</td>
      <td>2017-05-01 00:12:17</td>
      <td>3074</td>
      <td>Montrose Ave &amp; Bushwick Ave</td>
      <td>40.707678</td>
      <td>-73.940162</td>
      <td>3107</td>
      <td>Bedford Ave &amp; Nassau Ave</td>
      <td>40.723117</td>
      <td>-73.952123</td>
      <td>27667</td>
      <td>Subscriber</td>
      <td>1980.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>21</th>
      <td>21</td>
      <td>552</td>
      <td>2017-05-01 00:02:04</td>
      <td>2017-05-01 00:11:16</td>
      <td>446</td>
      <td>W 24 St &amp; 7 Ave</td>
      <td>40.744876</td>
      <td>-73.995299</td>
      <td>368</td>
      <td>Carmine St &amp; 6 Ave</td>
      <td>40.730386</td>
      <td>-74.002150</td>
      <td>28913</td>
      <td>Subscriber</td>
      <td>1985.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>22</th>
      <td>22</td>
      <td>628</td>
      <td>2017-05-01 00:02:14</td>
      <td>2017-05-01 00:12:42</td>
      <td>434</td>
      <td>9 Ave &amp; W 18 St</td>
      <td>40.743174</td>
      <td>-74.003664</td>
      <td>236</td>
      <td>St Marks Pl &amp; 2 Ave</td>
      <td>40.728419</td>
      <td>-73.987140</td>
      <td>29346</td>
      <td>Subscriber</td>
      <td>1996.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>23</th>
      <td>23</td>
      <td>156</td>
      <td>2017-05-01 00:02:18</td>
      <td>2017-05-01 00:04:55</td>
      <td>3428</td>
      <td>8 Ave &amp; W 16 St</td>
      <td>40.740983</td>
      <td>-74.001702</td>
      <td>509</td>
      <td>9 Ave &amp; W 22 St</td>
      <td>40.745497</td>
      <td>-74.001971</td>
      <td>27170</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>24</th>
      <td>24</td>
      <td>305</td>
      <td>2017-05-01 00:02:27</td>
      <td>2017-05-01 00:07:33</td>
      <td>504</td>
      <td>1 Ave &amp; E 16 St</td>
      <td>40.732219</td>
      <td>-73.981656</td>
      <td>403</td>
      <td>E 2 St &amp; 2 Ave</td>
      <td>40.725029</td>
      <td>-73.990697</td>
      <td>21322</td>
      <td>Subscriber</td>
      <td>1997.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>25</td>
      <td>372</td>
      <td>2017-05-01 00:02:47</td>
      <td>2017-05-01 00:08:59</td>
      <td>387</td>
      <td>Centre St &amp; Chambers St</td>
      <td>40.712733</td>
      <td>-74.004607</td>
      <td>2008</td>
      <td>Little West St &amp; 1 Pl</td>
      <td>40.705693</td>
      <td>-74.016777</td>
      <td>25564</td>
      <td>Subscriber</td>
      <td>1982.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>26</th>
      <td>26</td>
      <td>161</td>
      <td>2017-05-01 00:02:54</td>
      <td>2017-05-01 00:05:35</td>
      <td>399</td>
      <td>Lafayette Ave &amp; St James Pl</td>
      <td>40.688515</td>
      <td>-73.964763</td>
      <td>384</td>
      <td>Fulton St &amp; Washington Ave</td>
      <td>40.683048</td>
      <td>-73.964915</td>
      <td>20545</td>
      <td>Subscriber</td>
      <td>1987.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>27</th>
      <td>27</td>
      <td>227</td>
      <td>2017-05-01 00:02:55</td>
      <td>2017-05-01 00:06:42</td>
      <td>508</td>
      <td>W 46 St &amp; 11 Ave</td>
      <td>40.763414</td>
      <td>-73.996674</td>
      <td>450</td>
      <td>W 49 St &amp; 8 Ave</td>
      <td>40.762272</td>
      <td>-73.987882</td>
      <td>26622</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>28</th>
      <td>28</td>
      <td>1636</td>
      <td>2017-05-01 00:03:12</td>
      <td>2017-05-01 00:30:29</td>
      <td>477</td>
      <td>W 41 St &amp; 8 Ave</td>
      <td>40.756405</td>
      <td>-73.990026</td>
      <td>410</td>
      <td>Suffolk St &amp; Stanton St</td>
      <td>40.720664</td>
      <td>-73.985180</td>
      <td>24814</td>
      <td>Subscriber</td>
      <td>1962.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>29</th>
      <td>29</td>
      <td>1667</td>
      <td>2017-05-01 00:03:16</td>
      <td>2017-05-01 00:31:03</td>
      <td>173</td>
      <td>Broadway &amp; W 49 St</td>
      <td>40.760683</td>
      <td>-73.984527</td>
      <td>358</td>
      <td>Christopher St &amp; Greenwich St</td>
      <td>40.732916</td>
      <td>-74.007114</td>
      <td>26887</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>99970</th>
      <td>99970</td>
      <td>1606</td>
      <td>2017-05-02 18:04:12</td>
      <td>2017-05-02 18:30:59</td>
      <td>168</td>
      <td>W 18 St &amp; 6 Ave</td>
      <td>40.739713</td>
      <td>-73.994564</td>
      <td>360</td>
      <td>William St &amp; Pine St</td>
      <td>40.707179</td>
      <td>-74.008873</td>
      <td>25697</td>
      <td>Subscriber</td>
      <td>1988.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99971</th>
      <td>99971</td>
      <td>325</td>
      <td>2017-05-02 18:04:15</td>
      <td>2017-05-02 18:09:41</td>
      <td>533</td>
      <td>Broadway &amp; W 39 St</td>
      <td>40.752996</td>
      <td>-73.987216</td>
      <td>533</td>
      <td>Broadway &amp; W 39 St</td>
      <td>40.752996</td>
      <td>-73.987216</td>
      <td>26634</td>
      <td>Subscriber</td>
      <td>1980.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99972</th>
      <td>99972</td>
      <td>468</td>
      <td>2017-05-02 18:04:23</td>
      <td>2017-05-02 18:12:11</td>
      <td>3440</td>
      <td>Fulton St &amp; Adams St</td>
      <td>40.692418</td>
      <td>-73.989495</td>
      <td>467</td>
      <td>Dean St &amp; 4 Ave</td>
      <td>40.683125</td>
      <td>-73.978951</td>
      <td>25152</td>
      <td>Subscriber</td>
      <td>1963.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99973</th>
      <td>99973</td>
      <td>974</td>
      <td>2017-05-02 18:04:16</td>
      <td>2017-05-02 18:20:31</td>
      <td>3165</td>
      <td>Central Park West &amp; W 72 St</td>
      <td>40.775794</td>
      <td>-73.976206</td>
      <td>3137</td>
      <td>5 Ave &amp; E 73 St</td>
      <td>40.772828</td>
      <td>-73.966853</td>
      <td>20869</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>99974</th>
      <td>99974</td>
      <td>243</td>
      <td>2017-05-02 18:04:16</td>
      <td>2017-05-02 18:08:20</td>
      <td>3093</td>
      <td>N 6 St &amp; Bedford Ave</td>
      <td>40.717452</td>
      <td>-73.958509</td>
      <td>3102</td>
      <td>Driggs Ave &amp; Lorimer St</td>
      <td>40.721791</td>
      <td>-73.950415</td>
      <td>28112</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99975</th>
      <td>99975</td>
      <td>486</td>
      <td>2017-05-02 18:04:16</td>
      <td>2017-05-02 18:12:22</td>
      <td>3443</td>
      <td>W 52 St &amp; 6 Ave</td>
      <td>40.761330</td>
      <td>-73.979820</td>
      <td>305</td>
      <td>E 58 St &amp; 3 Ave</td>
      <td>40.760958</td>
      <td>-73.967245</td>
      <td>20010</td>
      <td>Subscriber</td>
      <td>1988.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99976</th>
      <td>99976</td>
      <td>125</td>
      <td>2017-05-02 18:04:17</td>
      <td>2017-05-02 18:06:23</td>
      <td>2008</td>
      <td>Little West St &amp; 1 Pl</td>
      <td>40.705693</td>
      <td>-74.016777</td>
      <td>2008</td>
      <td>Little West St &amp; 1 Pl</td>
      <td>40.705693</td>
      <td>-74.016777</td>
      <td>29367</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>99977</th>
      <td>99977</td>
      <td>432</td>
      <td>2017-05-02 18:04:17</td>
      <td>2017-05-02 18:11:30</td>
      <td>3390</td>
      <td>E 109 St &amp; 3 Ave</td>
      <td>40.793297</td>
      <td>-73.943208</td>
      <td>3312</td>
      <td>1 Ave &amp; E 94 St</td>
      <td>40.781721</td>
      <td>-73.945940</td>
      <td>27005</td>
      <td>Subscriber</td>
      <td>1985.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99978</th>
      <td>99978</td>
      <td>1980</td>
      <td>2017-05-02 18:04:17</td>
      <td>2017-05-02 18:37:17</td>
      <td>440</td>
      <td>E 45 St &amp; 3 Ave</td>
      <td>40.752554</td>
      <td>-73.972826</td>
      <td>426</td>
      <td>West St &amp; Chambers St</td>
      <td>40.717548</td>
      <td>-74.013221</td>
      <td>27077</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99979</th>
      <td>99979</td>
      <td>1013</td>
      <td>2017-05-02 18:04:19</td>
      <td>2017-05-02 18:21:13</td>
      <td>3057</td>
      <td>Kosciuszko St &amp; Tompkins Ave</td>
      <td>40.691283</td>
      <td>-73.945242</td>
      <td>3102</td>
      <td>Driggs Ave &amp; Lorimer St</td>
      <td>40.721791</td>
      <td>-73.950415</td>
      <td>27805</td>
      <td>Subscriber</td>
      <td>1992.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99980</th>
      <td>99980</td>
      <td>2273</td>
      <td>2017-05-02 18:04:19</td>
      <td>2017-05-02 18:42:13</td>
      <td>3002</td>
      <td>South End Ave &amp; Liberty St</td>
      <td>40.711512</td>
      <td>-74.015756</td>
      <td>530</td>
      <td>11 Ave &amp; W 59 St</td>
      <td>40.771522</td>
      <td>-73.990541</td>
      <td>17279</td>
      <td>Subscriber</td>
      <td>1955.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99981</th>
      <td>99981</td>
      <td>353</td>
      <td>2017-05-02 18:04:21</td>
      <td>2017-05-02 18:10:14</td>
      <td>252</td>
      <td>MacDougal St &amp; Washington Sq</td>
      <td>40.732264</td>
      <td>-73.998522</td>
      <td>445</td>
      <td>E 10 St &amp; Avenue A</td>
      <td>40.727408</td>
      <td>-73.981420</td>
      <td>27765</td>
      <td>Subscriber</td>
      <td>1992.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99982</th>
      <td>99982</td>
      <td>1642</td>
      <td>2017-05-02 18:04:22</td>
      <td>2017-05-02 18:31:44</td>
      <td>359</td>
      <td>E 47 St &amp; Park Ave</td>
      <td>40.755103</td>
      <td>-73.974987</td>
      <td>3147</td>
      <td>E 85 St &amp; 3 Ave</td>
      <td>40.778012</td>
      <td>-73.954071</td>
      <td>20809</td>
      <td>Subscriber</td>
      <td>1961.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99983</th>
      <td>99983</td>
      <td>1402</td>
      <td>2017-05-02 18:04:21</td>
      <td>2017-05-02 18:27:44</td>
      <td>3158</td>
      <td>W 63 St &amp; Broadway</td>
      <td>40.771639</td>
      <td>-73.982614</td>
      <td>402</td>
      <td>Broadway &amp; E 22 St</td>
      <td>40.740343</td>
      <td>-73.989551</td>
      <td>27577</td>
      <td>Subscriber</td>
      <td>1978.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99984</th>
      <td>99984</td>
      <td>515</td>
      <td>2017-05-02 18:04:22</td>
      <td>2017-05-02 18:12:57</td>
      <td>382</td>
      <td>University Pl &amp; E 14 St</td>
      <td>40.734927</td>
      <td>-73.992005</td>
      <td>446</td>
      <td>W 24 St &amp; 7 Ave</td>
      <td>40.744876</td>
      <td>-73.995299</td>
      <td>25648</td>
      <td>Subscriber</td>
      <td>1986.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99985</th>
      <td>99985</td>
      <td>424</td>
      <td>2017-05-02 18:04:23</td>
      <td>2017-05-02 18:11:28</td>
      <td>478</td>
      <td>11 Ave &amp; W 41 St</td>
      <td>40.760301</td>
      <td>-73.998842</td>
      <td>529</td>
      <td>W 42 St &amp; 8 Ave</td>
      <td>40.757570</td>
      <td>-73.990985</td>
      <td>27017</td>
      <td>Subscriber</td>
      <td>1968.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99986</th>
      <td>99986</td>
      <td>816</td>
      <td>2017-05-02 18:04:24</td>
      <td>2017-05-02 18:18:01</td>
      <td>3002</td>
      <td>South End Ave &amp; Liberty St</td>
      <td>40.711512</td>
      <td>-74.015756</td>
      <td>358</td>
      <td>Christopher St &amp; Greenwich St</td>
      <td>40.732916</td>
      <td>-74.007114</td>
      <td>17504</td>
      <td>Subscriber</td>
      <td>1971.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99987</th>
      <td>99987</td>
      <td>1712</td>
      <td>2017-05-02 18:04:33</td>
      <td>2017-05-02 18:33:05</td>
      <td>3137</td>
      <td>5 Ave &amp; E 73 St</td>
      <td>40.772828</td>
      <td>-73.966853</td>
      <td>3282</td>
      <td>5 Ave &amp; E 88 St</td>
      <td>40.783070</td>
      <td>-73.959390</td>
      <td>28558</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99988</th>
      <td>99988</td>
      <td>1194</td>
      <td>2017-05-02 18:04:26</td>
      <td>2017-05-02 18:24:21</td>
      <td>253</td>
      <td>W 13 St &amp; 5 Ave</td>
      <td>40.735439</td>
      <td>-73.994539</td>
      <td>3002</td>
      <td>South End Ave &amp; Liberty St</td>
      <td>40.711512</td>
      <td>-74.015756</td>
      <td>25335</td>
      <td>Subscriber</td>
      <td>1974.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99989</th>
      <td>99989</td>
      <td>598</td>
      <td>2017-05-02 18:04:27</td>
      <td>2017-05-02 18:14:25</td>
      <td>268</td>
      <td>Howard St &amp; Centre St</td>
      <td>40.719105</td>
      <td>-73.999733</td>
      <td>3002</td>
      <td>South End Ave &amp; Liberty St</td>
      <td>40.711512</td>
      <td>-74.015756</td>
      <td>28662</td>
      <td>Subscriber</td>
      <td>1984.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99990</th>
      <td>99990</td>
      <td>408</td>
      <td>2017-05-02 18:04:27</td>
      <td>2017-05-02 18:11:16</td>
      <td>501</td>
      <td>FDR Drive &amp; E 35 St</td>
      <td>40.744219</td>
      <td>-73.971212</td>
      <td>498</td>
      <td>Broadway &amp; W 32 St</td>
      <td>40.748549</td>
      <td>-73.988084</td>
      <td>27307</td>
      <td>Subscriber</td>
      <td>1983.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99991</th>
      <td>99991</td>
      <td>972</td>
      <td>2017-05-02 18:04:26</td>
      <td>2017-05-02 18:20:39</td>
      <td>3158</td>
      <td>W 63 St &amp; Broadway</td>
      <td>40.771639</td>
      <td>-73.982614</td>
      <td>490</td>
      <td>8 Ave &amp; W 33 St</td>
      <td>40.751551</td>
      <td>-73.993934</td>
      <td>18888</td>
      <td>Subscriber</td>
      <td>1963.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99992</th>
      <td>99992</td>
      <td>1345</td>
      <td>2017-05-02 18:04:28</td>
      <td>2017-05-02 18:26:54</td>
      <td>494</td>
      <td>W 26 St &amp; 8 Ave</td>
      <td>40.747348</td>
      <td>-73.997236</td>
      <td>457</td>
      <td>Broadway &amp; W 58 St</td>
      <td>40.766953</td>
      <td>-73.981693</td>
      <td>28880</td>
      <td>Subscriber</td>
      <td>1980.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99993</th>
      <td>99993</td>
      <td>1178</td>
      <td>2017-05-02 18:04:29</td>
      <td>2017-05-02 18:24:08</td>
      <td>3178</td>
      <td>Riverside Dr &amp; W 78 St</td>
      <td>40.784145</td>
      <td>-73.983625</td>
      <td>388</td>
      <td>W 26 St &amp; 10 Ave</td>
      <td>40.749718</td>
      <td>-74.002950</td>
      <td>18646</td>
      <td>Subscriber</td>
      <td>1988.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99994</th>
      <td>99994</td>
      <td>1582</td>
      <td>2017-05-02 18:04:28</td>
      <td>2017-05-02 18:30:51</td>
      <td>3117</td>
      <td>Franklin St &amp; Dupont St</td>
      <td>40.735640</td>
      <td>-73.958660</td>
      <td>414</td>
      <td>Pearl St &amp; Anchorage Pl</td>
      <td>40.702819</td>
      <td>-73.987658</td>
      <td>28792</td>
      <td>Subscriber</td>
      <td>1976.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99995</th>
      <td>99995</td>
      <td>2530</td>
      <td>2017-05-02 18:04:29</td>
      <td>2017-05-02 18:46:39</td>
      <td>3169</td>
      <td>Riverside Dr &amp; W 82 St</td>
      <td>40.787209</td>
      <td>-73.981281</td>
      <td>515</td>
      <td>W 43 St &amp; 10 Ave</td>
      <td>40.760094</td>
      <td>-73.994618</td>
      <td>26463</td>
      <td>Customer</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>99996</th>
      <td>99996</td>
      <td>1760</td>
      <td>2017-05-02 18:04:27</td>
      <td>2017-05-02 18:33:48</td>
      <td>382</td>
      <td>University Pl &amp; E 14 St</td>
      <td>40.734927</td>
      <td>-73.992005</td>
      <td>497</td>
      <td>E 17 St &amp; Broadway</td>
      <td>40.737050</td>
      <td>-73.990093</td>
      <td>29094</td>
      <td>Subscriber</td>
      <td>1987.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99997</th>
      <td>99997</td>
      <td>944</td>
      <td>2017-05-02 18:04:30</td>
      <td>2017-05-02 18:20:15</td>
      <td>402</td>
      <td>Broadway &amp; E 22 St</td>
      <td>40.740343</td>
      <td>-73.989551</td>
      <td>524</td>
      <td>W 43 St &amp; 6 Ave</td>
      <td>40.755273</td>
      <td>-73.983169</td>
      <td>28542</td>
      <td>Subscriber</td>
      <td>1987.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>99998</th>
      <td>99998</td>
      <td>352</td>
      <td>2017-05-02 18:04:30</td>
      <td>2017-05-02 18:10:22</td>
      <td>498</td>
      <td>Broadway &amp; W 32 St</td>
      <td>40.748549</td>
      <td>-73.988084</td>
      <td>442</td>
      <td>W 27 St &amp; 7 Ave</td>
      <td>40.746647</td>
      <td>-73.993915</td>
      <td>16621</td>
      <td>Subscriber</td>
      <td>1989.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>99999</th>
      <td>99999</td>
      <td>227</td>
      <td>2017-05-02 18:04:31</td>
      <td>2017-05-02 18:08:18</td>
      <td>3150</td>
      <td>E 85 St &amp; York Ave</td>
      <td>40.775369</td>
      <td>-73.948034</td>
      <td>3305</td>
      <td>E 91 St &amp; 2 Ave</td>
      <td>40.781122</td>
      <td>-73.949656</td>
      <td>28116</td>
      <td>Subscriber</td>
      <td>1969.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>100000 rows × 16 columns</p>
</div>



# geneder query


```python
res=Q('SELECT gender, count(*) from tripdata group by gender')
res
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
      <th>gender</th>
      <th>count(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1058729</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>5618129</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2008199</td>
    </tr>
  </tbody>
</table>
</div>




```python
res=Q( 'select usertype, gender,count(*) from tripdata group by gender, usertype' )
res
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
      <th>usertype</th>
      <th>gender</th>
      <th>count(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Customer</td>
      <td>0</td>
      <td>946624</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Subscriber</td>
      <td>0</td>
      <td>112105</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Customer</td>
      <td>1</td>
      <td>133632</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Subscriber</td>
      <td>1</td>
      <td>5484497</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Customer</td>
      <td>2</td>
      <td>78906</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Subscriber</td>
      <td>2</td>
      <td>1929293</td>
    </tr>
  </tbody>
</table>
</div>




```python
res = Q('''
SELECT
    (2020 - birth_year) as age,
    count(*)
from tripdata
group by age
''')
res
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
      <th>age</th>
      <th>count(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>1026035</td>
    </tr>
    <tr>
      <th>1</th>
      <td>19.0</td>
      <td>3111</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20.0</td>
      <td>13105</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21.0</td>
      <td>22640</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22.0</td>
      <td>35205</td>
    </tr>
    <tr>
      <th>5</th>
      <td>23.0</td>
      <td>43961</td>
    </tr>
    <tr>
      <th>6</th>
      <td>24.0</td>
      <td>61248</td>
    </tr>
    <tr>
      <th>7</th>
      <td>25.0</td>
      <td>82996</td>
    </tr>
    <tr>
      <th>8</th>
      <td>26.0</td>
      <td>145162</td>
    </tr>
    <tr>
      <th>9</th>
      <td>27.0</td>
      <td>225350</td>
    </tr>
    <tr>
      <th>10</th>
      <td>28.0</td>
      <td>275820</td>
    </tr>
    <tr>
      <th>11</th>
      <td>29.0</td>
      <td>303805</td>
    </tr>
    <tr>
      <th>12</th>
      <td>30.0</td>
      <td>329242</td>
    </tr>
    <tr>
      <th>13</th>
      <td>31.0</td>
      <td>333745</td>
    </tr>
    <tr>
      <th>14</th>
      <td>32.0</td>
      <td>336104</td>
    </tr>
    <tr>
      <th>15</th>
      <td>33.0</td>
      <td>324443</td>
    </tr>
    <tr>
      <th>16</th>
      <td>34.0</td>
      <td>320144</td>
    </tr>
    <tr>
      <th>17</th>
      <td>35.0</td>
      <td>315356</td>
    </tr>
    <tr>
      <th>18</th>
      <td>36.0</td>
      <td>290491</td>
    </tr>
    <tr>
      <th>19</th>
      <td>37.0</td>
      <td>272767</td>
    </tr>
    <tr>
      <th>20</th>
      <td>38.0</td>
      <td>254227</td>
    </tr>
    <tr>
      <th>21</th>
      <td>39.0</td>
      <td>235317</td>
    </tr>
    <tr>
      <th>22</th>
      <td>40.0</td>
      <td>213739</td>
    </tr>
    <tr>
      <th>23</th>
      <td>41.0</td>
      <td>195974</td>
    </tr>
    <tr>
      <th>24</th>
      <td>42.0</td>
      <td>178737</td>
    </tr>
    <tr>
      <th>25</th>
      <td>43.0</td>
      <td>167309</td>
    </tr>
    <tr>
      <th>26</th>
      <td>44.0</td>
      <td>164558</td>
    </tr>
    <tr>
      <th>27</th>
      <td>45.0</td>
      <td>151740</td>
    </tr>
    <tr>
      <th>28</th>
      <td>46.0</td>
      <td>153644</td>
    </tr>
    <tr>
      <th>29</th>
      <td>47.0</td>
      <td>140617</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>73</th>
      <td>91.0</td>
      <td>99</td>
    </tr>
    <tr>
      <th>74</th>
      <td>92.0</td>
      <td>69</td>
    </tr>
    <tr>
      <th>75</th>
      <td>93.0</td>
      <td>114</td>
    </tr>
    <tr>
      <th>76</th>
      <td>94.0</td>
      <td>95</td>
    </tr>
    <tr>
      <th>77</th>
      <td>97.0</td>
      <td>532</td>
    </tr>
    <tr>
      <th>78</th>
      <td>99.0</td>
      <td>56</td>
    </tr>
    <tr>
      <th>79</th>
      <td>100.0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>80</th>
      <td>102.0</td>
      <td>100</td>
    </tr>
    <tr>
      <th>81</th>
      <td>103.0</td>
      <td>133</td>
    </tr>
    <tr>
      <th>82</th>
      <td>104.0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>83</th>
      <td>105.0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>84</th>
      <td>107.0</td>
      <td>34</td>
    </tr>
    <tr>
      <th>85</th>
      <td>108.0</td>
      <td>137</td>
    </tr>
    <tr>
      <th>86</th>
      <td>110.0</td>
      <td>193</td>
    </tr>
    <tr>
      <th>87</th>
      <td>113.0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>88</th>
      <td>115.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>89</th>
      <td>119.0</td>
      <td>224</td>
    </tr>
    <tr>
      <th>90</th>
      <td>120.0</td>
      <td>921</td>
    </tr>
    <tr>
      <th>91</th>
      <td>121.0</td>
      <td>165</td>
    </tr>
    <tr>
      <th>92</th>
      <td>124.0</td>
      <td>21</td>
    </tr>
    <tr>
      <th>93</th>
      <td>125.0</td>
      <td>76</td>
    </tr>
    <tr>
      <th>94</th>
      <td>126.0</td>
      <td>32</td>
    </tr>
    <tr>
      <th>95</th>
      <td>127.0</td>
      <td>105</td>
    </tr>
    <tr>
      <th>96</th>
      <td>130.0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>97</th>
      <td>131.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>98</th>
      <td>132.0</td>
      <td>21</td>
    </tr>
    <tr>
      <th>99</th>
      <td>133.0</td>
      <td>48</td>
    </tr>
    <tr>
      <th>100</th>
      <td>134.0</td>
      <td>65</td>
    </tr>
    <tr>
      <th>101</th>
      <td>135.0</td>
      <td>245</td>
    </tr>
    <tr>
      <th>102</th>
      <td>146.0</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
<p>103 rows × 2 columns</p>
</div>




```python
res = Q('''
SELECT
    (2020 - birth_year) as age,
    count(*)
from tripdata
group by age
having age >1
''')
res
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
      <th>age</th>
      <th>count(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19.0</td>
      <td>3111</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20.0</td>
      <td>13105</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.0</td>
      <td>22640</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22.0</td>
      <td>35205</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23.0</td>
      <td>43961</td>
    </tr>
    <tr>
      <th>5</th>
      <td>24.0</td>
      <td>61248</td>
    </tr>
    <tr>
      <th>6</th>
      <td>25.0</td>
      <td>82996</td>
    </tr>
    <tr>
      <th>7</th>
      <td>26.0</td>
      <td>145162</td>
    </tr>
    <tr>
      <th>8</th>
      <td>27.0</td>
      <td>225350</td>
    </tr>
    <tr>
      <th>9</th>
      <td>28.0</td>
      <td>275820</td>
    </tr>
    <tr>
      <th>10</th>
      <td>29.0</td>
      <td>303805</td>
    </tr>
    <tr>
      <th>11</th>
      <td>30.0</td>
      <td>329242</td>
    </tr>
    <tr>
      <th>12</th>
      <td>31.0</td>
      <td>333745</td>
    </tr>
    <tr>
      <th>13</th>
      <td>32.0</td>
      <td>336104</td>
    </tr>
    <tr>
      <th>14</th>
      <td>33.0</td>
      <td>324443</td>
    </tr>
    <tr>
      <th>15</th>
      <td>34.0</td>
      <td>320144</td>
    </tr>
    <tr>
      <th>16</th>
      <td>35.0</td>
      <td>315356</td>
    </tr>
    <tr>
      <th>17</th>
      <td>36.0</td>
      <td>290491</td>
    </tr>
    <tr>
      <th>18</th>
      <td>37.0</td>
      <td>272767</td>
    </tr>
    <tr>
      <th>19</th>
      <td>38.0</td>
      <td>254227</td>
    </tr>
    <tr>
      <th>20</th>
      <td>39.0</td>
      <td>235317</td>
    </tr>
    <tr>
      <th>21</th>
      <td>40.0</td>
      <td>213739</td>
    </tr>
    <tr>
      <th>22</th>
      <td>41.0</td>
      <td>195974</td>
    </tr>
    <tr>
      <th>23</th>
      <td>42.0</td>
      <td>178737</td>
    </tr>
    <tr>
      <th>24</th>
      <td>43.0</td>
      <td>167309</td>
    </tr>
    <tr>
      <th>25</th>
      <td>44.0</td>
      <td>164558</td>
    </tr>
    <tr>
      <th>26</th>
      <td>45.0</td>
      <td>151740</td>
    </tr>
    <tr>
      <th>27</th>
      <td>46.0</td>
      <td>153644</td>
    </tr>
    <tr>
      <th>28</th>
      <td>47.0</td>
      <td>140617</td>
    </tr>
    <tr>
      <th>29</th>
      <td>48.0</td>
      <td>138148</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>72</th>
      <td>91.0</td>
      <td>99</td>
    </tr>
    <tr>
      <th>73</th>
      <td>92.0</td>
      <td>69</td>
    </tr>
    <tr>
      <th>74</th>
      <td>93.0</td>
      <td>114</td>
    </tr>
    <tr>
      <th>75</th>
      <td>94.0</td>
      <td>95</td>
    </tr>
    <tr>
      <th>76</th>
      <td>97.0</td>
      <td>532</td>
    </tr>
    <tr>
      <th>77</th>
      <td>99.0</td>
      <td>56</td>
    </tr>
    <tr>
      <th>78</th>
      <td>100.0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>79</th>
      <td>102.0</td>
      <td>100</td>
    </tr>
    <tr>
      <th>80</th>
      <td>103.0</td>
      <td>133</td>
    </tr>
    <tr>
      <th>81</th>
      <td>104.0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>82</th>
      <td>105.0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>83</th>
      <td>107.0</td>
      <td>34</td>
    </tr>
    <tr>
      <th>84</th>
      <td>108.0</td>
      <td>137</td>
    </tr>
    <tr>
      <th>85</th>
      <td>110.0</td>
      <td>193</td>
    </tr>
    <tr>
      <th>86</th>
      <td>113.0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>87</th>
      <td>115.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>88</th>
      <td>119.0</td>
      <td>224</td>
    </tr>
    <tr>
      <th>89</th>
      <td>120.0</td>
      <td>921</td>
    </tr>
    <tr>
      <th>90</th>
      <td>121.0</td>
      <td>165</td>
    </tr>
    <tr>
      <th>91</th>
      <td>124.0</td>
      <td>21</td>
    </tr>
    <tr>
      <th>92</th>
      <td>125.0</td>
      <td>76</td>
    </tr>
    <tr>
      <th>93</th>
      <td>126.0</td>
      <td>32</td>
    </tr>
    <tr>
      <th>94</th>
      <td>127.0</td>
      <td>105</td>
    </tr>
    <tr>
      <th>95</th>
      <td>130.0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>96</th>
      <td>131.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>97</th>
      <td>132.0</td>
      <td>21</td>
    </tr>
    <tr>
      <th>98</th>
      <td>133.0</td>
      <td>48</td>
    </tr>
    <tr>
      <th>99</th>
      <td>134.0</td>
      <td>65</td>
    </tr>
    <tr>
      <th>100</th>
      <td>135.0</td>
      <td>245</td>
    </tr>
    <tr>
      <th>101</th>
      <td>146.0</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
<p>102 rows × 2 columns</p>
</div>




```python
res = Q('''
SELECT bikeid, sum(tripduration)
FROM tripdata
GROUP BY bikeid
ORDER BY 2
''')
res
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
      <th>bikeid</th>
      <th>sum(tripduration)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>31358</td>
      <td>93</td>
    </tr>
    <tr>
      <th>1</th>
      <td>31789</td>
      <td>157</td>
    </tr>
    <tr>
      <th>2</th>
      <td>29838</td>
      <td>209</td>
    </tr>
    <tr>
      <th>3</th>
      <td>26258</td>
      <td>274</td>
    </tr>
    <tr>
      <th>4</th>
      <td>31941</td>
      <td>304</td>
    </tr>
    <tr>
      <th>5</th>
      <td>29301</td>
      <td>325</td>
    </tr>
    <tr>
      <th>6</th>
      <td>30405</td>
      <td>353</td>
    </tr>
    <tr>
      <th>7</th>
      <td>30829</td>
      <td>451</td>
    </tr>
    <tr>
      <th>8</th>
      <td>30402</td>
      <td>467</td>
    </tr>
    <tr>
      <th>9</th>
      <td>29843</td>
      <td>593</td>
    </tr>
    <tr>
      <th>10</th>
      <td>30294</td>
      <td>642</td>
    </tr>
    <tr>
      <th>11</th>
      <td>29610</td>
      <td>667</td>
    </tr>
    <tr>
      <th>12</th>
      <td>25231</td>
      <td>716</td>
    </tr>
    <tr>
      <th>13</th>
      <td>31107</td>
      <td>741</td>
    </tr>
    <tr>
      <th>14</th>
      <td>24357</td>
      <td>773</td>
    </tr>
    <tr>
      <th>15</th>
      <td>30339</td>
      <td>773</td>
    </tr>
    <tr>
      <th>16</th>
      <td>30050</td>
      <td>877</td>
    </tr>
    <tr>
      <th>17</th>
      <td>29644</td>
      <td>939</td>
    </tr>
    <tr>
      <th>18</th>
      <td>31807</td>
      <td>956</td>
    </tr>
    <tr>
      <th>19</th>
      <td>29637</td>
      <td>1086</td>
    </tr>
    <tr>
      <th>20</th>
      <td>30157</td>
      <td>1265</td>
    </tr>
    <tr>
      <th>21</th>
      <td>29836</td>
      <td>1273</td>
    </tr>
    <tr>
      <th>22</th>
      <td>29253</td>
      <td>1315</td>
    </tr>
    <tr>
      <th>23</th>
      <td>29501</td>
      <td>1326</td>
    </tr>
    <tr>
      <th>24</th>
      <td>29845</td>
      <td>1457</td>
    </tr>
    <tr>
      <th>25</th>
      <td>29824</td>
      <td>1460</td>
    </tr>
    <tr>
      <th>26</th>
      <td>29217</td>
      <td>1498</td>
    </tr>
    <tr>
      <th>27</th>
      <td>31784</td>
      <td>1530</td>
    </tr>
    <tr>
      <th>28</th>
      <td>31782</td>
      <td>1563</td>
    </tr>
    <tr>
      <th>29</th>
      <td>31827</td>
      <td>1593</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12148</th>
      <td>16666</td>
      <td>3874348</td>
    </tr>
    <tr>
      <th>12149</th>
      <td>18811</td>
      <td>3875045</td>
    </tr>
    <tr>
      <th>12150</th>
      <td>21308</td>
      <td>3910883</td>
    </tr>
    <tr>
      <th>12151</th>
      <td>20541</td>
      <td>4071344</td>
    </tr>
    <tr>
      <th>12152</th>
      <td>25707</td>
      <td>4115980</td>
    </tr>
    <tr>
      <th>12153</th>
      <td>28014</td>
      <td>4191835</td>
    </tr>
    <tr>
      <th>12154</th>
      <td>20596</td>
      <td>4225521</td>
    </tr>
    <tr>
      <th>12155</th>
      <td>17515</td>
      <td>4236450</td>
    </tr>
    <tr>
      <th>12156</th>
      <td>26484</td>
      <td>4237832</td>
    </tr>
    <tr>
      <th>12157</th>
      <td>29755</td>
      <td>4330469</td>
    </tr>
    <tr>
      <th>12158</th>
      <td>15462</td>
      <td>4374386</td>
    </tr>
    <tr>
      <th>12159</th>
      <td>24843</td>
      <td>4382533</td>
    </tr>
    <tr>
      <th>12160</th>
      <td>25360</td>
      <td>4500421</td>
    </tr>
    <tr>
      <th>12161</th>
      <td>25945</td>
      <td>4507709</td>
    </tr>
    <tr>
      <th>12162</th>
      <td>27892</td>
      <td>4517035</td>
    </tr>
    <tr>
      <th>12163</th>
      <td>26591</td>
      <td>4517352</td>
    </tr>
    <tr>
      <th>12164</th>
      <td>17428</td>
      <td>4531047</td>
    </tr>
    <tr>
      <th>12165</th>
      <td>26970</td>
      <td>4610802</td>
    </tr>
    <tr>
      <th>12166</th>
      <td>28873</td>
      <td>4644748</td>
    </tr>
    <tr>
      <th>12167</th>
      <td>26975</td>
      <td>4682062</td>
    </tr>
    <tr>
      <th>12168</th>
      <td>17799</td>
      <td>4709650</td>
    </tr>
    <tr>
      <th>12169</th>
      <td>28653</td>
      <td>4781705</td>
    </tr>
    <tr>
      <th>12170</th>
      <td>20145</td>
      <td>4811001</td>
    </tr>
    <tr>
      <th>12171</th>
      <td>17928</td>
      <td>5115114</td>
    </tr>
    <tr>
      <th>12172</th>
      <td>15062</td>
      <td>5189426</td>
    </tr>
    <tr>
      <th>12173</th>
      <td>25505</td>
      <td>5218382</td>
    </tr>
    <tr>
      <th>12174</th>
      <td>21683</td>
      <td>5386738</td>
    </tr>
    <tr>
      <th>12175</th>
      <td>26502</td>
      <td>5867954</td>
    </tr>
    <tr>
      <th>12176</th>
      <td>21173</td>
      <td>6164703</td>
    </tr>
    <tr>
      <th>12177</th>
      <td>16811</td>
      <td>6223765</td>
    </tr>
  </tbody>
</table>
<p>12178 rows × 2 columns</p>
</div>




```python
res = Q('''
SELECT bikeid, sum(tripduration)/3600 as trip_duration_hours
FROM tripdata
GROUP BY bikeid
ORDER BY 2
''')
res
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
      <th>bikeid</th>
      <th>trip_duration_hours</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19312</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22380</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>24357</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>24747</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25231</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>26152</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>26216</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>26258</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>27275</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>29217</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>29253</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>29273</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>29301</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>29484</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>29501</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>29513</td>
      <td>0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>29594</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>29610</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>29619</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>29637</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>29638</td>
      <td>0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>29644</td>
      <td>0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>29650</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>29673</td>
      <td>0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>29705</td>
      <td>0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>29824</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>29831</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>29836</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>29838</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>29843</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12148</th>
      <td>16666</td>
      <td>1076</td>
    </tr>
    <tr>
      <th>12149</th>
      <td>18811</td>
      <td>1076</td>
    </tr>
    <tr>
      <th>12150</th>
      <td>21308</td>
      <td>1086</td>
    </tr>
    <tr>
      <th>12151</th>
      <td>20541</td>
      <td>1130</td>
    </tr>
    <tr>
      <th>12152</th>
      <td>25707</td>
      <td>1143</td>
    </tr>
    <tr>
      <th>12153</th>
      <td>28014</td>
      <td>1164</td>
    </tr>
    <tr>
      <th>12154</th>
      <td>20596</td>
      <td>1173</td>
    </tr>
    <tr>
      <th>12155</th>
      <td>17515</td>
      <td>1176</td>
    </tr>
    <tr>
      <th>12156</th>
      <td>26484</td>
      <td>1177</td>
    </tr>
    <tr>
      <th>12157</th>
      <td>29755</td>
      <td>1202</td>
    </tr>
    <tr>
      <th>12158</th>
      <td>15462</td>
      <td>1215</td>
    </tr>
    <tr>
      <th>12159</th>
      <td>24843</td>
      <td>1217</td>
    </tr>
    <tr>
      <th>12160</th>
      <td>25360</td>
      <td>1250</td>
    </tr>
    <tr>
      <th>12161</th>
      <td>25945</td>
      <td>1252</td>
    </tr>
    <tr>
      <th>12162</th>
      <td>26591</td>
      <td>1254</td>
    </tr>
    <tr>
      <th>12163</th>
      <td>27892</td>
      <td>1254</td>
    </tr>
    <tr>
      <th>12164</th>
      <td>17428</td>
      <td>1258</td>
    </tr>
    <tr>
      <th>12165</th>
      <td>26970</td>
      <td>1280</td>
    </tr>
    <tr>
      <th>12166</th>
      <td>28873</td>
      <td>1290</td>
    </tr>
    <tr>
      <th>12167</th>
      <td>26975</td>
      <td>1300</td>
    </tr>
    <tr>
      <th>12168</th>
      <td>17799</td>
      <td>1308</td>
    </tr>
    <tr>
      <th>12169</th>
      <td>28653</td>
      <td>1328</td>
    </tr>
    <tr>
      <th>12170</th>
      <td>20145</td>
      <td>1336</td>
    </tr>
    <tr>
      <th>12171</th>
      <td>17928</td>
      <td>1420</td>
    </tr>
    <tr>
      <th>12172</th>
      <td>15062</td>
      <td>1441</td>
    </tr>
    <tr>
      <th>12173</th>
      <td>25505</td>
      <td>1449</td>
    </tr>
    <tr>
      <th>12174</th>
      <td>21683</td>
      <td>1496</td>
    </tr>
    <tr>
      <th>12175</th>
      <td>26502</td>
      <td>1629</td>
    </tr>
    <tr>
      <th>12176</th>
      <td>21173</td>
      <td>1712</td>
    </tr>
    <tr>
      <th>12177</th>
      <td>16811</td>
      <td>1728</td>
    </tr>
  </tbody>
</table>
<p>12178 rows × 2 columns</p>
</div>




```python
Q('''
SELECT gender, avg(tripduration)
FROM tripdata
Group by gender

''')
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
      <th>gender</th>
      <th>avg(tripduration)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1879.283770</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>906.374779</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1039.558765</td>
    </tr>
  </tbody>
</table>
</div>




```python
Q('''
SELECT bikeid, sum(tripduration) as total
FROM tripdata
group by bikeid

''')
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
      <th>bikeid</th>
      <th>total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14529</td>
      <td>893894</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14530</td>
      <td>632416</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14531</td>
      <td>694481</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14532</td>
      <td>511588</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14533</td>
      <td>734408</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14534</td>
      <td>657774</td>
    </tr>
    <tr>
      <th>6</th>
      <td>14535</td>
      <td>829025</td>
    </tr>
    <tr>
      <th>7</th>
      <td>14536</td>
      <td>736906</td>
    </tr>
    <tr>
      <th>8</th>
      <td>14537</td>
      <td>643763</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14539</td>
      <td>765586</td>
    </tr>
    <tr>
      <th>10</th>
      <td>14540</td>
      <td>437563</td>
    </tr>
    <tr>
      <th>11</th>
      <td>14541</td>
      <td>800923</td>
    </tr>
    <tr>
      <th>12</th>
      <td>14544</td>
      <td>785139</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14545</td>
      <td>959844</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14547</td>
      <td>624715</td>
    </tr>
    <tr>
      <th>15</th>
      <td>14548</td>
      <td>603874</td>
    </tr>
    <tr>
      <th>16</th>
      <td>14549</td>
      <td>631430</td>
    </tr>
    <tr>
      <th>17</th>
      <td>14550</td>
      <td>520644</td>
    </tr>
    <tr>
      <th>18</th>
      <td>14551</td>
      <td>1608483</td>
    </tr>
    <tr>
      <th>19</th>
      <td>14552</td>
      <td>608622</td>
    </tr>
    <tr>
      <th>20</th>
      <td>14553</td>
      <td>757367</td>
    </tr>
    <tr>
      <th>21</th>
      <td>14555</td>
      <td>638516</td>
    </tr>
    <tr>
      <th>22</th>
      <td>14556</td>
      <td>575367</td>
    </tr>
    <tr>
      <th>23</th>
      <td>14558</td>
      <td>549563</td>
    </tr>
    <tr>
      <th>24</th>
      <td>14559</td>
      <td>796470</td>
    </tr>
    <tr>
      <th>25</th>
      <td>14561</td>
      <td>661922</td>
    </tr>
    <tr>
      <th>26</th>
      <td>14562</td>
      <td>512684</td>
    </tr>
    <tr>
      <th>27</th>
      <td>14563</td>
      <td>588310</td>
    </tr>
    <tr>
      <th>28</th>
      <td>14565</td>
      <td>533523</td>
    </tr>
    <tr>
      <th>29</th>
      <td>14566</td>
      <td>738766</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12148</th>
      <td>31941</td>
      <td>304</td>
    </tr>
    <tr>
      <th>12149</th>
      <td>31942</td>
      <td>11269</td>
    </tr>
    <tr>
      <th>12150</th>
      <td>31944</td>
      <td>23005</td>
    </tr>
    <tr>
      <th>12151</th>
      <td>31947</td>
      <td>11386</td>
    </tr>
    <tr>
      <th>12152</th>
      <td>31950</td>
      <td>20951</td>
    </tr>
    <tr>
      <th>12153</th>
      <td>31951</td>
      <td>3361</td>
    </tr>
    <tr>
      <th>12154</th>
      <td>31953</td>
      <td>9665</td>
    </tr>
    <tr>
      <th>12155</th>
      <td>31955</td>
      <td>19491</td>
    </tr>
    <tr>
      <th>12156</th>
      <td>31958</td>
      <td>18938</td>
    </tr>
    <tr>
      <th>12157</th>
      <td>31959</td>
      <td>14588</td>
    </tr>
    <tr>
      <th>12158</th>
      <td>31960</td>
      <td>12513</td>
    </tr>
    <tr>
      <th>12159</th>
      <td>31961</td>
      <td>4597</td>
    </tr>
    <tr>
      <th>12160</th>
      <td>31962</td>
      <td>14790</td>
    </tr>
    <tr>
      <th>12161</th>
      <td>31963</td>
      <td>16857</td>
    </tr>
    <tr>
      <th>12162</th>
      <td>31964</td>
      <td>18890</td>
    </tr>
    <tr>
      <th>12163</th>
      <td>31965</td>
      <td>4726</td>
    </tr>
    <tr>
      <th>12164</th>
      <td>31966</td>
      <td>11343</td>
    </tr>
    <tr>
      <th>12165</th>
      <td>31967</td>
      <td>7416</td>
    </tr>
    <tr>
      <th>12166</th>
      <td>31968</td>
      <td>15542</td>
    </tr>
    <tr>
      <th>12167</th>
      <td>31969</td>
      <td>9512</td>
    </tr>
    <tr>
      <th>12168</th>
      <td>31970</td>
      <td>17205</td>
    </tr>
    <tr>
      <th>12169</th>
      <td>31971</td>
      <td>15581</td>
    </tr>
    <tr>
      <th>12170</th>
      <td>31972</td>
      <td>7674</td>
    </tr>
    <tr>
      <th>12171</th>
      <td>31973</td>
      <td>7402</td>
    </tr>
    <tr>
      <th>12172</th>
      <td>31974</td>
      <td>9893</td>
    </tr>
    <tr>
      <th>12173</th>
      <td>31975</td>
      <td>18640</td>
    </tr>
    <tr>
      <th>12174</th>
      <td>31976</td>
      <td>14094</td>
    </tr>
    <tr>
      <th>12175</th>
      <td>31977</td>
      <td>13542</td>
    </tr>
    <tr>
      <th>12176</th>
      <td>31978</td>
      <td>2474</td>
    </tr>
    <tr>
      <th>12177</th>
      <td>31979</td>
      <td>18413</td>
    </tr>
  </tbody>
</table>
<p>12178 rows × 2 columns</p>
</div>




```python
Q('''
select hour_bucket, count(*) from (
SELECT bikeid, sum(tripduration)/3600 as hours, round(sum(tripduration)/3600 /100)*100 hour_bucket
FROM tripdata
group by bikeid
)
group by hour_bucket
''')
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
      <th>hour_bucket</th>
      <th>count(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>1725</td>
    </tr>
    <tr>
      <th>1</th>
      <td>100.0</td>
      <td>4336</td>
    </tr>
    <tr>
      <th>2</th>
      <td>200.0</td>
      <td>4686</td>
    </tr>
    <tr>
      <th>3</th>
      <td>300.0</td>
      <td>935</td>
    </tr>
    <tr>
      <th>4</th>
      <td>400.0</td>
      <td>221</td>
    </tr>
    <tr>
      <th>5</th>
      <td>500.0</td>
      <td>102</td>
    </tr>
    <tr>
      <th>6</th>
      <td>600.0</td>
      <td>65</td>
    </tr>
    <tr>
      <th>7</th>
      <td>700.0</td>
      <td>30</td>
    </tr>
    <tr>
      <th>8</th>
      <td>800.0</td>
      <td>29</td>
    </tr>
    <tr>
      <th>9</th>
      <td>900.0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1000.0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1100.0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1200.0</td>
      <td>10</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1300.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1400.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1600.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1700.0</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
Q('''

SELECT starttime
from tripdata
limit 3


'''
)
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
      <th>starttime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2017-05-01 00:00:13</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017-05-01 00:00:19</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2017-05-01 00:00:19</td>
    </tr>
  </tbody>
</table>
</div>




```python
Q('''

SELECT starttime,
substr(starttime,1,10)
from tripdata
limit 3


'''
)
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
      <th>starttime</th>
      <th>substr(starttime,1,10)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2017-05-01 00:00:13</td>
      <td>2017-05-01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01</td>
    </tr>
  </tbody>
</table>
</div>




```python
Q('''

SELECT starttime,
substr(starttime,1,10) as d,
substr(starttime,11,10) as t
from tripdata
limit 3


'''
)
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
      <th>starttime</th>
      <th>d</th>
      <th>t</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2017-05-01 00:00:13</td>
      <td>2017-05-01</td>
      <td>00:00:13</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01</td>
      <td>00:00:19</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01</td>
      <td>00:00:19</td>
    </tr>
  </tbody>
</table>
</div>




```python
Q('''

SELECT starttime,
substr(starttime,1,10) as d,
substr(starttime,1,4) as y,
substr(starttime,6,2) as m,
substr(starttime,9,2) as d,

substr(starttime,11,10) as t
from tripdata
limit 3


'''
)
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
      <th>starttime</th>
      <th>d</th>
      <th>y</th>
      <th>m</th>
      <th>d</th>
      <th>t</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2017-05-01 00:00:13</td>
      <td>2017-05-01</td>
      <td>2017</td>
      <td>05</td>
      <td>01</td>
      <td>00:00:13</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01</td>
      <td>2017</td>
      <td>05</td>
      <td>01</td>
      <td>00:00:19</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2017-05-01 00:00:19</td>
      <td>2017-05-01</td>
      <td>2017</td>
      <td>05</td>
      <td>01</td>
      <td>00:00:19</td>
    </tr>
  </tbody>
</table>
</div>




```python
Q('SELECT bikeid, substr(starttime, 6 , 2) as m, sum(tripduration)/3600 as total from tripdata group by bikeid,m')
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
      <th>bikeid</th>
      <th>m</th>
      <th>total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14529</td>
      <td>05</td>
      <td>34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14529</td>
      <td>06</td>
      <td>21</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14529</td>
      <td>07</td>
      <td>128</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14529</td>
      <td>08</td>
      <td>31</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14529</td>
      <td>09</td>
      <td>31</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14530</td>
      <td>05</td>
      <td>34</td>
    </tr>
    <tr>
      <th>6</th>
      <td>14530</td>
      <td>06</td>
      <td>39</td>
    </tr>
    <tr>
      <th>7</th>
      <td>14530</td>
      <td>07</td>
      <td>30</td>
    </tr>
    <tr>
      <th>8</th>
      <td>14530</td>
      <td>08</td>
      <td>31</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14530</td>
      <td>09</td>
      <td>39</td>
    </tr>
    <tr>
      <th>10</th>
      <td>14531</td>
      <td>05</td>
      <td>35</td>
    </tr>
    <tr>
      <th>11</th>
      <td>14531</td>
      <td>06</td>
      <td>34</td>
    </tr>
    <tr>
      <th>12</th>
      <td>14531</td>
      <td>07</td>
      <td>45</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14531</td>
      <td>08</td>
      <td>32</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14531</td>
      <td>09</td>
      <td>45</td>
    </tr>
    <tr>
      <th>15</th>
      <td>14532</td>
      <td>05</td>
      <td>16</td>
    </tr>
    <tr>
      <th>16</th>
      <td>14532</td>
      <td>06</td>
      <td>40</td>
    </tr>
    <tr>
      <th>17</th>
      <td>14532</td>
      <td>07</td>
      <td>20</td>
    </tr>
    <tr>
      <th>18</th>
      <td>14532</td>
      <td>08</td>
      <td>33</td>
    </tr>
    <tr>
      <th>19</th>
      <td>14532</td>
      <td>09</td>
      <td>31</td>
    </tr>
    <tr>
      <th>20</th>
      <td>14533</td>
      <td>05</td>
      <td>12</td>
    </tr>
    <tr>
      <th>21</th>
      <td>14533</td>
      <td>06</td>
      <td>47</td>
    </tr>
    <tr>
      <th>22</th>
      <td>14533</td>
      <td>07</td>
      <td>48</td>
    </tr>
    <tr>
      <th>23</th>
      <td>14533</td>
      <td>08</td>
      <td>47</td>
    </tr>
    <tr>
      <th>24</th>
      <td>14533</td>
      <td>09</td>
      <td>49</td>
    </tr>
    <tr>
      <th>25</th>
      <td>14534</td>
      <td>05</td>
      <td>30</td>
    </tr>
    <tr>
      <th>26</th>
      <td>14534</td>
      <td>06</td>
      <td>51</td>
    </tr>
    <tr>
      <th>27</th>
      <td>14534</td>
      <td>07</td>
      <td>44</td>
    </tr>
    <tr>
      <th>28</th>
      <td>14534</td>
      <td>08</td>
      <td>12</td>
    </tr>
    <tr>
      <th>29</th>
      <td>14534</td>
      <td>09</td>
      <td>43</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>52849</th>
      <td>31941</td>
      <td>09</td>
      <td>0</td>
    </tr>
    <tr>
      <th>52850</th>
      <td>31942</td>
      <td>09</td>
      <td>3</td>
    </tr>
    <tr>
      <th>52851</th>
      <td>31944</td>
      <td>09</td>
      <td>6</td>
    </tr>
    <tr>
      <th>52852</th>
      <td>31947</td>
      <td>09</td>
      <td>3</td>
    </tr>
    <tr>
      <th>52853</th>
      <td>31950</td>
      <td>09</td>
      <td>5</td>
    </tr>
    <tr>
      <th>52854</th>
      <td>31951</td>
      <td>09</td>
      <td>0</td>
    </tr>
    <tr>
      <th>52855</th>
      <td>31953</td>
      <td>09</td>
      <td>2</td>
    </tr>
    <tr>
      <th>52856</th>
      <td>31955</td>
      <td>09</td>
      <td>5</td>
    </tr>
    <tr>
      <th>52857</th>
      <td>31958</td>
      <td>09</td>
      <td>5</td>
    </tr>
    <tr>
      <th>52858</th>
      <td>31959</td>
      <td>09</td>
      <td>4</td>
    </tr>
    <tr>
      <th>52859</th>
      <td>31960</td>
      <td>09</td>
      <td>3</td>
    </tr>
    <tr>
      <th>52860</th>
      <td>31961</td>
      <td>09</td>
      <td>1</td>
    </tr>
    <tr>
      <th>52861</th>
      <td>31962</td>
      <td>09</td>
      <td>4</td>
    </tr>
    <tr>
      <th>52862</th>
      <td>31963</td>
      <td>09</td>
      <td>4</td>
    </tr>
    <tr>
      <th>52863</th>
      <td>31964</td>
      <td>09</td>
      <td>5</td>
    </tr>
    <tr>
      <th>52864</th>
      <td>31965</td>
      <td>09</td>
      <td>1</td>
    </tr>
    <tr>
      <th>52865</th>
      <td>31966</td>
      <td>09</td>
      <td>3</td>
    </tr>
    <tr>
      <th>52866</th>
      <td>31967</td>
      <td>09</td>
      <td>2</td>
    </tr>
    <tr>
      <th>52867</th>
      <td>31968</td>
      <td>09</td>
      <td>4</td>
    </tr>
    <tr>
      <th>52868</th>
      <td>31969</td>
      <td>09</td>
      <td>2</td>
    </tr>
    <tr>
      <th>52869</th>
      <td>31970</td>
      <td>09</td>
      <td>4</td>
    </tr>
    <tr>
      <th>52870</th>
      <td>31971</td>
      <td>09</td>
      <td>4</td>
    </tr>
    <tr>
      <th>52871</th>
      <td>31972</td>
      <td>09</td>
      <td>2</td>
    </tr>
    <tr>
      <th>52872</th>
      <td>31973</td>
      <td>09</td>
      <td>2</td>
    </tr>
    <tr>
      <th>52873</th>
      <td>31974</td>
      <td>09</td>
      <td>2</td>
    </tr>
    <tr>
      <th>52874</th>
      <td>31975</td>
      <td>09</td>
      <td>5</td>
    </tr>
    <tr>
      <th>52875</th>
      <td>31976</td>
      <td>09</td>
      <td>3</td>
    </tr>
    <tr>
      <th>52876</th>
      <td>31977</td>
      <td>09</td>
      <td>3</td>
    </tr>
    <tr>
      <th>52877</th>
      <td>31978</td>
      <td>09</td>
      <td>0</td>
    </tr>
    <tr>
      <th>52878</th>
      <td>31979</td>
      <td>09</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>52879 rows × 3 columns</p>
</div>




```python
Q('''
SELECT
    bikeid,
    sum(case when m = '05' then total end)as may,
    sum(case when m = '06' then total end)as jun,
    sum(case when m = '07' then total end)as jul,
    sum(case when m = '08' then total end)as aug,
    sum(case when m = '09' then total end)as sep
FROM
(
    SELECT
        bikeid, substr(starttime, 6 , 2) as m,
        sum(tripduration)/3600 as total
    from
        tripdata
    group by
        bikeid,
        m
)
group by bikeid
''')
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
      <th>bikeid</th>
      <th>may</th>
      <th>jun</th>
      <th>jul</th>
      <th>aug</th>
      <th>sep</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>14529</td>
      <td>34.0</td>
      <td>21.0</td>
      <td>128.0</td>
      <td>31.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>14530</td>
      <td>34.0</td>
      <td>39.0</td>
      <td>30.0</td>
      <td>31.0</td>
      <td>39.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14531</td>
      <td>35.0</td>
      <td>34.0</td>
      <td>45.0</td>
      <td>32.0</td>
      <td>45.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14532</td>
      <td>16.0</td>
      <td>40.0</td>
      <td>20.0</td>
      <td>33.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14533</td>
      <td>12.0</td>
      <td>47.0</td>
      <td>48.0</td>
      <td>47.0</td>
      <td>49.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14534</td>
      <td>30.0</td>
      <td>51.0</td>
      <td>44.0</td>
      <td>12.0</td>
      <td>43.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>14535</td>
      <td>42.0</td>
      <td>57.0</td>
      <td>43.0</td>
      <td>43.0</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>14536</td>
      <td>21.0</td>
      <td>68.0</td>
      <td>53.0</td>
      <td>35.0</td>
      <td>25.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>14537</td>
      <td>19.0</td>
      <td>40.0</td>
      <td>42.0</td>
      <td>38.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14539</td>
      <td>38.0</td>
      <td>35.0</td>
      <td>54.0</td>
      <td>51.0</td>
      <td>32.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>14540</td>
      <td>20.0</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>50.0</td>
      <td>37.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>14541</td>
      <td>38.0</td>
      <td>44.0</td>
      <td>40.0</td>
      <td>43.0</td>
      <td>55.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>14544</td>
      <td>49.0</td>
      <td>59.0</td>
      <td>49.0</td>
      <td>27.0</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14545</td>
      <td>70.0</td>
      <td>50.0</td>
      <td>40.0</td>
      <td>56.0</td>
      <td>48.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14547</td>
      <td>30.0</td>
      <td>4.0</td>
      <td>33.0</td>
      <td>57.0</td>
      <td>48.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>14548</td>
      <td>34.0</td>
      <td>56.0</td>
      <td>23.0</td>
      <td>13.0</td>
      <td>39.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>14549</td>
      <td>21.0</td>
      <td>27.0</td>
      <td>56.0</td>
      <td>28.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>14550</td>
      <td>41.0</td>
      <td>23.0</td>
      <td>36.0</td>
      <td>14.0</td>
      <td>29.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>14551</td>
      <td>22.0</td>
      <td>40.0</td>
      <td>311.0</td>
      <td>31.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>14552</td>
      <td>34.0</td>
      <td>23.0</td>
      <td>32.0</td>
      <td>40.0</td>
      <td>37.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>14553</td>
      <td>33.0</td>
      <td>37.0</td>
      <td>44.0</td>
      <td>52.0</td>
      <td>41.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>14555</td>
      <td>52.0</td>
      <td>49.0</td>
      <td>47.0</td>
      <td>6.0</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>14556</td>
      <td>13.0</td>
      <td>24.0</td>
      <td>56.0</td>
      <td>36.0</td>
      <td>29.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>14558</td>
      <td>20.0</td>
      <td>46.0</td>
      <td>11.0</td>
      <td>33.0</td>
      <td>41.0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>14559</td>
      <td>43.0</td>
      <td>48.0</td>
      <td>54.0</td>
      <td>40.0</td>
      <td>33.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>14561</td>
      <td>53.0</td>
      <td>28.0</td>
      <td>38.0</td>
      <td>44.0</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>14562</td>
      <td>21.0</td>
      <td>37.0</td>
      <td>40.0</td>
      <td>29.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>14563</td>
      <td>39.0</td>
      <td>26.0</td>
      <td>18.0</td>
      <td>25.0</td>
      <td>53.0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>14565</td>
      <td>22.0</td>
      <td>27.0</td>
      <td>22.0</td>
      <td>35.0</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>14566</td>
      <td>30.0</td>
      <td>47.0</td>
      <td>44.0</td>
      <td>45.0</td>
      <td>37.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12148</th>
      <td>31941</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>12149</th>
      <td>31942</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>12150</th>
      <td>31944</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>12151</th>
      <td>31947</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>12152</th>
      <td>31950</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>12153</th>
      <td>31951</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>12154</th>
      <td>31953</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>12155</th>
      <td>31955</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>12156</th>
      <td>31958</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>12157</th>
      <td>31959</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>12158</th>
      <td>31960</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>12159</th>
      <td>31961</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>12160</th>
      <td>31962</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>12161</th>
      <td>31963</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>12162</th>
      <td>31964</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>12163</th>
      <td>31965</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>12164</th>
      <td>31966</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>12165</th>
      <td>31967</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>12166</th>
      <td>31968</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>12167</th>
      <td>31969</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>12168</th>
      <td>31970</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>12169</th>
      <td>31971</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>12170</th>
      <td>31972</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>12171</th>
      <td>31973</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>12172</th>
      <td>31974</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>12173</th>
      <td>31975</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>12174</th>
      <td>31976</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>12175</th>
      <td>31977</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>12176</th>
      <td>31978</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>12177</th>
      <td>31979</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
<p>12178 rows × 6 columns</p>
</div>




```python
Q('''
select
    start_station_name,
    sum(case when gender = 1 then 1 else 0 end)as male,
    sum(case when gender = 2 then 2 else 0 end)as female
from tripdata
group by 1

''')
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
      <th>start_station_name</th>
      <th>male</th>
      <th>female</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>193</td>
      <td>200</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1 Ave &amp; E 16 St</td>
      <td>22764</td>
      <td>17910</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1 Ave &amp; E 18 St</td>
      <td>16518</td>
      <td>12152</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1 Ave &amp; E 30 St</td>
      <td>15321</td>
      <td>10304</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1 Ave &amp; E 44 St</td>
      <td>8654</td>
      <td>6836</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1 Ave &amp; E 62 St</td>
      <td>13905</td>
      <td>9716</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1 Ave &amp; E 68 St</td>
      <td>19418</td>
      <td>14474</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1 Ave &amp; E 78 St</td>
      <td>12751</td>
      <td>9924</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1 Ave &amp; E 94 St</td>
      <td>4070</td>
      <td>3406</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1 Pl &amp; Clinton St</td>
      <td>2385</td>
      <td>2538</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10 Hudson Yards</td>
      <td>1569</td>
      <td>1040</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10 St &amp; 5 Ave</td>
      <td>2088</td>
      <td>1952</td>
    </tr>
    <tr>
      <th>12</th>
      <td>10 St &amp; 7 Ave</td>
      <td>3064</td>
      <td>2856</td>
    </tr>
    <tr>
      <th>13</th>
      <td>11 Ave &amp; W 27 St</td>
      <td>20088</td>
      <td>14972</td>
    </tr>
    <tr>
      <th>14</th>
      <td>11 Ave &amp; W 41 St</td>
      <td>17370</td>
      <td>9942</td>
    </tr>
    <tr>
      <th>15</th>
      <td>11 Ave &amp; W 59 St</td>
      <td>16723</td>
      <td>11244</td>
    </tr>
    <tr>
      <th>16</th>
      <td>11 St &amp; 35 Ave</td>
      <td>7</td>
      <td>8</td>
    </tr>
    <tr>
      <th>17</th>
      <td>12 Ave &amp; W 40 St</td>
      <td>26897</td>
      <td>23924</td>
    </tr>
    <tr>
      <th>18</th>
      <td>12 St &amp; 4 Ave</td>
      <td>1490</td>
      <td>1090</td>
    </tr>
    <tr>
      <th>19</th>
      <td>14 St &amp; 5 Ave</td>
      <td>2916</td>
      <td>3016</td>
    </tr>
    <tr>
      <th>20</th>
      <td>14 St &amp; 7 Ave</td>
      <td>3351</td>
      <td>2734</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2 Ave  &amp; E 104 St</td>
      <td>3003</td>
      <td>1946</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2 Ave &amp; 36 St - Citi Bike HQ at Industry City</td>
      <td>993</td>
      <td>930</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2 Ave &amp; 9 St</td>
      <td>1344</td>
      <td>806</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2 Ave &amp; E 122 St</td>
      <td>51</td>
      <td>26</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2 Ave &amp; E 31 St</td>
      <td>14612</td>
      <td>9932</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2 Ave &amp; E 96 St</td>
      <td>6948</td>
      <td>5100</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2 Ave &amp; E 99 St</td>
      <td>1904</td>
      <td>1092</td>
    </tr>
    <tr>
      <th>28</th>
      <td>21 St &amp; 31 Dr</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>29</th>
      <td>21 St &amp; 36 Ave</td>
      <td>10</td>
      <td>4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>680</th>
      <td>W 90 St &amp; Amsterdam Ave</td>
      <td>3919</td>
      <td>3398</td>
    </tr>
    <tr>
      <th>681</th>
      <td>W 92 St &amp; Broadway</td>
      <td>5987</td>
      <td>5006</td>
    </tr>
    <tr>
      <th>682</th>
      <td>W 95 St &amp; Broadway</td>
      <td>7164</td>
      <td>5982</td>
    </tr>
    <tr>
      <th>683</th>
      <td>W Broadway &amp; Spring St</td>
      <td>11765</td>
      <td>10224</td>
    </tr>
    <tr>
      <th>684</th>
      <td>W Broadway &amp; Spring Street</td>
      <td>3381</td>
      <td>3032</td>
    </tr>
    <tr>
      <th>685</th>
      <td>Warren St &amp; Church St</td>
      <td>9341</td>
      <td>6404</td>
    </tr>
    <tr>
      <th>686</th>
      <td>Warren St &amp; Court St</td>
      <td>4011</td>
      <td>3854</td>
    </tr>
    <tr>
      <th>687</th>
      <td>Washington Ave &amp; Greene Ave</td>
      <td>2932</td>
      <td>2790</td>
    </tr>
    <tr>
      <th>688</th>
      <td>Washington Ave &amp; Park Ave</td>
      <td>4085</td>
      <td>3430</td>
    </tr>
    <tr>
      <th>689</th>
      <td>Washington Park</td>
      <td>3616</td>
      <td>3730</td>
    </tr>
    <tr>
      <th>690</th>
      <td>Washington Pl &amp; 6 Ave</td>
      <td>10689</td>
      <td>7972</td>
    </tr>
    <tr>
      <th>691</th>
      <td>Washington Pl &amp; Broadway</td>
      <td>13054</td>
      <td>8016</td>
    </tr>
    <tr>
      <th>692</th>
      <td>Washington St &amp; Gansevoort St</td>
      <td>16815</td>
      <td>15268</td>
    </tr>
    <tr>
      <th>693</th>
      <td>Water - Whitehall Plaza</td>
      <td>6126</td>
      <td>4066</td>
    </tr>
    <tr>
      <th>694</th>
      <td>Watts St &amp; Greenwich St</td>
      <td>13572</td>
      <td>9586</td>
    </tr>
    <tr>
      <th>695</th>
      <td>West Drive &amp; Prospect Park West</td>
      <td>5570</td>
      <td>6770</td>
    </tr>
    <tr>
      <th>696</th>
      <td>West End Ave &amp; W 107 St</td>
      <td>3090</td>
      <td>3104</td>
    </tr>
    <tr>
      <th>697</th>
      <td>West End Ave &amp; W 94 St</td>
      <td>5133</td>
      <td>5016</td>
    </tr>
    <tr>
      <th>698</th>
      <td>West St &amp; Chambers St</td>
      <td>34146</td>
      <td>35194</td>
    </tr>
    <tr>
      <th>699</th>
      <td>West Thames St</td>
      <td>13502</td>
      <td>12856</td>
    </tr>
    <tr>
      <th>700</th>
      <td>William St &amp; Pine St</td>
      <td>7941</td>
      <td>5060</td>
    </tr>
    <tr>
      <th>701</th>
      <td>Willoughby Ave &amp; Hall St</td>
      <td>4714</td>
      <td>5074</td>
    </tr>
    <tr>
      <th>702</th>
      <td>Willoughby Ave &amp; Tompkins Ave</td>
      <td>1423</td>
      <td>1582</td>
    </tr>
    <tr>
      <th>703</th>
      <td>Willoughby Ave &amp; Walworth St</td>
      <td>1854</td>
      <td>1234</td>
    </tr>
    <tr>
      <th>704</th>
      <td>Willoughby St &amp; Fleet St</td>
      <td>7210</td>
      <td>5786</td>
    </tr>
    <tr>
      <th>705</th>
      <td>Wolcott St &amp; Dwight St</td>
      <td>807</td>
      <td>760</td>
    </tr>
    <tr>
      <th>706</th>
      <td>Wyckoff St &amp; 3 Ave</td>
      <td>2285</td>
      <td>2512</td>
    </tr>
    <tr>
      <th>707</th>
      <td>Wythe Ave &amp; Metropolitan Ave</td>
      <td>10631</td>
      <td>10188</td>
    </tr>
    <tr>
      <th>708</th>
      <td>Yankee Ferry Terminal</td>
      <td>2203</td>
      <td>2818</td>
    </tr>
    <tr>
      <th>709</th>
      <td>York St &amp; Jay St</td>
      <td>9932</td>
      <td>9494</td>
    </tr>
  </tbody>
</table>
<p>710 rows × 3 columns</p>
</div>




```python
Q('''
select
    start_station_name,
    100.0 * male /(male+female) as pct_male
from

(
    select
        start_station_name,
        sum(case when gender = 1 then 1 else 0 end)as male,
        sum(case when gender = 2 then 2 else 0 end)as female
    from tripdata
    group by 1
)
''')
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
      <th>start_station_name</th>
      <th>pct_male</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>49.109415</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1 Ave &amp; E 16 St</td>
      <td>55.966957</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1 Ave &amp; E 18 St</td>
      <td>57.614231</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1 Ave &amp; E 30 St</td>
      <td>59.789268</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1 Ave &amp; E 44 St</td>
      <td>55.868302</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1 Ave &amp; E 62 St</td>
      <td>58.867110</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1 Ave &amp; E 68 St</td>
      <td>57.293757</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1 Ave &amp; E 78 St</td>
      <td>56.233738</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1 Ave &amp; E 94 St</td>
      <td>54.440877</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1 Pl &amp; Clinton St</td>
      <td>48.446069</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10 Hudson Yards</td>
      <td>60.137984</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10 St &amp; 5 Ave</td>
      <td>51.683168</td>
    </tr>
    <tr>
      <th>12</th>
      <td>10 St &amp; 7 Ave</td>
      <td>51.756757</td>
    </tr>
    <tr>
      <th>13</th>
      <td>11 Ave &amp; W 27 St</td>
      <td>57.296064</td>
    </tr>
    <tr>
      <th>14</th>
      <td>11 Ave &amp; W 41 St</td>
      <td>63.598418</td>
    </tr>
    <tr>
      <th>15</th>
      <td>11 Ave &amp; W 59 St</td>
      <td>59.795473</td>
    </tr>
    <tr>
      <th>16</th>
      <td>11 St &amp; 35 Ave</td>
      <td>46.666667</td>
    </tr>
    <tr>
      <th>17</th>
      <td>12 Ave &amp; W 40 St</td>
      <td>52.924972</td>
    </tr>
    <tr>
      <th>18</th>
      <td>12 St &amp; 4 Ave</td>
      <td>57.751938</td>
    </tr>
    <tr>
      <th>19</th>
      <td>14 St &amp; 5 Ave</td>
      <td>49.157114</td>
    </tr>
    <tr>
      <th>20</th>
      <td>14 St &amp; 7 Ave</td>
      <td>55.069844</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2 Ave  &amp; E 104 St</td>
      <td>60.678925</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2 Ave &amp; 36 St - Citi Bike HQ at Industry City</td>
      <td>51.638066</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2 Ave &amp; 9 St</td>
      <td>62.511628</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2 Ave &amp; E 122 St</td>
      <td>66.233766</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2 Ave &amp; E 31 St</td>
      <td>59.533898</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2 Ave &amp; E 96 St</td>
      <td>57.669323</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2 Ave &amp; E 99 St</td>
      <td>63.551402</td>
    </tr>
    <tr>
      <th>28</th>
      <td>21 St &amp; 31 Dr</td>
      <td>42.857143</td>
    </tr>
    <tr>
      <th>29</th>
      <td>21 St &amp; 36 Ave</td>
      <td>71.428571</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>680</th>
      <td>W 90 St &amp; Amsterdam Ave</td>
      <td>53.560202</td>
    </tr>
    <tr>
      <th>681</th>
      <td>W 92 St &amp; Broadway</td>
      <td>54.461930</td>
    </tr>
    <tr>
      <th>682</th>
      <td>W 95 St &amp; Broadway</td>
      <td>54.495664</td>
    </tr>
    <tr>
      <th>683</th>
      <td>W Broadway &amp; Spring St</td>
      <td>53.504025</td>
    </tr>
    <tr>
      <th>684</th>
      <td>W Broadway &amp; Spring Street</td>
      <td>52.721035</td>
    </tr>
    <tr>
      <th>685</th>
      <td>Warren St &amp; Church St</td>
      <td>59.326770</td>
    </tr>
    <tr>
      <th>686</th>
      <td>Warren St &amp; Court St</td>
      <td>50.998093</td>
    </tr>
    <tr>
      <th>687</th>
      <td>Washington Ave &amp; Greene Ave</td>
      <td>51.240825</td>
    </tr>
    <tr>
      <th>688</th>
      <td>Washington Ave &amp; Park Ave</td>
      <td>54.357951</td>
    </tr>
    <tr>
      <th>689</th>
      <td>Washington Park</td>
      <td>49.224068</td>
    </tr>
    <tr>
      <th>690</th>
      <td>Washington Pl &amp; 6 Ave</td>
      <td>57.279889</td>
    </tr>
    <tr>
      <th>691</th>
      <td>Washington Pl &amp; Broadway</td>
      <td>61.955387</td>
    </tr>
    <tr>
      <th>692</th>
      <td>Washington St &amp; Gansevoort St</td>
      <td>52.410934</td>
    </tr>
    <tr>
      <th>693</th>
      <td>Water - Whitehall Plaza</td>
      <td>60.105965</td>
    </tr>
    <tr>
      <th>694</th>
      <td>Watts St &amp; Greenwich St</td>
      <td>58.606097</td>
    </tr>
    <tr>
      <th>695</th>
      <td>West Drive &amp; Prospect Park West</td>
      <td>45.137763</td>
    </tr>
    <tr>
      <th>696</th>
      <td>West End Ave &amp; W 107 St</td>
      <td>49.886987</td>
    </tr>
    <tr>
      <th>697</th>
      <td>West End Ave &amp; W 94 St</td>
      <td>50.576411</td>
    </tr>
    <tr>
      <th>698</th>
      <td>West St &amp; Chambers St</td>
      <td>49.244303</td>
    </tr>
    <tr>
      <th>699</th>
      <td>West Thames St</td>
      <td>51.225434</td>
    </tr>
    <tr>
      <th>700</th>
      <td>William St &amp; Pine St</td>
      <td>61.079917</td>
    </tr>
    <tr>
      <th>701</th>
      <td>Willoughby Ave &amp; Hall St</td>
      <td>48.161013</td>
    </tr>
    <tr>
      <th>702</th>
      <td>Willoughby Ave &amp; Tompkins Ave</td>
      <td>47.354409</td>
    </tr>
    <tr>
      <th>703</th>
      <td>Willoughby Ave &amp; Walworth St</td>
      <td>60.038860</td>
    </tr>
    <tr>
      <th>704</th>
      <td>Willoughby St &amp; Fleet St</td>
      <td>55.478609</td>
    </tr>
    <tr>
      <th>705</th>
      <td>Wolcott St &amp; Dwight St</td>
      <td>51.499681</td>
    </tr>
    <tr>
      <th>706</th>
      <td>Wyckoff St &amp; 3 Ave</td>
      <td>47.633938</td>
    </tr>
    <tr>
      <th>707</th>
      <td>Wythe Ave &amp; Metropolitan Ave</td>
      <td>51.063932</td>
    </tr>
    <tr>
      <th>708</th>
      <td>Yankee Ferry Terminal</td>
      <td>43.875722</td>
    </tr>
    <tr>
      <th>709</th>
      <td>York St &amp; Jay St</td>
      <td>51.127355</td>
    </tr>
  </tbody>
</table>
<p>710 rows × 2 columns</p>
</div>




```python
Q('''
    select
        start_station_name,
        round((2020 - birth_year)/5)*5 as age,
        substr(starttime,11,2) as h,
        sum(case when gender = 1 then 1 else 0 end)as male,
        sum(case when gender = 2 then 2 else 0 end)as female
    from tripdata
    group by 1,2,3

''')
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
      <th>start_station_name</th>
      <th>age</th>
      <th>h</th>
      <th>male</th>
      <th>female</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>NaN</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>25.0</td>
      <td>0</td>
      <td>6</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>25.0</td>
      <td>1</td>
      <td>6</td>
      <td>6</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>25.0</td>
      <td>2</td>
      <td>6</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>30.0</td>
      <td>0</td>
      <td>2</td>
      <td>6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>30.0</td>
      <td>1</td>
      <td>10</td>
      <td>40</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>30.0</td>
      <td>2</td>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>35.0</td>
      <td>0</td>
      <td>22</td>
      <td>6</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>35.0</td>
      <td>1</td>
      <td>24</td>
      <td>28</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>35.0</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>40.0</td>
      <td>0</td>
      <td>28</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>40.0</td>
      <td>1</td>
      <td>14</td>
      <td>36</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>40.0</td>
      <td>2</td>
      <td>8</td>
      <td>2</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>45.0</td>
      <td>0</td>
      <td>13</td>
      <td>6</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>45.0</td>
      <td>1</td>
      <td>17</td>
      <td>8</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>45.0</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>50.0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>50.0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>50.0</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>55.0</td>
      <td>0</td>
      <td>1</td>
      <td>32</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>55.0</td>
      <td>1</td>
      <td>9</td>
      <td>16</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>60.0</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>65.0</td>
      <td>0</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>65.0</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>65.0</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>75.0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>75.0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1 Ave &amp; E 16 St</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>27659</th>
      <td>York St &amp; Jay St</td>
      <td>30.0</td>
      <td>1</td>
      <td>1388</td>
      <td>1592</td>
    </tr>
    <tr>
      <th>27660</th>
      <td>York St &amp; Jay St</td>
      <td>30.0</td>
      <td>2</td>
      <td>254</td>
      <td>228</td>
    </tr>
    <tr>
      <th>27661</th>
      <td>York St &amp; Jay St</td>
      <td>35.0</td>
      <td>0</td>
      <td>389</td>
      <td>322</td>
    </tr>
    <tr>
      <th>27662</th>
      <td>York St &amp; Jay St</td>
      <td>35.0</td>
      <td>1</td>
      <td>1755</td>
      <td>1970</td>
    </tr>
    <tr>
      <th>27663</th>
      <td>York St &amp; Jay St</td>
      <td>35.0</td>
      <td>2</td>
      <td>244</td>
      <td>244</td>
    </tr>
    <tr>
      <th>27664</th>
      <td>York St &amp; Jay St</td>
      <td>40.0</td>
      <td>0</td>
      <td>294</td>
      <td>252</td>
    </tr>
    <tr>
      <th>27665</th>
      <td>York St &amp; Jay St</td>
      <td>40.0</td>
      <td>1</td>
      <td>1219</td>
      <td>1190</td>
    </tr>
    <tr>
      <th>27666</th>
      <td>York St &amp; Jay St</td>
      <td>40.0</td>
      <td>2</td>
      <td>199</td>
      <td>126</td>
    </tr>
    <tr>
      <th>27667</th>
      <td>York St &amp; Jay St</td>
      <td>45.0</td>
      <td>0</td>
      <td>220</td>
      <td>192</td>
    </tr>
    <tr>
      <th>27668</th>
      <td>York St &amp; Jay St</td>
      <td>45.0</td>
      <td>1</td>
      <td>805</td>
      <td>552</td>
    </tr>
    <tr>
      <th>27669</th>
      <td>York St &amp; Jay St</td>
      <td>45.0</td>
      <td>2</td>
      <td>119</td>
      <td>72</td>
    </tr>
    <tr>
      <th>27670</th>
      <td>York St &amp; Jay St</td>
      <td>50.0</td>
      <td>0</td>
      <td>105</td>
      <td>162</td>
    </tr>
    <tr>
      <th>27671</th>
      <td>York St &amp; Jay St</td>
      <td>50.0</td>
      <td>1</td>
      <td>846</td>
      <td>342</td>
    </tr>
    <tr>
      <th>27672</th>
      <td>York St &amp; Jay St</td>
      <td>50.0</td>
      <td>2</td>
      <td>115</td>
      <td>36</td>
    </tr>
    <tr>
      <th>27673</th>
      <td>York St &amp; Jay St</td>
      <td>55.0</td>
      <td>0</td>
      <td>94</td>
      <td>60</td>
    </tr>
    <tr>
      <th>27674</th>
      <td>York St &amp; Jay St</td>
      <td>55.0</td>
      <td>1</td>
      <td>437</td>
      <td>254</td>
    </tr>
    <tr>
      <th>27675</th>
      <td>York St &amp; Jay St</td>
      <td>55.0</td>
      <td>2</td>
      <td>66</td>
      <td>56</td>
    </tr>
    <tr>
      <th>27676</th>
      <td>York St &amp; Jay St</td>
      <td>60.0</td>
      <td>0</td>
      <td>21</td>
      <td>36</td>
    </tr>
    <tr>
      <th>27677</th>
      <td>York St &amp; Jay St</td>
      <td>60.0</td>
      <td>1</td>
      <td>176</td>
      <td>262</td>
    </tr>
    <tr>
      <th>27678</th>
      <td>York St &amp; Jay St</td>
      <td>60.0</td>
      <td>2</td>
      <td>32</td>
      <td>20</td>
    </tr>
    <tr>
      <th>27679</th>
      <td>York St &amp; Jay St</td>
      <td>65.0</td>
      <td>0</td>
      <td>32</td>
      <td>22</td>
    </tr>
    <tr>
      <th>27680</th>
      <td>York St &amp; Jay St</td>
      <td>65.0</td>
      <td>1</td>
      <td>99</td>
      <td>114</td>
    </tr>
    <tr>
      <th>27681</th>
      <td>York St &amp; Jay St</td>
      <td>65.0</td>
      <td>2</td>
      <td>11</td>
      <td>10</td>
    </tr>
    <tr>
      <th>27682</th>
      <td>York St &amp; Jay St</td>
      <td>70.0</td>
      <td>0</td>
      <td>6</td>
      <td>6</td>
    </tr>
    <tr>
      <th>27683</th>
      <td>York St &amp; Jay St</td>
      <td>70.0</td>
      <td>1</td>
      <td>56</td>
      <td>46</td>
    </tr>
    <tr>
      <th>27684</th>
      <td>York St &amp; Jay St</td>
      <td>70.0</td>
      <td>2</td>
      <td>4</td>
      <td>6</td>
    </tr>
    <tr>
      <th>27685</th>
      <td>York St &amp; Jay St</td>
      <td>75.0</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27686</th>
      <td>York St &amp; Jay St</td>
      <td>75.0</td>
      <td>1</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>27687</th>
      <td>York St &amp; Jay St</td>
      <td>80.0</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27688</th>
      <td>York St &amp; Jay St</td>
      <td>80.0</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>27689 rows × 5 columns</p>
</div>




```python
res = Q('select count(*) from tripdata')
res
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
      <th>count(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8685057</td>
    </tr>
  </tbody>
</table>
</div>



# Creating a clean dataset


```python
db.execute('''
CREATE TABLE
    tripdata_clean as
SELECT
    (2020- birth_year) as age,
    CASE WHEN gender = 0 then 'X'
         WHEN gender = 1 then 'Male'
         WHEN gender = 2 then 'Female' end as sex,

    *
FROM tripdata
WHERE age > 0
    AND age < 80
    AND tripduration <6000

''')
```




    <sqlite3.Cursor at 0x29822a0af10>




```python
Q('select count(*) from tripdata_clean')
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
      <th>count(*)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7630014</td>
    </tr>
  </tbody>
</table>
</div>



# Plotting:


```python
%pylab inline
```

    Populating the interactive namespace from numpy and matplotlib



```python
import seaborn as sb
```


```python
res = Q('''

select age, count(*) as c from tripdata_clean group by age


''')
```


```python
hist(res.age, weights=res.c, bins= len(res) )
```




    (array([  3091.,  13052.,  22426.,  34788.,  43686.,  60885.,  82500.,
            144783., 224798., 275293., 303208., 328422., 333107., 335369.,
            321919., 319496., 314728., 289934., 272027., 253770., 234885.,
            213225., 195550., 178354., 166852., 164220., 151432., 153265.,
            140317., 137885., 145172., 153601., 142248., 126760., 116005.,
            109145., 112818., 107669., 102578., 100664.,  86778.,  87151.,
             76582.,  68865.,  63716.,  53088.,  44981.,  43182.,  38850.,
             26466.,  23917.,  17193.,  14196.,  12097.,  12677.,   9279.,
              5376.,   4472.,   3260.,   4688.,   3273.]),
     array([19.        , 19.98360656, 20.96721311, 21.95081967, 22.93442623,
            23.91803279, 24.90163934, 25.8852459 , 26.86885246, 27.85245902,
            28.83606557, 29.81967213, 30.80327869, 31.78688525, 32.7704918 ,
            33.75409836, 34.73770492, 35.72131148, 36.70491803, 37.68852459,
            38.67213115, 39.6557377 , 40.63934426, 41.62295082, 42.60655738,
            43.59016393, 44.57377049, 45.55737705, 46.54098361, 47.52459016,
            48.50819672, 49.49180328, 50.47540984, 51.45901639, 52.44262295,
            53.42622951, 54.40983607, 55.39344262, 56.37704918, 57.36065574,
            58.3442623 , 59.32786885, 60.31147541, 61.29508197, 62.27868852,
            63.26229508, 64.24590164, 65.2295082 , 66.21311475, 67.19672131,
            68.18032787, 69.16393443, 70.14754098, 71.13114754, 72.1147541 ,
            73.09836066, 74.08196721, 75.06557377, 76.04918033, 77.03278689,
            78.01639344, 79.        ]),
     <a list of 61 Patch objects>)




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_44_1.png" alt="linearly separable data">



```python
res = Q('''
select
    age,
    sum(case when sex = 'Female' then 1 end) as 'Female',
    sum(case when sex = 'Male' then 1 end) as 'Male'
from
    tripdata_clean
group by 1


''')
```


```python
res.head()
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
      <th>age</th>
      <th>Female</th>
      <th>Male</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19.0</td>
      <td>488</td>
      <td>2520</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20.0</td>
      <td>2245</td>
      <td>10693</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.0</td>
      <td>5102</td>
      <td>17286</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22.0</td>
      <td>7509</td>
      <td>26574</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23.0</td>
      <td>11465</td>
      <td>32083</td>
    </tr>
  </tbody>
</table>
</div>




```python
figure(figsize = (5,12))
barh(res.age, res.Female/res.Female.sum(), color ='r', label = 'Female')
barh(res.age,-res.Male/res.Male.sum(), color='b', label = 'Male')
title('age distribution')
ylabel('age')
grid()
legend()
```




    <matplotlib.legend.Legend at 0x29800abec48>




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_47_1.png" alt="linearly separable data">



```python
res = Q('''
select
    age,
    sum(case when usertype = 'Subscriber' then 1 end ) as Subscriber,
    sum(case when usertype != 'Subscriber' then 1 end ) as Customer
from
    tripdata_clean
group by
    1

''')

res.head()
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
      <th>age</th>
      <th>Subscriber</th>
      <th>Customer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19.0</td>
      <td>3089</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20.0</td>
      <td>13051</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.0</td>
      <td>20178</td>
      <td>2248</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22.0</td>
      <td>30122</td>
      <td>4666</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23.0</td>
      <td>39579</td>
      <td>4107</td>
    </tr>
  </tbody>
</table>
</div>




```python
figure(figsize = (5,12))
barh(res.age, res.Subscriber/res.Subscriber.sum(), color ='y', label = 'Subscriber')
barh(res.age,-res.Customer/res.Customer.sum(), color='g', label = 'Customer')
title('age distribution')
ylabel('age')
grid()
legend()
```




    <matplotlib.legend.Legend at 0x2980f0ca8c8>




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_49_1.png" alt="linearly separable data">



```python
res = Q('''
select
    tripduration / 60 as 'duration',
    sum(case when sex = 'Female' then 1 end) as 'Female',
    sum(case when sex = 'Male' then 1 end) as 'Male'
from
    tripdata_clean
group by
    1
''')

res.head()
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
      <th>duration</th>
      <th>Female</th>
      <th>Male</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>17006</td>
      <td>85222</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>45796</td>
      <td>213363</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>78325</td>
      <td>325656</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>104389</td>
      <td>393120</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>120663</td>
      <td>414420</td>
    </tr>
  </tbody>
</table>
</div>




```python
figure(figsize = (5,12))
barh(res.duration, res.Female/res.Female.sum(), color ='r', label = 'Female')
barh(res.duration,-res.Male/res.Male.sum(), color='b', label = 'Male')
title('duration distribution')
ylabel('duration[in minutes]')
grid()
legend()
ylim(0,50)
```




    (0, 50)




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_51_1.png" alt="linearly separable data">



```python
res = Q('''
select
    start_station_name,
    start_station_latitude as lat,
    start_station_longitude as long,
    count(*) as c
from
    tripdata_clean
group by
    1,2,3

''')
res.head()
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
      <th>start_station_name</th>
      <th>lat</th>
      <th>long</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>40.792327</td>
      <td>-73.938300</td>
      <td>291</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1 Ave &amp; E 16 St</td>
      <td>40.732219</td>
      <td>-73.981656</td>
      <td>31789</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1 Ave &amp; E 18 St</td>
      <td>40.733812</td>
      <td>-73.980544</td>
      <td>22571</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1 Ave &amp; E 30 St</td>
      <td>40.741444</td>
      <td>-73.975361</td>
      <td>20550</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1 Ave &amp; E 44 St</td>
      <td>40.750020</td>
      <td>-73.969053</td>
      <td>12099</td>
    </tr>
  </tbody>
</table>
</div>




```python
hist(res.lat, range=(40.6, 40.9), bins= 100);
```


<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_53_0.png" alt="linearly separable data">



```python
hist(res.long, bins= 100, range = (-74.1,-73.8));
```


<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_54_0.png" alt="linearly separable data">



```python
plot(res.long, res.lat, '.')
xlim(-80,-70)
ylim(40,43)
```




    (40, 43)




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_55_1.png" alt="linearly separable data">



```python
plot(res.long, res.lat, '.')
xlim(-74.5,-73.5)
ylim(40.5,41)
```




    (40.5, 41)




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_56_1.png" alt="linearly separable data">



```python
plot(res.long, res.lat, '.')
xlim(-74.1,-73.8)
ylim(40.6,40.85)
```




    (40.6, 40.85)




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_57_1.png" alt="linearly separable data">



```python
figure(figsize =(10,10))
xstart=-74.1
ystart=40.6
extent=.25
plot(res.long, res.lat, '.')
xlim(xstart,xstart+extent)
ylim(ystart,ystart+extent)
```




    (40.6, 40.85)




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_58_1.png" alt="linearly separable data">



```python
figure(figsize =(10,10))
xstart=-74.05
ystart=40.645
extent=.18
plot(res.long, res.lat, 'o', ms=5)
xlim(xstart,xstart+extent)
ylim(ystart,ystart+extent)
```




    (40.645, 40.825)




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_59_1.png" alt="linearly separable data">



```python
figure(figsize =(10,10))
xstart=-74.05
ystart=40.645
extent=.18
scatter(res.long, res.lat, 35*res.c/res.c.max())
xlim(xstart,xstart+extent)
ylim(ystart,ystart+extent)
```




    (40.645, 40.825)




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_60_1.png" alt="linearly separable data">



```python
figure(figsize =(10,10))
xstart=-74.05
ystart=40.645
extent=.18
scatter(res.long, res.lat, c= 35*res.c/res.c.max(), cmap = 'rainbow')
xlim(xstart,xstart+extent)
ylim(ystart,ystart+extent)
colorbar()
```




    <matplotlib.colorbar.Colorbar at 0x298307f1b88>




<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_61_1.png" alt="linearly separable data">



```python
def my_plot(x,y,s,c,max_size=50):
    figure(figsize =(10,10))
    xstart=-74.05
    ystart=40.645
    extent=.18
    scatter(x, y, s= max_size*s/max(s), c=c, cmap = 'rainbow')
    xlim(xstart,xstart+extent)
    ylim(ystart,ystart+extent)
    colorbar()
```


```python
my_plot(res.long , res.lat,res.c, res.c)
```


<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_63_0.png" alt="linearly separable data">




```python
res = Q('''
select
    start_station_name,
    start_station_latitude as lat,
    start_station_longitude as long,
    avg(tripduration) as d,
    sum(case when sex = 'Female' then 1 end) as 'Female',
    sum(case when sex = 'Male' then 1 end) as 'Male'
from
    tripdata_clean
group by
    1,2,3

''')

res.head()
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
      <th>start_station_name</th>
      <th>lat</th>
      <th>long</th>
      <th>d</th>
      <th>Female</th>
      <th>Male</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1 Ave &amp; E 110 St</td>
      <td>40.792327</td>
      <td>-73.938300</td>
      <td>868.518900</td>
      <td>99.0</td>
      <td>192</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1 Ave &amp; E 16 St</td>
      <td>40.732219</td>
      <td>-73.981656</td>
      <td>665.284658</td>
      <td>8934.0</td>
      <td>22702</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1 Ave &amp; E 18 St</td>
      <td>40.733812</td>
      <td>-73.980544</td>
      <td>678.556112</td>
      <td>6063.0</td>
      <td>16447</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1 Ave &amp; E 30 St</td>
      <td>40.741444</td>
      <td>-73.975361</td>
      <td>734.345791</td>
      <td>5146.0</td>
      <td>15300</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1 Ave &amp; E 44 St</td>
      <td>40.750020</td>
      <td>-73.969053</td>
      <td>857.510621</td>
      <td>3412.0</td>
      <td>8644</td>
    </tr>
  </tbody>
</table>
</div>




```python
my_plot(res.long, res.lat, res.d,100*res.Male/(res.Male+res.Female))
```


<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_65_0.png" alt="linearly separable data">



```python
res = Q('''
select
    start_station_name,
    start_station_latitude as lat,
    start_station_longitude as long,
    avg(case when sex = 'Female' then age end) as 'Female',
    avg(case when sex = 'Male' then age end) as 'Male'
from
    tripdata_clean
group by
    1,2,3

''')

```


```python
my_plot(res.long, res.lat, res.Female*0+1, res.Female)
```


<img src="{{ site.url }}{{ site.baseurl }}/images/Citi/output_67_0.png" alt="linearly separable data">
