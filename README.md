

```python
#Dependencies
import requests as req
import json
# pip install citipy
from citipy import citipy # downloaded from https://pypi.python.org/pypi/citipy
# https://github.com/wingchen/citipy for how to use it and more info about it

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
```


```python
# coordinates: (latitude (horizontal), longitude (vertical))
    #coordinate range: ([-90,90], [-180, 180])
city = citipy.nearest_city(-100, 90).city_name
country = citipy.nearest_city(-100, 90).country_code

# Testing to see if coordinates work 
test = [[city, country]]
test[0]
```




    ['albany', 'au']




```python
# url, api key for openweathermap (OWM)
url = ' http://api.openweathermap.org/data/2.5/weather' #For how to use OWM's api: https://openweathermap.org/current
apiKey = 'c8e2baf63c2571f94bf9d95c94483400' # my requested api key

payload_Test = {'APPID':apiKey, 'units':'imperial', 'q': str(test[0][0]) + ',' + str(test[0][1])} # q doesn't seem to be case sensitive


testResp = req.get(url, params=payload_Test)
weather = testResp.json()

# Testing to see if OpenWeatherMap can find test city and country
weather
```




    {'base': 'stations',
     'clouds': {'all': 20},
     'cod': 200,
     'coord': {'lat': -35.02, 'lon': 117.89},
     'dt': 1512830826,
     'id': 2077963,
     'main': {'grnd_level': 1014,
      'humidity': 50,
      'pressure': 1014,
      'sea_level': 1026.61,
      'temp': 69.95,
      'temp_max': 69.95,
      'temp_min': 69.95},
     'name': 'Albany',
     'sys': {'country': 'AU',
      'message': 0.1706,
      'sunrise': 1512766051,
      'sunset': 1512818081},
     'weather': [{'description': 'few clouds',
       'icon': '02n',
       'id': 801,
       'main': 'Clouds'}],
     'wind': {'deg': 13.5021, 'speed': 1.05}}




```python
# Sample of 500 countries from citipy

#loop to fill the sample list with 600 entries
def getCitySample(number):
    sample_ls = []
    city_ls = []
    country_ls = []
    
    #while sample is less than the sample number
    while len(sample_ls) < number:
        Lati = np.random.uniform(low = -90, high = 90) #random lat and long values to be input into citypy
        Longi = np.random.uniform(low = -180, high = 180)

        #get nearest city name and country code (in case city name is common one)
        city = citipy.nearest_city(Lati, Longi).city_name
        country = citipy.nearest_city(Lati, Longi).country_code

        #check if city name is in the sample list
        duplicate = False
        for entry in sample_ls:
            if city == entry[0] and country == entry [1]:
                duplicate = True
                break
            else:
                continue
        if duplicate == False:
            city_ls.append(city)
            country_ls.append(country)
            sample_ls.append([city, country])

            
    #dataframe of the sample of cities
    columns=['Country', 'Date', 'TimeStamp', 'Cloudiness', 'Humidity', 'Latitude', 'Longitude',\
             'Max Temp', 'Wind Speed (mph)']
    df = pd.DataFrame(data=sample_ls, index=city_ls)
    df = df.reindex(columns=columns) #set blank columns
    df['Country'] = country_ls # add in country abbreviations

    return df

# get sample of cities == 600
citySample_df = getCitySample(600)

citySample_df
    
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Date</th>
      <th>TimeStamp</th>
      <th>Cloudiness</th>
      <th>Humidity</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Max Temp</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>illoqqortoormiut</th>
      <td>gl</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>rikitea</th>
      <td>pf</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>puerto ayora</th>
      <td>ec</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>busselton</th>
      <td>au</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>taolanaro</th>
      <td>mg</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>cabo san lucas</th>
      <td>mx</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>bredasdorp</th>
      <td>za</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>phan rang</th>
      <td>vn</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>anadyr</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>ribeira grande</th>
      <td>pt</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>talnakh</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>tiksi</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>huarmey</th>
      <td>pe</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>sibolga</th>
      <td>id</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>marzuq</th>
      <td>ly</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>harlingen</th>
      <td>nl</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>castro</th>
      <td>cl</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>hermanus</th>
      <td>za</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>samusu</th>
      <td>ws</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>kinablangan</th>
      <td>ph</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>corrente</th>
      <td>br</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>okato</th>
      <td>nz</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>luanda</th>
      <td>ao</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>jamestown</th>
      <td>sh</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>belushya guba</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>san felipe</th>
      <td>mx</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>kitgum</th>
      <td>ug</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>souillac</th>
      <td>mu</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mahebourg</th>
      <td>mu</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>hithadhoo</th>
      <td>mv</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
    </tr>
    <tr>
      <th>dekoa</th>
      <td>cf</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>khargone</th>
      <td>in</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>bose</th>
      <td>cn</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>harper</th>
      <td>lr</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>bonavista</th>
      <td>ca</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>isangel</th>
      <td>vu</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>pula</th>
      <td>hr</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>shumskiy</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>pesaro</th>
      <td>it</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>carballo</th>
      <td>es</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>burriana</th>
      <td>es</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>meteti</th>
      <td>pa</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>pakokku</th>
      <td>mm</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>aktau</th>
      <td>kz</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>springdale</th>
      <td>ca</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>koumac</th>
      <td>nc</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>hanmer springs</th>
      <td>nz</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>tahoua</th>
      <td>ne</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>ahipara</th>
      <td>nz</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>muros</th>
      <td>es</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>zyryanskoye</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>kushalgarh</th>
      <td>in</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>peniche</th>
      <td>pt</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>stephenville</th>
      <td>us</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>staryy urukh</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>faribault</th>
      <td>us</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>lavrentiya</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>erzin</th>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>dire</th>
      <td>ml</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>saint anthony</th>
      <td>ca</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>600 rows × 9 columns</p>
</div>




```python
# url, api key for openweathermap (OWM)
url = ' http://api.openweathermap.org/data/2.5/weather' #For how to use OWM's api: https://openweathermap.org/current
apiKey = 'c8e2baf63c2571f94bf9d95c94483400' # my requested api key

# loop through dataframe to get values for each index
    # If OWM blocks api key, rewrite to limit requests to 50 or 60 requests per minute. 
    #Import time, then time.sleep(60).
def getWeather(df):
    count = 0
    drop_count = 0 # count the number of dropped values 
    for index, row in df.iterrows():
        #print log of each city as it's being processed. 
        print('Processing Record Number ' + str(count+1) + ' | ' + index)
    
        #get weather data based on city and country
        payload_sampl = {'APPID':apiKey, 'units':'imperial', 'q': index + ',' + row['Country']} 
            # q doesn't seem to be case sensitive
        weather = req.get(url, params=payload_sampl)
        weather_json = weather.json()
        try:     
            if weather_json['cod'] == 200: #'cod' seems to equal '404' if city isn't found in OWM. 'cod' == 200 otherwise. 
                df.set_value(index, 'TimeStamp', weather_json['dt'])
                df.set_value(index, 'Latitude', weather_json['coord']['lat'])
                df.set_value(index, 'Longitude', weather_json['coord']['lon'])
                df.set_value(index, 'Max Temp', weather_json['main']['temp_max'])
                df.set_value(index, 'Humidity', weather_json['main']['humidity'])
                df.set_value(index, 'Cloudiness', weather_json['clouds']['all']) #measure of cloudiness?
                df.set_value(index, 'Wind Speed (mph)', weather_json['wind']['speed'])
                print(weather.url)
            if weather_json['cod'] == '404':
                df.drop(index, inplace=True)
                drop_count += 1
                print('City Not Found! Dropping the Row in the DataFrame! | ' +  'Row(s) dropped: '+ str(drop_count))
                print(weather.url)
        except ValueError:
            df.drop(index, inplace=True)
            print('There was a ValueError!')
            continue
        
        count += 1
    print('\n' + str(len(df)) + ' total rows.')
    print('\n' + str(drop_count) + ' row(s) have been dropped due to absence in the OWM api.')
    
    return(df)

#get weather for citySample_df
citySample_df = getWeather(citySample_df)
citySample_df
```

    Processing Record Number 1 | illoqqortoormiut
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 1
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=illoqqortoormiut%2Cgl
    Processing Record Number 2 | rikitea
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=rikitea%2Cpf
    Processing Record Number 3 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=puerto+ayora%2Cec
    Processing Record Number 4 | busselton
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=busselton%2Cau
    Processing Record Number 5 | taolanaro
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 2
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=taolanaro%2Cmg
    Processing Record Number 6 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cabo+san+lucas%2Cmx
    Processing Record Number 7 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bredasdorp%2Cza
    Processing Record Number 8 | phan rang
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 3
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=phan+rang%2Cvn
    Processing Record Number 9 | anadyr
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=anadyr%2Cru
    Processing Record Number 10 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ribeira+grande%2Cpt
    Processing Record Number 11 | talnakh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=talnakh%2Cru
    Processing Record Number 12 | tiksi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tiksi%2Cru
    Processing Record Number 13 | huarmey
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=huarmey%2Cpe
    Processing Record Number 14 | sibolga
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sibolga%2Cid
    Processing Record Number 15 | marzuq
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 4
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=marzuq%2Cly
    Processing Record Number 16 | harlingen
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=harlingen%2Cnl
    Processing Record Number 17 | castro
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=castro%2Ccl
    Processing Record Number 18 | hermanus
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hermanus%2Cza
    Processing Record Number 19 | samusu
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 5
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=samusu%2Cws
    Processing Record Number 20 | kinablangan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kinablangan%2Cph
    Processing Record Number 21 | corrente
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=corrente%2Cbr
    Processing Record Number 22 | okato
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=okato%2Cnz
    Processing Record Number 23 | luanda
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=luanda%2Cao
    Processing Record Number 24 | jamestown
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=jamestown%2Csh
    Processing Record Number 25 | belushya guba
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 6
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=belushya+guba%2Cru
    Processing Record Number 26 | san felipe
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=san+felipe%2Cmx
    Processing Record Number 27 | kitgum
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kitgum%2Cug
    Processing Record Number 28 | souillac
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=souillac%2Cmu
    Processing Record Number 29 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mahebourg%2Cmu
    Processing Record Number 30 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hithadhoo%2Cmv
    Processing Record Number 31 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ushuaia%2Car
    Processing Record Number 32 | hobart
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hobart%2Cau
    Processing Record Number 33 | hvolsvollur
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 7
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hvolsvollur%2Cis
    Processing Record Number 34 | kahului
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kahului%2Cus
    Processing Record Number 35 | cape town
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cape+town%2Cza
    Processing Record Number 36 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saint-philippe%2Cre
    Processing Record Number 37 | vaitupu
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 8
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vaitupu%2Cwf
    Processing Record Number 38 | vaini
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vaini%2Cto
    Processing Record Number 39 | mataura
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 9
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mataura%2Cpf
    Processing Record Number 40 | palaia fokaia
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=palaia+fokaia%2Cgr
    Processing Record Number 41 | katsuura
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=katsuura%2Cjp
    Processing Record Number 42 | hambantota
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hambantota%2Clk
    Processing Record Number 43 | bur gabo
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 10
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bur+gabo%2Cso
    Processing Record Number 44 | kapaa
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kapaa%2Cus
    Processing Record Number 45 | dikson
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dikson%2Cru
    Processing Record Number 46 | san patricio
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=san+patricio%2Cmx
    Processing Record Number 47 | sindor
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sindor%2Cru
    Processing Record Number 48 | cayenne
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cayenne%2Cgf
    Processing Record Number 49 | tautira
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tautira%2Cpf
    Processing Record Number 50 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=qaanaaq%2Cgl
    Processing Record Number 51 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=punta+arenas%2Ccl
    Processing Record Number 52 | angoche
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 11
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=angoche%2Cmz
    Processing Record Number 53 | birkeland
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=birkeland%2Cno
    Processing Record Number 54 | gambela
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=gambela%2Cet
    Processing Record Number 55 | tsihombe
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 12
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tsihombe%2Cmg
    Processing Record Number 56 | melito di napoli
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=melito+di+napoli%2Cit
    Processing Record Number 57 | chernyshevskiy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chernyshevskiy%2Cru
    Processing Record Number 58 | nurota
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nurota%2Cuz
    Processing Record Number 59 | tura
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tura%2Cru
    Processing Record Number 60 | lidkoping
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lidkoping%2Cse
    Processing Record Number 61 | sentyabrskiy
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 13
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sentyabrskiy%2Cru
    Processing Record Number 62 | comodoro rivadavia
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=comodoro+rivadavia%2Car
    Processing Record Number 63 | prince rupert
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=prince+rupert%2Cca
    Processing Record Number 64 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ilulissat%2Cgl
    Processing Record Number 65 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=arraial+do+cabo%2Cbr
    Processing Record Number 66 | buariki
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 14
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=buariki%2Cki
    Processing Record Number 67 | hasaki
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hasaki%2Cjp
    Processing Record Number 68 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mar+del+plata%2Car
    Processing Record Number 69 | faya
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 15
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=faya%2Ctd
    Processing Record Number 70 | jurmala
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=jurmala%2Clv
    Processing Record Number 71 | griffith
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=griffith%2Cau
    Processing Record Number 72 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=new+norfolk%2Cau
    Processing Record Number 73 | koungou
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=koungou%2Cyt
    Processing Record Number 74 | avarua
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=avarua%2Cck
    Processing Record Number 75 | sturgeon falls
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 16
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sturgeon+falls%2Cca
    Processing Record Number 76 | adrar
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=adrar%2Cdz
    Processing Record Number 77 | barentsburg
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 17
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=barentsburg%2Csj
    Processing Record Number 78 | kirkuk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kirkuk%2Ciq
    Processing Record Number 79 | gombong
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=gombong%2Cid
    Processing Record Number 80 | pisco
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pisco%2Cpe
    Processing Record Number 81 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vila+franca+do+campo%2Cpt
    Processing Record Number 82 | atuona
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=atuona%2Cpf
    Processing Record Number 83 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bengkulu%2Cid
    Processing Record Number 84 | faanui
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=faanui%2Cpf
    Processing Record Number 85 | amderma
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 18
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=amderma%2Cru
    Processing Record Number 86 | yarmouth
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=yarmouth%2Cca
    Processing Record Number 87 | barbar
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 19
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=barbar%2Csd
    Processing Record Number 88 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saskylakh%2Cru
    Processing Record Number 89 | albany
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=albany%2Cau
    Processing Record Number 90 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=severo-kurilsk%2Cru
    Processing Record Number 91 | bestobe
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bestobe%2Ckz
    Processing Record Number 92 | paamiut
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=paamiut%2Cgl
    Processing Record Number 93 | mosquera
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mosquera%2Cco
    Processing Record Number 94 | cashel
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cashel%2Cie
    Processing Record Number 95 | novikovo
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 20
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=novikovo%2Cru
    Processing Record Number 96 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pangnirtung%2Cca
    Processing Record Number 97 | hilo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hilo%2Cus
    Processing Record Number 98 | kishi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kishi%2Cng
    Processing Record Number 99 | porto walter
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=porto+walter%2Cbr
    Processing Record Number 100 | tazmalt
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tazmalt%2Cdz
    Processing Record Number 101 | mao
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mao%2Ctd
    Processing Record Number 102 | bethel
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bethel%2Cus
    Processing Record Number 103 | georgetown
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=georgetown%2Csh
    Processing Record Number 104 | luba
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=luba%2Cgq
    Processing Record Number 105 | briceni
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=briceni%2Cmd
    Processing Record Number 106 | mount isa
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mount+isa%2Cau
    Processing Record Number 107 | gat
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 21
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=gat%2Cly
    Processing Record Number 108 | itarema
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 22
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=itarema%2Cbr
    Processing Record Number 109 | kupang
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kupang%2Cid
    Processing Record Number 110 | kaitangata
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 23
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kaitangata%2Cnz
    Processing Record Number 111 | codrington
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 24
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=codrington%2Cag
    Processing Record Number 112 | antofagasta
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=antofagasta%2Ccl
    Processing Record Number 113 | sitka
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sitka%2Cus
    Processing Record Number 114 | acandi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=acandi%2Cco
    Processing Record Number 115 | vostok
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vostok%2Cru
    Processing Record Number 116 | khandbari
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=khandbari%2Cnp
    Processing Record Number 117 | yulara
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=yulara%2Cau
    Processing Record Number 118 | asau
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 25
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=asau%2Ctv
    Processing Record Number 119 | parkes
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=parkes%2Cau
    Processing Record Number 120 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=coquimbo%2Ccl
    Processing Record Number 121 | araouane
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=araouane%2Cml
    Processing Record Number 122 | podhum
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=podhum%2Cba
    Processing Record Number 123 | victoria
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=victoria%2Csc
    Processing Record Number 124 | baruun-urt
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=baruun-urt%2Cmn
    Processing Record Number 125 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cherskiy%2Cru
    Processing Record Number 126 | kaeo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kaeo%2Cnz
    Processing Record Number 127 | mwanza
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mwanza%2Cmw
    Processing Record Number 128 | hofn
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hofn%2Cis
    Processing Record Number 129 | doctor pedro p. pena
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 26
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=doctor+pedro+p.+pena%2Cpy
    Processing Record Number 130 | chuy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chuy%2Cuy
    Processing Record Number 131 | viedma
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=viedma%2Car
    Processing Record Number 132 | kampene
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kampene%2Ccd
    Processing Record Number 133 | nanakuli
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nanakuli%2Cus
    Processing Record Number 134 | ferme-neuve
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ferme-neuve%2Cca
    Processing Record Number 135 | acarau
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=acarau%2Cbr
    Processing Record Number 136 | bluff
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bluff%2Cnz
    Processing Record Number 137 | soyo
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 27
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=soyo%2Cao
    Processing Record Number 138 | port alfred
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=port+alfred%2Cza
    Processing Record Number 139 | clyde river
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=clyde+river%2Cca
    Processing Record Number 140 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=fairbanks%2Cus
    Processing Record Number 141 | luwuk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=luwuk%2Cid
    Processing Record Number 142 | banswara
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=banswara%2Cin
    Processing Record Number 143 | kavieng
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kavieng%2Cpg
    Processing Record Number 144 | matara
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=matara%2Clk
    Processing Record Number 145 | ibra
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ibra%2Com
    Processing Record Number 146 | college
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=college%2Cus
    Processing Record Number 147 | kieta
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kieta%2Cpg
    Processing Record Number 148 | keti bandar
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=keti+bandar%2Cpk
    Processing Record Number 149 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ostrovnoy%2Cru
    Processing Record Number 150 | el balyana
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 28
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=el+balyana%2Ceg
    Processing Record Number 151 | nizhneudinsk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nizhneudinsk%2Cru
    Processing Record Number 152 | ixtapa
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ixtapa%2Cmx
    Processing Record Number 153 | cabedelo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cabedelo%2Cbr
    Processing Record Number 154 | kandrian
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kandrian%2Cpg
    Processing Record Number 155 | severomuysk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=severomuysk%2Cru
    Processing Record Number 156 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=yellowknife%2Cca
    Processing Record Number 157 | kodiak
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kodiak%2Cus
    Processing Record Number 158 | gvarv
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=gvarv%2Cno
    Processing Record Number 159 | pochutla
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 29
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pochutla%2Cmx
    Processing Record Number 160 | yashalta
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=yashalta%2Cru
    Processing Record Number 161 | taksimo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=taksimo%2Cru
    Processing Record Number 162 | ostersund
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ostersund%2Cse
    Processing Record Number 163 | lagoa
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lagoa%2Cpt
    Processing Record Number 164 | caravelas
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=caravelas%2Cbr
    Processing Record Number 165 | vanimo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vanimo%2Cpg
    Processing Record Number 166 | rivadavia
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=rivadavia%2Car
    Processing Record Number 167 | kamenskoye
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 30
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kamenskoye%2Cru
    Processing Record Number 168 | estevan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=estevan%2Cca
    Processing Record Number 169 | manoharpur
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=manoharpur%2Cin
    Processing Record Number 170 | flinders
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=flinders%2Cau
    Processing Record Number 171 | chichicapam
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 31
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chichicapam%2Cmx
    Processing Record Number 172 | abu samrah
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 32
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=abu+samrah%2Cqa
    Processing Record Number 173 | waipawa
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=waipawa%2Cnz
    Processing Record Number 174 | aykhal
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=aykhal%2Cru
    Processing Record Number 175 | maykor
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=maykor%2Cru
    Processing Record Number 176 | bolungarvik
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 33
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bolungarvik%2Cis
    Processing Record Number 177 | dwarka
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dwarka%2Cin
    Processing Record Number 178 | umm lajj
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=umm+lajj%2Csa
    Processing Record Number 179 | huntington
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=huntington%2Cus
    Processing Record Number 180 | benjamin constant
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=benjamin+constant%2Cbr
    Processing Record Number 181 | tiznit
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tiznit%2Cma
    Processing Record Number 182 | cam ranh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cam+ranh%2Cvn
    Processing Record Number 183 | barrow
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=barrow%2Cus
    Processing Record Number 184 | natal
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=natal%2Cbr
    Processing Record Number 185 | marcona
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 34
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=marcona%2Cpe
    Processing Record Number 186 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=okhotsk%2Cru
    Processing Record Number 187 | sivaki
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sivaki%2Cru
    Processing Record Number 188 | twentynine palms
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=twentynine+palms%2Cus
    Processing Record Number 189 | lima
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lima%2Cpe
    Processing Record Number 190 | karpathos
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=karpathos%2Cgr
    Processing Record Number 191 | dennery
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dennery%2Clc
    Processing Record Number 192 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chokurdakh%2Cru
    Processing Record Number 193 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tuatapere%2Cnz
    Processing Record Number 194 | ardon
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ardon%2Cru
    Processing Record Number 195 | kuhestan
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 35
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kuhestan%2Caf
    Processing Record Number 196 | sao jose da coroa grande
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sao+jose+da+coroa+grande%2Cbr
    Processing Record Number 197 | raudeberg
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 36
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=raudeberg%2Cno
    Processing Record Number 198 | thompson
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=thompson%2Cca
    Processing Record Number 199 | kutum
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kutum%2Csd
    Processing Record Number 200 | hamilton
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hamilton%2Cbm
    Processing Record Number 201 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=yar-sale%2Cru
    Processing Record Number 202 | labuhan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=labuhan%2Cid
    Processing Record Number 203 | tsagan aman
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tsagan+aman%2Cru
    Processing Record Number 204 | sembakung
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sembakung%2Cid
    Processing Record Number 205 | belfast
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=belfast%2Cza
    Processing Record Number 206 | provideniya
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=provideniya%2Cru
    Processing Record Number 207 | saint george
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saint+george%2Cbm
    Processing Record Number 208 | kuche
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 37
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kuche%2Ccn
    Processing Record Number 209 | lensk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lensk%2Cru
    Processing Record Number 210 | aquiraz
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=aquiraz%2Cbr
    Processing Record Number 211 | teahupoo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=teahupoo%2Cpf
    Processing Record Number 212 | margate
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=margate%2Cza
    Processing Record Number 213 | lasa
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 38
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lasa%2Ccn
    Processing Record Number 214 | byron bay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=byron+bay%2Cau
    Processing Record Number 215 | mehamn
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mehamn%2Cno
    Processing Record Number 216 | naze
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=naze%2Cjp
    Processing Record Number 217 | mwene-ditu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mwene-ditu%2Ccd
    Processing Record Number 218 | sibu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sibu%2Cmy
    Processing Record Number 219 | hovd
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hovd%2Cmn
    Processing Record Number 220 | coahuayana
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=coahuayana%2Cmx
    Processing Record Number 221 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tuktoyaktuk%2Cca
    Processing Record Number 222 | angra
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 39
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=angra%2Cpt
    Processing Record Number 223 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tasiilaq%2Cgl
    Processing Record Number 224 | eskisehir
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=eskisehir%2Ctr
    Processing Record Number 225 | east london
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=east+london%2Cza
    Processing Record Number 226 | qasigiannguit
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=qasigiannguit%2Cgl
    Processing Record Number 227 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kavaratti%2Cin
    Processing Record Number 228 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mount+gambier%2Cau
    Processing Record Number 229 | ulaangom
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ulaangom%2Cmn
    Processing Record Number 230 | tomatlan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tomatlan%2Cmx
    Processing Record Number 231 | sukhinichi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sukhinichi%2Cru
    Processing Record Number 232 | oussouye
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=oussouye%2Csn
    Processing Record Number 233 | carutapera
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=carutapera%2Cbr
    Processing Record Number 234 | sobolevo
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 40
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sobolevo%2Cru
    Processing Record Number 235 | alofi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=alofi%2Cnu
    Processing Record Number 236 | shakhtinsk
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 41
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=shakhtinsk%2Ckz
    Processing Record Number 237 | amga
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=amga%2Cru
    Processing Record Number 238 | sal rei
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sal+rei%2Ccv
    Processing Record Number 239 | khatanga
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=khatanga%2Cru
    Processing Record Number 240 | kogon
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kogon%2Cuz
    Processing Record Number 241 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sinnamary%2Cgf
    Processing Record Number 242 | palabuhanratu
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 42
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=palabuhanratu%2Cid
    Processing Record Number 243 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bambous+virieux%2Cmu
    Processing Record Number 244 | whitehorse
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=whitehorse%2Cca
    Processing Record Number 245 | geraldton
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=geraldton%2Cau
    Processing Record Number 246 | katherine
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=katherine%2Cau
    Processing Record Number 247 | kurilsk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kurilsk%2Cru
    Processing Record Number 248 | kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kudahuvadhoo%2Cmv
    Processing Record Number 249 | hervey bay
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 43
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hervey+bay%2Cau
    Processing Record Number 250 | martapura
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=martapura%2Cid
    Processing Record Number 251 | cairo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cairo%2Ceg
    Processing Record Number 252 | vredefort
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vredefort%2Cza
    Processing Record Number 253 | chambas
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chambas%2Ccu
    Processing Record Number 254 | onokhoy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=onokhoy%2Cru
    Processing Record Number 255 | ndele
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ndele%2Ccf
    Processing Record Number 256 | parnarama
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=parnarama%2Cbr
    Processing Record Number 257 | vao
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vao%2Cnc
    Processing Record Number 258 | mutsu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mutsu%2Cjp
    Processing Record Number 259 | dandeli
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dandeli%2Cin
    Processing Record Number 260 | tezu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tezu%2Cin
    Processing Record Number 261 | warri
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=warri%2Cng
    Processing Record Number 262 | saint-joseph
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saint-joseph%2Cre
    Processing Record Number 263 | opobo
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 44
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=opobo%2Cng
    Processing Record Number 264 | urdzhar
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 45
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=urdzhar%2Ckz
    Processing Record Number 265 | dunedin
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dunedin%2Cnz
    Processing Record Number 266 | broken hill
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=broken+hill%2Cau
    Processing Record Number 267 | camacupa
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=camacupa%2Cao
    Processing Record Number 268 | feodosiya
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=feodosiya%2Cua
    Processing Record Number 269 | kulykiv
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 46
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kulykiv%2Cua
    Processing Record Number 270 | sao goncalo do sapucai
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sao+goncalo+do+sapucai%2Cbr
    Processing Record Number 271 | jamame
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 47
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=jamame%2Cso
    Processing Record Number 272 | te anau
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=te+anau%2Cnz
    Processing Record Number 273 | port blair
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=port+blair%2Cin
    Processing Record Number 274 | trastenik
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 48
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=trastenik%2Cbg
    Processing Record Number 275 | chabua
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chabua%2Cin
    Processing Record Number 276 | chabahar
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 49
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chabahar%2Cir
    Processing Record Number 277 | attawapiskat
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 50
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=attawapiskat%2Cca
    Processing Record Number 278 | taree
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=taree%2Cau
    Processing Record Number 279 | lokosovo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lokosovo%2Cru
    Processing Record Number 280 | pasni
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pasni%2Cpk
    Processing Record Number 281 | kabo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kabo%2Ccf
    Processing Record Number 282 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=los+llanos+de+aridane%2Ces
    Processing Record Number 283 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sao+joao+da+barra%2Cbr
    Processing Record Number 284 | taua
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 51
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=taua%2Cbr
    Processing Record Number 285 | lompoc
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lompoc%2Cus
    Processing Record Number 286 | pontes e lacerda
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pontes+e+lacerda%2Cbr
    Processing Record Number 287 | russell
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 52
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=russell%2Cnz
    Processing Record Number 288 | lebu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lebu%2Ccl
    Processing Record Number 289 | bundibugyo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bundibugyo%2Cug
    Processing Record Number 290 | esperance
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=esperance%2Cau
    Processing Record Number 291 | aswan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=aswan%2Ceg
    Processing Record Number 292 | weihai
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=weihai%2Ccn
    Processing Record Number 293 | mecca
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mecca%2Csa
    Processing Record Number 294 | hildburghausen
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hildburghausen%2Cde
    Processing Record Number 295 | saleaula
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 53
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saleaula%2Cws
    Processing Record Number 296 | pionerskiy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pionerskiy%2Cru
    Processing Record Number 297 | syamzha
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=syamzha%2Cru
    Processing Record Number 298 | buritis
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=buritis%2Cbr
    Processing Record Number 299 | timra
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=timra%2Cse
    Processing Record Number 300 | fortuna
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=fortuna%2Cus
    Processing Record Number 301 | skagastrond
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 54
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=skagastrond%2Cis
    Processing Record Number 302 | roebourne
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=roebourne%2Cau
    Processing Record Number 303 | novobirilyussy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=novobirilyussy%2Cru
    Processing Record Number 304 | beidao
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=beidao%2Ccn
    Processing Record Number 305 | kalmunai
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kalmunai%2Clk
    Processing Record Number 306 | medvode
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=medvode%2Csi
    Processing Record Number 307 | morondava
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=morondava%2Cmg
    Processing Record Number 308 | sheridan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sheridan%2Cus
    Processing Record Number 309 | saint-pierre
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saint-pierre%2Cpm
    Processing Record Number 310 | saldanha
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saldanha%2Cza
    Processing Record Number 311 | tiarei
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tiarei%2Cpf
    Processing Record Number 312 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=klaksvik%2Cfo
    Processing Record Number 313 | finschhafen
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=finschhafen%2Cpg
    Processing Record Number 314 | erdaojiang
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=erdaojiang%2Ccn
    Processing Record Number 315 | cairns
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cairns%2Cau
    Processing Record Number 316 | igrim
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=igrim%2Cru
    Processing Record Number 317 | pakxan
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 55
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pakxan%2Cla
    Processing Record Number 318 | talcahuano
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=talcahuano%2Ccl
    Processing Record Number 319 | paita
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=paita%2Cpe
    Processing Record Number 320 | rassvet
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=rassvet%2Cru
    Processing Record Number 321 | sarab
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 56
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sarab%2Cir
    Processing Record Number 322 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=half+moon+bay%2Cus
    Processing Record Number 323 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sao+filipe%2Ccv
    Processing Record Number 324 | liverpool
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=liverpool%2Cca
    Processing Record Number 325 | denpasar
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=denpasar%2Cid
    Processing Record Number 326 | necochea
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=necochea%2Car
    Processing Record Number 327 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=port+elizabeth%2Cza
    Processing Record Number 328 | miyang
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=miyang%2Ccn
    Processing Record Number 329 | butaritari
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=butaritari%2Cki
    Processing Record Number 330 | vanderhoof
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vanderhoof%2Cca
    Processing Record Number 331 | ati
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ati%2Ctd
    Processing Record Number 332 | cururupu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cururupu%2Cbr
    Processing Record Number 333 | fez
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 57
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=fez%2Cma
    Processing Record Number 334 | dera bugti
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dera+bugti%2Cpk
    Processing Record Number 335 | enderby
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=enderby%2Cca
    Processing Record Number 336 | mys shmidta
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 58
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mys+shmidta%2Cru
    Processing Record Number 337 | altay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=altay%2Ccn
    Processing Record Number 338 | mackenzie
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mackenzie%2Cca
    Processing Record Number 339 | makakilo city
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=makakilo+city%2Cus
    Processing Record Number 340 | banda aceh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=banda+aceh%2Cid
    Processing Record Number 341 | husavik
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=husavik%2Cis
    Processing Record Number 342 | puerto escondido
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=puerto+escondido%2Cmx
    Processing Record Number 343 | vardo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vardo%2Cno
    Processing Record Number 344 | lakes entrance
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lakes+entrance%2Cau
    Processing Record Number 345 | deer lake
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=deer+lake%2Cca
    Processing Record Number 346 | gopalpur
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=gopalpur%2Cin
    Processing Record Number 347 | ornskoldsvik
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ornskoldsvik%2Cse
    Processing Record Number 348 | sao desiderio
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sao+desiderio%2Cbr
    Processing Record Number 349 | marsh harbour
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=marsh+harbour%2Cbs
    Processing Record Number 350 | kaputa
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kaputa%2Czm
    Processing Record Number 351 | haibowan
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 59
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=haibowan%2Ccn
    Processing Record Number 352 | riyadh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=riyadh%2Csa
    Processing Record Number 353 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pacific+grove%2Cus
    Processing Record Number 354 | sadiqabad
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sadiqabad%2Cpk
    Processing Record Number 355 | contamana
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=contamana%2Cpe
    Processing Record Number 356 | waycross
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=waycross%2Cus
    Processing Record Number 357 | pevek
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pevek%2Cru
    Processing Record Number 358 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sisimiut%2Cgl
    Processing Record Number 359 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kruisfontein%2Cza
    Processing Record Number 360 | vila velha
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vila+velha%2Cbr
    Processing Record Number 361 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=longyearbyen%2Csj
    Processing Record Number 362 | luderitz
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=luderitz%2Cna
    Processing Record Number 363 | la macarena
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=la+macarena%2Cco
    Processing Record Number 364 | dakar
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dakar%2Csn
    Processing Record Number 365 | lewisporte
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lewisporte%2Cca
    Processing Record Number 366 | plouzane
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=plouzane%2Cfr
    Processing Record Number 367 | general pico
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=general+pico%2Car
    Processing Record Number 368 | rafaela
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=rafaela%2Car
    Processing Record Number 369 | tynda
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tynda%2Cru
    Processing Record Number 370 | tam ky
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tam+ky%2Cvn
    Processing Record Number 371 | salalah
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=salalah%2Com
    Processing Record Number 372 | empangeni
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=empangeni%2Cza
    Processing Record Number 373 | clarence town
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=clarence+town%2Cbs
    Processing Record Number 374 | sola
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sola%2Cvu
    Processing Record Number 375 | dibaya
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 60
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dibaya%2Ccd
    Processing Record Number 376 | lengshuitan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lengshuitan%2Ccn
    Processing Record Number 377 | puerto rico
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=puerto+rico%2Cco
    Processing Record Number 378 | goderich
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 61
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=goderich%2Csl
    Processing Record Number 379 | abha
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=abha%2Csa
    Processing Record Number 380 | airai
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 62
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=airai%2Cpw
    Processing Record Number 381 | oktyabrskoye
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=oktyabrskoye%2Cru
    Processing Record Number 382 | tumannyy
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 63
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tumannyy%2Cru
    Processing Record Number 383 | primero de enero
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=primero+de+enero%2Ccu
    Processing Record Number 384 | dong hoi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dong+hoi%2Cvn
    Processing Record Number 385 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ponta+do+sol%2Ccv
    Processing Record Number 386 | kuytun
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 64
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kuytun%2Ccn
    Processing Record Number 387 | dillon
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dillon%2Cus
    Processing Record Number 388 | alugan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=alugan%2Cph
    Processing Record Number 389 | cidreira
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=cidreira%2Cbr
    Processing Record Number 390 | miraflores
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=miraflores%2Cco
    Processing Record Number 391 | upernavik
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=upernavik%2Cgl
    Processing Record Number 392 | nova soure
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nova+soure%2Cbr
    Processing Record Number 393 | gazanjyk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=gazanjyk%2Ctm
    Processing Record Number 394 | kaniama
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kaniama%2Ccd
    Processing Record Number 395 | portland
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=portland%2Cau
    Processing Record Number 396 | ancud
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ancud%2Ccl
    Processing Record Number 397 | the valley
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=the+valley%2Cai
    Processing Record Number 398 | lumut
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lumut%2Cmy
    Processing Record Number 399 | hede
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hede%2Ccn
    Processing Record Number 400 | along
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=along%2Cin
    Processing Record Number 401 | san borja
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=san+borja%2Cbo
    Processing Record Number 402 | kismayo
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 65
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kismayo%2Cso
    Processing Record Number 403 | humaita
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=humaita%2Cbr
    Processing Record Number 404 | nemuro
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nemuro%2Cjp
    Processing Record Number 405 | sevlievo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sevlievo%2Cbg
    Processing Record Number 406 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=beringovskiy%2Cru
    Processing Record Number 407 | shingu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=shingu%2Cjp
    Processing Record Number 408 | dehloran
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dehloran%2Cir
    Processing Record Number 409 | grand river south east
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 66
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=grand+river+south+east%2Cmu
    Processing Record Number 410 | burgeo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=burgeo%2Cca
    Processing Record Number 411 | bhandaria
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bhandaria%2Cbd
    Processing Record Number 412 | georgetown
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=georgetown%2Cgy
    Processing Record Number 413 | broome
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=broome%2Cau
    Processing Record Number 414 | villamartin
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=villamartin%2Ces
    Processing Record Number 415 | matipo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=matipo%2Cbr
    Processing Record Number 416 | high level
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=high+level%2Cca
    Processing Record Number 417 | neiafu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=neiafu%2Cto
    Processing Record Number 418 | grand gaube
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=grand+gaube%2Cmu
    Processing Record Number 419 | port hedland
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=port+hedland%2Cau
    Processing Record Number 420 | quelimane
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=quelimane%2Cmz
    Processing Record Number 421 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=san+cristobal%2Cec
    Processing Record Number 422 | nikolskoye
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 67
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nikolskoye%2Cru
    Processing Record Number 423 | yeppoon
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=yeppoon%2Cau
    Processing Record Number 424 | qaqortoq
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=qaqortoq%2Cgl
    Processing Record Number 425 | hualmay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hualmay%2Cpe
    Processing Record Number 426 | yichun
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=yichun%2Ccn
    Processing Record Number 427 | karkaralinsk
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 68
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=karkaralinsk%2Ckz
    Processing Record Number 428 | mikhaylovka
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 69
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mikhaylovka%2Ckz
    Processing Record Number 429 | milingimbi
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 70
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=milingimbi%2Cau
    Processing Record Number 430 | kochki
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kochki%2Cru
    Processing Record Number 431 | norman wells
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=norman+wells%2Cca
    Processing Record Number 432 | presidente medici
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=presidente+medici%2Cbr
    Processing Record Number 433 | san vicente
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=san+vicente%2Cph
    Processing Record Number 434 | berberati
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=berberati%2Ccf
    Processing Record Number 435 | ahumada
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 71
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ahumada%2Cmx
    Processing Record Number 436 | laguna
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=laguna%2Cbr
    Processing Record Number 437 | balkanabat
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=balkanabat%2Ctm
    Processing Record Number 438 | danville
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=danville%2Cus
    Processing Record Number 439 | jeremie
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=jeremie%2Cht
    Processing Record Number 440 | boyolangu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=boyolangu%2Cid
    Processing Record Number 441 | chiredzi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chiredzi%2Czw
    Processing Record Number 442 | sujiatun
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sujiatun%2Ccn
    Processing Record Number 443 | chuzhou
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chuzhou%2Ccn
    Processing Record Number 444 | chibuto
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chibuto%2Cmz
    Processing Record Number 445 | areosa
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=areosa%2Cpt
    Processing Record Number 446 | abu dhabi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=abu+dhabi%2Cae
    Processing Record Number 447 | evensk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=evensk%2Cru
    Processing Record Number 448 | nelson bay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nelson+bay%2Cau
    Processing Record Number 449 | waddan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=waddan%2Cly
    Processing Record Number 450 | maningrida
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=maningrida%2Cau
    Processing Record Number 451 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=guerrero+negro%2Cmx
    Processing Record Number 452 | sol-iletsk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sol-iletsk%2Cru
    Processing Record Number 453 | haines junction
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=haines+junction%2Cca
    Processing Record Number 454 | tadine
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tadine%2Cnc
    Processing Record Number 455 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=carnarvon%2Cau
    Processing Record Number 456 | vancouver
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vancouver%2Cca
    Processing Record Number 457 | morro bay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=morro+bay%2Cus
    Processing Record Number 458 | camana
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=camana%2Cpe
    Processing Record Number 459 | qui nhon
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 72
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=qui+nhon%2Cvn
    Processing Record Number 460 | monrovia
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=monrovia%2Clr
    Processing Record Number 461 | labutta
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 73
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=labutta%2Cmm
    Processing Record Number 462 | inuvik
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=inuvik%2Cca
    Processing Record Number 463 | snezhnogorsk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=snezhnogorsk%2Cru
    Processing Record Number 464 | qurayyat
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 74
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=qurayyat%2Com
    Processing Record Number 465 | savonlinna
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=savonlinna%2Cfi
    Processing Record Number 466 | lorengau
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lorengau%2Cpg
    Processing Record Number 467 | port hardy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=port+hardy%2Cca
    Processing Record Number 468 | namatanai
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=namatanai%2Cpg
    Processing Record Number 469 | tromso
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tromso%2Cno
    Processing Record Number 470 | bulgan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bulgan%2Cmn
    Processing Record Number 471 | bodden town
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bodden+town%2Cky
    Processing Record Number 472 | boksitogorsk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=boksitogorsk%2Cru
    Processing Record Number 473 | mendahara
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 75
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mendahara%2Cid
    Processing Record Number 474 | alotau
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 76
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=alotau%2Cpg
    Processing Record Number 475 | la ronge
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=la+ronge%2Cca
    Processing Record Number 476 | constitucion
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 77
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=constitucion%2Cmx
    Processing Record Number 477 | lithakia
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lithakia%2Cgr
    Processing Record Number 478 | nabire
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nabire%2Cid
    Processing Record Number 479 | touros
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=touros%2Cbr
    Processing Record Number 480 | trollhattan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=trollhattan%2Cse
    Processing Record Number 481 | shanghai
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=shanghai%2Ccn
    Processing Record Number 482 | severo-yeniseyskiy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=severo-yeniseyskiy%2Cru
    Processing Record Number 483 | noumea
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=noumea%2Cnc
    Processing Record Number 484 | ilo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ilo%2Cpe
    Processing Record Number 485 | safwah
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 78
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=safwah%2Csa
    Processing Record Number 486 | padang
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=padang%2Cid
    Processing Record Number 487 | jawhar
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=jawhar%2Cso
    Processing Record Number 488 | huanren
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=huanren%2Ccn
    Processing Record Number 489 | coihaique
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=coihaique%2Ccl
    Processing Record Number 490 | pietersburg
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 79
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pietersburg%2Cza
    Processing Record Number 491 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=egvekinot%2Cru
    Processing Record Number 492 | villanueva
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=villanueva%2Cco
    Processing Record Number 493 | chimbote
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=chimbote%2Cpe
    Processing Record Number 494 | obuasi
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=obuasi%2Cgh
    Processing Record Number 495 | dalinghe
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 80
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dalinghe%2Ccn
    Processing Record Number 496 | richards bay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=richards+bay%2Cza
    Processing Record Number 497 | xam nua
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=xam+nua%2Cla
    Processing Record Number 498 | brae
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=brae%2Cgb
    Processing Record Number 499 | bujaru
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bujaru%2Cbr
    Processing Record Number 500 | george town
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=george+town%2Cky
    Processing Record Number 501 | bulungu
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bulungu%2Ccd
    Processing Record Number 502 | marsa matruh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=marsa+matruh%2Ceg
    Processing Record Number 503 | mazagao
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mazagao%2Cbr
    Processing Record Number 504 | krasnoselkup
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 81
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=krasnoselkup%2Cru
    Processing Record Number 505 | michigan city
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=michigan+city%2Cus
    Processing Record Number 506 | baft
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 82
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=baft%2Cir
    Processing Record Number 507 | bilma
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bilma%2Cne
    Processing Record Number 508 | abong mbang
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=abong+mbang%2Ccm
    Processing Record Number 509 | ngunguru
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ngunguru%2Cnz
    Processing Record Number 510 | nizhneyansk
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 83
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nizhneyansk%2Cru
    Processing Record Number 511 | aripuana
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=aripuana%2Cbr
    Processing Record Number 512 | kamenka
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kamenka%2Cru
    Processing Record Number 513 | mocambique
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 84
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mocambique%2Cmz
    Processing Record Number 514 | mogocha
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mogocha%2Cru
    Processing Record Number 515 | vila nova de milfontes
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=vila+nova+de+milfontes%2Cpt
    Processing Record Number 516 | nyurba
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nyurba%2Cru
    Processing Record Number 517 | balgazyn
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=balgazyn%2Cru
    Processing Record Number 518 | san martin
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=san+martin%2Car
    Processing Record Number 519 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=meulaboh%2Cid
    Processing Record Number 520 | ghauspur
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ghauspur%2Cpk
    Processing Record Number 521 | paradwip
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 85
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=paradwip%2Cin
    Processing Record Number 522 | akron
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=akron%2Cus
    Processing Record Number 523 | kisangani
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kisangani%2Ccd
    Processing Record Number 524 | kirakira
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kirakira%2Csb
    Processing Record Number 525 | santo domingo
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 86
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=santo+domingo%2Cec
    Processing Record Number 526 | bereda
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 87
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bereda%2Cso
    Processing Record Number 527 | caraguatay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=caraguatay%2Cpy
    Processing Record Number 528 | fare
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=fare%2Cpf
    Processing Record Number 529 | totolapan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=totolapan%2Cmx
    Processing Record Number 530 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nanortalik%2Cgl
    Processing Record Number 531 | barreirinhas
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=barreirinhas%2Cbr
    Processing Record Number 532 | ishim
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ishim%2Cru
    Processing Record Number 533 | belyy yar
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=belyy+yar%2Cru
    Processing Record Number 534 | sao gabriel da cachoeira
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sao+gabriel+da+cachoeira%2Cbr
    Processing Record Number 535 | mayumba
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mayumba%2Cga
    Processing Record Number 536 | nisia floresta
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nisia+floresta%2Cbr
    Processing Record Number 537 | alice springs
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=alice+springs%2Cau
    Processing Record Number 538 | wanning
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=wanning%2Ccn
    Processing Record Number 539 | sur
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sur%2Com
    Processing Record Number 540 | torbay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=torbay%2Cca
    Processing Record Number 541 | kodinsk
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kodinsk%2Cru
    Processing Record Number 542 | flic en flac
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=flic+en+flac%2Cmu
    Processing Record Number 543 | olafsvik
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 88
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=olafsvik%2Cis
    Processing Record Number 544 | mwinilunga
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mwinilunga%2Czm
    Processing Record Number 545 | talaja
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=talaja%2Cin
    Processing Record Number 546 | taltal
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=taltal%2Ccl
    Processing Record Number 547 | wuwei
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=wuwei%2Ccn
    Processing Record Number 548 | jinchang
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=jinchang%2Ccn
    Processing Record Number 549 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nouadhibou%2Cmr
    Processing Record Number 550 | henties bay
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 89
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=henties+bay%2Cna
    Processing Record Number 551 | severnyy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=severnyy%2Cru
    Processing Record Number 552 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saint-augustin%2Cca
    Processing Record Number 553 | ust-kamchatsk
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 90
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ust-kamchatsk%2Cru
    Processing Record Number 554 | lichtenburg
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lichtenburg%2Cza
    Processing Record Number 555 | mazatlan
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mazatlan%2Cmx
    Processing Record Number 556 | ossora
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ossora%2Cru
    Processing Record Number 557 | casablanca
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=casablanca%2Cma
    Processing Record Number 558 | nieves
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=nieves%2Cmx
    Processing Record Number 559 | sangar
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=sangar%2Cru
    Processing Record Number 560 | tucumcari
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tucumcari%2Cus
    Processing Record Number 561 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=iqaluit%2Cca
    Processing Record Number 562 | shasta lake
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=shasta+lake%2Cus
    Processing Record Number 563 | batemans bay
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=batemans+bay%2Cau
    Processing Record Number 564 | saint-georges
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 91
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saint-georges%2Cgf
    Processing Record Number 565 | mercedes
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=mercedes%2Car
    Processing Record Number 566 | zheleznodorozhnyy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=zheleznodorozhnyy%2Cru
    Processing Record Number 567 | micheweni
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=micheweni%2Ctz
    Processing Record Number 568 | linhares
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=linhares%2Cbr
    Processing Record Number 569 | northam
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=northam%2Cau
    Processing Record Number 570 | la asuncion
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=la+asuncion%2Cve
    Processing Record Number 571 | dekoa
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 92
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dekoa%2Ccf
    Processing Record Number 572 | khargone
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 93
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=khargone%2Cin
    Processing Record Number 573 | bose
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 94
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bose%2Ccn
    Processing Record Number 574 | harper
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=harper%2Clr
    Processing Record Number 575 | bonavista
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=bonavista%2Cca
    Processing Record Number 576 | isangel
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=isangel%2Cvu
    Processing Record Number 577 | pula
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pula%2Chr
    Processing Record Number 578 | shumskiy
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=shumskiy%2Cru
    Processing Record Number 579 | pesaro
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pesaro%2Cit
    Processing Record Number 580 | carballo
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=carballo%2Ces
    Processing Record Number 581 | burriana
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=burriana%2Ces
    Processing Record Number 582 | meteti
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=meteti%2Cpa
    Processing Record Number 583 | pakokku
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=pakokku%2Cmm
    Processing Record Number 584 | aktau
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=aktau%2Ckz
    Processing Record Number 585 | springdale
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=springdale%2Cca
    Processing Record Number 586 | koumac
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=koumac%2Cnc
    Processing Record Number 587 | hanmer springs
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=hanmer+springs%2Cnz
    Processing Record Number 588 | tahoua
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=tahoua%2Cne
    Processing Record Number 589 | ahipara
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=ahipara%2Cnz
    Processing Record Number 590 | muros
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=muros%2Ces
    Processing Record Number 591 | zyryanskoye
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=zyryanskoye%2Cru
    Processing Record Number 592 | kushalgarh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=kushalgarh%2Cin
    Processing Record Number 593 | peniche
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=peniche%2Cpt
    Processing Record Number 594 | stephenville
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=stephenville%2Cus
    Processing Record Number 595 | staryy urukh
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=staryy+urukh%2Cru
    Processing Record Number 596 | faribault
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=faribault%2Cus
    Processing Record Number 597 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=lavrentiya%2Cru
    Processing Record Number 598 | erzin
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=erzin%2Cru
    Processing Record Number 599 | dire
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=dire%2Cml
    Processing Record Number 600 | saint anthony
    City Not Found! Dropping the Row in the DataFrame! | Row(s) dropped: 95
    http://api.openweathermap.org/data/2.5/weather?APPID=c8e2baf63c2571f94bf9d95c94483400&units=imperial&q=saint+anthony%2Cca
    
    505 total rows.
    
    95 row(s) have been dropped due to absence in the OWM api.
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Date</th>
      <th>TimeStamp</th>
      <th>Cloudiness</th>
      <th>Humidity</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Max Temp</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>rikitea</th>
      <td>pf</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>36.0</td>
      <td>99.0</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>78.23</td>
      <td>5.97</td>
    </tr>
    <tr>
      <th>puerto ayora</th>
      <td>ec</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>75.0</td>
      <td>83.0</td>
      <td>-0.74</td>
      <td>-90.35</td>
      <td>73.40</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>busselton</th>
      <td>au</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>87.0</td>
      <td>-33.65</td>
      <td>115.33</td>
      <td>72.02</td>
      <td>12.01</td>
    </tr>
    <tr>
      <th>cabo san lucas</th>
      <td>mx</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>32.0</td>
      <td>100.0</td>
      <td>22.89</td>
      <td>-109.91</td>
      <td>76.16</td>
      <td>15.82</td>
    </tr>
    <tr>
      <th>bredasdorp</th>
      <td>za</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>0.0</td>
      <td>49.0</td>
      <td>-34.53</td>
      <td>20.04</td>
      <td>71.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>anadyr</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>90.0</td>
      <td>84.0</td>
      <td>64.75</td>
      <td>177.48</td>
      <td>8.60</td>
      <td>4.47</td>
    </tr>
    <tr>
      <th>ribeira grande</th>
      <td>pt</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>20.0</td>
      <td>83.0</td>
      <td>38.52</td>
      <td>-28.70</td>
      <td>69.80</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>talnakh</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>80.0</td>
      <td>77.0</td>
      <td>69.49</td>
      <td>88.40</td>
      <td>7.31</td>
      <td>18.28</td>
    </tr>
    <tr>
      <th>tiksi</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>44.0</td>
      <td>67.0</td>
      <td>71.69</td>
      <td>128.87</td>
      <td>-14.74</td>
      <td>3.85</td>
    </tr>
    <tr>
      <th>huarmey</th>
      <td>pe</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>36.0</td>
      <td>77.0</td>
      <td>-10.07</td>
      <td>-78.15</td>
      <td>68.42</td>
      <td>3.40</td>
    </tr>
    <tr>
      <th>sibolga</th>
      <td>id</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>92.0</td>
      <td>100.0</td>
      <td>1.74</td>
      <td>98.78</td>
      <td>67.66</td>
      <td>1.28</td>
    </tr>
    <tr>
      <th>harlingen</th>
      <td>nl</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>90.0</td>
      <td>86.0</td>
      <td>53.17</td>
      <td>5.42</td>
      <td>41.00</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>castro</th>
      <td>cl</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>88.0</td>
      <td>97.0</td>
      <td>-42.47</td>
      <td>-73.77</td>
      <td>54.47</td>
      <td>6.87</td>
    </tr>
    <tr>
      <th>hermanus</th>
      <td>za</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>53.0</td>
      <td>-34.42</td>
      <td>19.23</td>
      <td>69.32</td>
      <td>12.80</td>
    </tr>
    <tr>
      <th>kinablangan</th>
      <td>ph</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>88.0</td>
      <td>100.0</td>
      <td>7.69</td>
      <td>126.55</td>
      <td>81.61</td>
      <td>1.28</td>
    </tr>
    <tr>
      <th>corrente</th>
      <td>br</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>57.0</td>
      <td>-3.74</td>
      <td>-43.34</td>
      <td>91.19</td>
      <td>11.01</td>
    </tr>
    <tr>
      <th>okato</th>
      <td>nz</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>88.0</td>
      <td>96.0</td>
      <td>-39.20</td>
      <td>173.88</td>
      <td>65.50</td>
      <td>13.13</td>
    </tr>
    <tr>
      <th>luanda</th>
      <td>ao</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>75.0</td>
      <td>62.0</td>
      <td>-8.84</td>
      <td>13.23</td>
      <td>86.00</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>jamestown</th>
      <td>sh</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>48.0</td>
      <td>100.0</td>
      <td>-15.94</td>
      <td>-5.72</td>
      <td>68.83</td>
      <td>11.90</td>
    </tr>
    <tr>
      <th>san felipe</th>
      <td>mx</td>
      <td>NaN</td>
      <td>1.512827e+09</td>
      <td>40.0</td>
      <td>35.0</td>
      <td>21.48</td>
      <td>-101.22</td>
      <td>41.00</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>kitgum</th>
      <td>ug</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>28.0</td>
      <td>3.28</td>
      <td>32.89</td>
      <td>87.95</td>
      <td>5.64</td>
    </tr>
    <tr>
      <th>souillac</th>
      <td>mu</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>40.0</td>
      <td>69.0</td>
      <td>-20.52</td>
      <td>57.52</td>
      <td>80.60</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>mahebourg</th>
      <td>mu</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>40.0</td>
      <td>69.0</td>
      <td>-20.41</td>
      <td>57.70</td>
      <td>80.60</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>hithadhoo</th>
      <td>mv</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>100.0</td>
      <td>-0.60</td>
      <td>73.08</td>
      <td>81.65</td>
      <td>8.55</td>
    </tr>
    <tr>
      <th>ushuaia</th>
      <td>ar</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>75.0</td>
      <td>70.0</td>
      <td>-54.80</td>
      <td>-68.30</td>
      <td>42.80</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>hobart</th>
      <td>au</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>40.0</td>
      <td>54.0</td>
      <td>-42.88</td>
      <td>147.33</td>
      <td>59.00</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>kahului</th>
      <td>us</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>1.0</td>
      <td>88.0</td>
      <td>20.89</td>
      <td>-156.47</td>
      <td>64.40</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>cape town</th>
      <td>za</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>0.0</td>
      <td>49.0</td>
      <td>-33.93</td>
      <td>18.42</td>
      <td>71.60</td>
      <td>35.57</td>
    </tr>
    <tr>
      <th>saint-philippe</th>
      <td>re</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>20.0</td>
      <td>65.0</td>
      <td>-21.36</td>
      <td>55.77</td>
      <td>80.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>vaini</th>
      <td>to</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>20.0</td>
      <td>88.0</td>
      <td>-21.20</td>
      <td>-175.20</td>
      <td>75.20</td>
      <td>21.52</td>
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
    </tr>
    <tr>
      <th>micheweni</th>
      <td>tz</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>44.0</td>
      <td>100.0</td>
      <td>-4.97</td>
      <td>39.83</td>
      <td>83.18</td>
      <td>19.06</td>
    </tr>
    <tr>
      <th>linhares</th>
      <td>br</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>36.0</td>
      <td>95.0</td>
      <td>-19.39</td>
      <td>-40.07</td>
      <td>84.76</td>
      <td>4.97</td>
    </tr>
    <tr>
      <th>northam</th>
      <td>au</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>24.0</td>
      <td>56.0</td>
      <td>-31.67</td>
      <td>116.67</td>
      <td>73.40</td>
      <td>1.12</td>
    </tr>
    <tr>
      <th>la asuncion</th>
      <td>ve</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>36.0</td>
      <td>100.0</td>
      <td>11.03</td>
      <td>-63.86</td>
      <td>78.95</td>
      <td>14.47</td>
    </tr>
    <tr>
      <th>harper</th>
      <td>lr</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>100.0</td>
      <td>4.38</td>
      <td>-7.72</td>
      <td>76.52</td>
      <td>14.25</td>
    </tr>
    <tr>
      <th>bonavista</th>
      <td>ca</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>88.0</td>
      <td>98.0</td>
      <td>48.65</td>
      <td>-53.11</td>
      <td>35.53</td>
      <td>11.45</td>
    </tr>
    <tr>
      <th>isangel</th>
      <td>vu</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>80.0</td>
      <td>100.0</td>
      <td>-19.55</td>
      <td>169.27</td>
      <td>77.51</td>
      <td>11.12</td>
    </tr>
    <tr>
      <th>pula</th>
      <td>hr</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>20.0</td>
      <td>56.0</td>
      <td>44.87</td>
      <td>13.85</td>
      <td>42.80</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>shumskiy</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>76.0</td>
      <td>82.0</td>
      <td>54.83</td>
      <td>99.13</td>
      <td>13.25</td>
      <td>2.62</td>
    </tr>
    <tr>
      <th>pesaro</th>
      <td>it</td>
      <td>NaN</td>
      <td>1.512829e+09</td>
      <td>40.0</td>
      <td>100.0</td>
      <td>43.90</td>
      <td>12.89</td>
      <td>44.60</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>carballo</th>
      <td>es</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>75.0</td>
      <td>82.0</td>
      <td>43.21</td>
      <td>-8.69</td>
      <td>60.80</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>burriana</th>
      <td>es</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>20.0</td>
      <td>44.0</td>
      <td>39.89</td>
      <td>-0.09</td>
      <td>60.80</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>meteti</th>
      <td>pa</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>80.0</td>
      <td>100.0</td>
      <td>8.50</td>
      <td>-77.98</td>
      <td>76.93</td>
      <td>3.62</td>
    </tr>
    <tr>
      <th>pakokku</th>
      <td>mm</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>92.0</td>
      <td>100.0</td>
      <td>21.33</td>
      <td>95.10</td>
      <td>69.50</td>
      <td>6.87</td>
    </tr>
    <tr>
      <th>aktau</th>
      <td>kz</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>20.0</td>
      <td>92.0</td>
      <td>43.65</td>
      <td>51.20</td>
      <td>24.80</td>
      <td>6.71</td>
    </tr>
    <tr>
      <th>springdale</th>
      <td>ca</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>93.0</td>
      <td>49.50</td>
      <td>-56.06</td>
      <td>33.68</td>
      <td>13.80</td>
    </tr>
    <tr>
      <th>koumac</th>
      <td>nc</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>12.0</td>
      <td>95.0</td>
      <td>-20.57</td>
      <td>164.28</td>
      <td>73.15</td>
      <td>5.97</td>
    </tr>
    <tr>
      <th>hanmer springs</th>
      <td>nz</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>66.0</td>
      <td>-42.52</td>
      <td>172.82</td>
      <td>62.21</td>
      <td>10.67</td>
    </tr>
    <tr>
      <th>tahoua</th>
      <td>ne</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>0.0</td>
      <td>18.0</td>
      <td>14.89</td>
      <td>5.27</td>
      <td>84.20</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>ahipara</th>
      <td>nz</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>64.0</td>
      <td>100.0</td>
      <td>-35.17</td>
      <td>173.17</td>
      <td>61.13</td>
      <td>4.63</td>
    </tr>
    <tr>
      <th>muros</th>
      <td>es</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>90.0</td>
      <td>100.0</td>
      <td>42.78</td>
      <td>-9.06</td>
      <td>55.40</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>zyryanskoye</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>77.0</td>
      <td>56.83</td>
      <td>86.63</td>
      <td>14.83</td>
      <td>11.68</td>
    </tr>
    <tr>
      <th>kushalgarh</th>
      <td>in</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>32.0</td>
      <td>64.0</td>
      <td>23.20</td>
      <td>74.45</td>
      <td>70.67</td>
      <td>4.52</td>
    </tr>
    <tr>
      <th>peniche</th>
      <td>pt</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>40.0</td>
      <td>82.0</td>
      <td>39.36</td>
      <td>-9.38</td>
      <td>62.60</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>stephenville</th>
      <td>us</td>
      <td>NaN</td>
      <td>1.512829e+09</td>
      <td>1.0</td>
      <td>55.0</td>
      <td>32.22</td>
      <td>-98.20</td>
      <td>42.80</td>
      <td>14.47</td>
    </tr>
    <tr>
      <th>staryy urukh</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>0.0</td>
      <td>75.0</td>
      <td>43.34</td>
      <td>44.02</td>
      <td>41.00</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>faribault</th>
      <td>us</td>
      <td>NaN</td>
      <td>1.512829e+09</td>
      <td>90.0</td>
      <td>73.0</td>
      <td>44.30</td>
      <td>-93.27</td>
      <td>21.20</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>lavrentiya</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>100.0</td>
      <td>65.58</td>
      <td>-171.00</td>
      <td>23.33</td>
      <td>24.09</td>
    </tr>
    <tr>
      <th>erzin</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>53.0</td>
      <td>50.26</td>
      <td>95.16</td>
      <td>-13.57</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>dire</th>
      <td>ml</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>22.0</td>
      <td>12.28</td>
      <td>-10.97</td>
      <td>92.27</td>
      <td>8.32</td>
    </tr>
  </tbody>
</table>
<p>505 rows × 9 columns</p>
</div>




```python
# adding to df if length of citySample_df < 500
def addCitiesTo(number, df):
    while len(df) < number:
        addCities_df = getCitySample(number-len(df))
        addCities_df = getWeather(addCities_df)
    
        # append addCities_df to citySample_df
        df = df.append(addCities_df)
        print ('\n' + 'Total rows in dataframe: ' + str(len(df)) + '\n')
    return df

# add cities to citySample_df
citySample_df = addCitiesTo(500, citySample_df)
citySample_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Date</th>
      <th>TimeStamp</th>
      <th>Cloudiness</th>
      <th>Humidity</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Max Temp</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>rikitea</th>
      <td>pf</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>36.0</td>
      <td>99.0</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>78.23</td>
      <td>5.97</td>
    </tr>
    <tr>
      <th>puerto ayora</th>
      <td>ec</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>75.0</td>
      <td>83.0</td>
      <td>-0.74</td>
      <td>-90.35</td>
      <td>73.40</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>busselton</th>
      <td>au</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>87.0</td>
      <td>-33.65</td>
      <td>115.33</td>
      <td>72.02</td>
      <td>12.01</td>
    </tr>
    <tr>
      <th>cabo san lucas</th>
      <td>mx</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>32.0</td>
      <td>100.0</td>
      <td>22.89</td>
      <td>-109.91</td>
      <td>76.16</td>
      <td>15.82</td>
    </tr>
    <tr>
      <th>bredasdorp</th>
      <td>za</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>0.0</td>
      <td>49.0</td>
      <td>-34.53</td>
      <td>20.04</td>
      <td>71.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>anadyr</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>90.0</td>
      <td>84.0</td>
      <td>64.75</td>
      <td>177.48</td>
      <td>8.60</td>
      <td>4.47</td>
    </tr>
    <tr>
      <th>ribeira grande</th>
      <td>pt</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>20.0</td>
      <td>83.0</td>
      <td>38.52</td>
      <td>-28.70</td>
      <td>69.80</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>talnakh</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>80.0</td>
      <td>77.0</td>
      <td>69.49</td>
      <td>88.40</td>
      <td>7.31</td>
      <td>18.28</td>
    </tr>
    <tr>
      <th>tiksi</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>44.0</td>
      <td>67.0</td>
      <td>71.69</td>
      <td>128.87</td>
      <td>-14.74</td>
      <td>3.85</td>
    </tr>
    <tr>
      <th>huarmey</th>
      <td>pe</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>36.0</td>
      <td>77.0</td>
      <td>-10.07</td>
      <td>-78.15</td>
      <td>68.42</td>
      <td>3.40</td>
    </tr>
    <tr>
      <th>sibolga</th>
      <td>id</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>92.0</td>
      <td>100.0</td>
      <td>1.74</td>
      <td>98.78</td>
      <td>67.66</td>
      <td>1.28</td>
    </tr>
    <tr>
      <th>harlingen</th>
      <td>nl</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>90.0</td>
      <td>86.0</td>
      <td>53.17</td>
      <td>5.42</td>
      <td>41.00</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>castro</th>
      <td>cl</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>88.0</td>
      <td>97.0</td>
      <td>-42.47</td>
      <td>-73.77</td>
      <td>54.47</td>
      <td>6.87</td>
    </tr>
    <tr>
      <th>hermanus</th>
      <td>za</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>53.0</td>
      <td>-34.42</td>
      <td>19.23</td>
      <td>69.32</td>
      <td>12.80</td>
    </tr>
    <tr>
      <th>kinablangan</th>
      <td>ph</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>88.0</td>
      <td>100.0</td>
      <td>7.69</td>
      <td>126.55</td>
      <td>81.61</td>
      <td>1.28</td>
    </tr>
    <tr>
      <th>corrente</th>
      <td>br</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>57.0</td>
      <td>-3.74</td>
      <td>-43.34</td>
      <td>91.19</td>
      <td>11.01</td>
    </tr>
    <tr>
      <th>okato</th>
      <td>nz</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>88.0</td>
      <td>96.0</td>
      <td>-39.20</td>
      <td>173.88</td>
      <td>65.50</td>
      <td>13.13</td>
    </tr>
    <tr>
      <th>luanda</th>
      <td>ao</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>75.0</td>
      <td>62.0</td>
      <td>-8.84</td>
      <td>13.23</td>
      <td>86.00</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>jamestown</th>
      <td>sh</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>48.0</td>
      <td>100.0</td>
      <td>-15.94</td>
      <td>-5.72</td>
      <td>68.83</td>
      <td>11.90</td>
    </tr>
    <tr>
      <th>san felipe</th>
      <td>mx</td>
      <td>NaN</td>
      <td>1.512827e+09</td>
      <td>40.0</td>
      <td>35.0</td>
      <td>21.48</td>
      <td>-101.22</td>
      <td>41.00</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>kitgum</th>
      <td>ug</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>28.0</td>
      <td>3.28</td>
      <td>32.89</td>
      <td>87.95</td>
      <td>5.64</td>
    </tr>
    <tr>
      <th>souillac</th>
      <td>mu</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>40.0</td>
      <td>69.0</td>
      <td>-20.52</td>
      <td>57.52</td>
      <td>80.60</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>mahebourg</th>
      <td>mu</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>40.0</td>
      <td>69.0</td>
      <td>-20.41</td>
      <td>57.70</td>
      <td>80.60</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>hithadhoo</th>
      <td>mv</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>100.0</td>
      <td>-0.60</td>
      <td>73.08</td>
      <td>81.65</td>
      <td>8.55</td>
    </tr>
    <tr>
      <th>ushuaia</th>
      <td>ar</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>75.0</td>
      <td>70.0</td>
      <td>-54.80</td>
      <td>-68.30</td>
      <td>42.80</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>hobart</th>
      <td>au</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>40.0</td>
      <td>54.0</td>
      <td>-42.88</td>
      <td>147.33</td>
      <td>59.00</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>kahului</th>
      <td>us</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>1.0</td>
      <td>88.0</td>
      <td>20.89</td>
      <td>-156.47</td>
      <td>64.40</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>cape town</th>
      <td>za</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>0.0</td>
      <td>49.0</td>
      <td>-33.93</td>
      <td>18.42</td>
      <td>71.60</td>
      <td>35.57</td>
    </tr>
    <tr>
      <th>saint-philippe</th>
      <td>re</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>20.0</td>
      <td>65.0</td>
      <td>-21.36</td>
      <td>55.77</td>
      <td>80.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>vaini</th>
      <td>to</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>20.0</td>
      <td>88.0</td>
      <td>-21.20</td>
      <td>-175.20</td>
      <td>75.20</td>
      <td>21.52</td>
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
    </tr>
    <tr>
      <th>micheweni</th>
      <td>tz</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>44.0</td>
      <td>100.0</td>
      <td>-4.97</td>
      <td>39.83</td>
      <td>83.18</td>
      <td>19.06</td>
    </tr>
    <tr>
      <th>linhares</th>
      <td>br</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>36.0</td>
      <td>95.0</td>
      <td>-19.39</td>
      <td>-40.07</td>
      <td>84.76</td>
      <td>4.97</td>
    </tr>
    <tr>
      <th>northam</th>
      <td>au</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>24.0</td>
      <td>56.0</td>
      <td>-31.67</td>
      <td>116.67</td>
      <td>73.40</td>
      <td>1.12</td>
    </tr>
    <tr>
      <th>la asuncion</th>
      <td>ve</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>36.0</td>
      <td>100.0</td>
      <td>11.03</td>
      <td>-63.86</td>
      <td>78.95</td>
      <td>14.47</td>
    </tr>
    <tr>
      <th>harper</th>
      <td>lr</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>100.0</td>
      <td>4.38</td>
      <td>-7.72</td>
      <td>76.52</td>
      <td>14.25</td>
    </tr>
    <tr>
      <th>bonavista</th>
      <td>ca</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>88.0</td>
      <td>98.0</td>
      <td>48.65</td>
      <td>-53.11</td>
      <td>35.53</td>
      <td>11.45</td>
    </tr>
    <tr>
      <th>isangel</th>
      <td>vu</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>80.0</td>
      <td>100.0</td>
      <td>-19.55</td>
      <td>169.27</td>
      <td>77.51</td>
      <td>11.12</td>
    </tr>
    <tr>
      <th>pula</th>
      <td>hr</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>20.0</td>
      <td>56.0</td>
      <td>44.87</td>
      <td>13.85</td>
      <td>42.80</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>shumskiy</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>76.0</td>
      <td>82.0</td>
      <td>54.83</td>
      <td>99.13</td>
      <td>13.25</td>
      <td>2.62</td>
    </tr>
    <tr>
      <th>pesaro</th>
      <td>it</td>
      <td>NaN</td>
      <td>1.512829e+09</td>
      <td>40.0</td>
      <td>100.0</td>
      <td>43.90</td>
      <td>12.89</td>
      <td>44.60</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>carballo</th>
      <td>es</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>75.0</td>
      <td>82.0</td>
      <td>43.21</td>
      <td>-8.69</td>
      <td>60.80</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>burriana</th>
      <td>es</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>20.0</td>
      <td>44.0</td>
      <td>39.89</td>
      <td>-0.09</td>
      <td>60.80</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>meteti</th>
      <td>pa</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>80.0</td>
      <td>100.0</td>
      <td>8.50</td>
      <td>-77.98</td>
      <td>76.93</td>
      <td>3.62</td>
    </tr>
    <tr>
      <th>pakokku</th>
      <td>mm</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>92.0</td>
      <td>100.0</td>
      <td>21.33</td>
      <td>95.10</td>
      <td>69.50</td>
      <td>6.87</td>
    </tr>
    <tr>
      <th>aktau</th>
      <td>kz</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>20.0</td>
      <td>92.0</td>
      <td>43.65</td>
      <td>51.20</td>
      <td>24.80</td>
      <td>6.71</td>
    </tr>
    <tr>
      <th>springdale</th>
      <td>ca</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>93.0</td>
      <td>49.50</td>
      <td>-56.06</td>
      <td>33.68</td>
      <td>13.80</td>
    </tr>
    <tr>
      <th>koumac</th>
      <td>nc</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>12.0</td>
      <td>95.0</td>
      <td>-20.57</td>
      <td>164.28</td>
      <td>73.15</td>
      <td>5.97</td>
    </tr>
    <tr>
      <th>hanmer springs</th>
      <td>nz</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>56.0</td>
      <td>66.0</td>
      <td>-42.52</td>
      <td>172.82</td>
      <td>62.21</td>
      <td>10.67</td>
    </tr>
    <tr>
      <th>tahoua</th>
      <td>ne</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>0.0</td>
      <td>18.0</td>
      <td>14.89</td>
      <td>5.27</td>
      <td>84.20</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>ahipara</th>
      <td>nz</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>64.0</td>
      <td>100.0</td>
      <td>-35.17</td>
      <td>173.17</td>
      <td>61.13</td>
      <td>4.63</td>
    </tr>
    <tr>
      <th>muros</th>
      <td>es</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>90.0</td>
      <td>100.0</td>
      <td>42.78</td>
      <td>-9.06</td>
      <td>55.40</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>zyryanskoye</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>77.0</td>
      <td>56.83</td>
      <td>86.63</td>
      <td>14.83</td>
      <td>11.68</td>
    </tr>
    <tr>
      <th>kushalgarh</th>
      <td>in</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>32.0</td>
      <td>64.0</td>
      <td>23.20</td>
      <td>74.45</td>
      <td>70.67</td>
      <td>4.52</td>
    </tr>
    <tr>
      <th>peniche</th>
      <td>pt</td>
      <td>NaN</td>
      <td>1.512830e+09</td>
      <td>40.0</td>
      <td>82.0</td>
      <td>39.36</td>
      <td>-9.38</td>
      <td>62.60</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>stephenville</th>
      <td>us</td>
      <td>NaN</td>
      <td>1.512829e+09</td>
      <td>1.0</td>
      <td>55.0</td>
      <td>32.22</td>
      <td>-98.20</td>
      <td>42.80</td>
      <td>14.47</td>
    </tr>
    <tr>
      <th>staryy urukh</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512828e+09</td>
      <td>0.0</td>
      <td>75.0</td>
      <td>43.34</td>
      <td>44.02</td>
      <td>41.00</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>faribault</th>
      <td>us</td>
      <td>NaN</td>
      <td>1.512829e+09</td>
      <td>90.0</td>
      <td>73.0</td>
      <td>44.30</td>
      <td>-93.27</td>
      <td>21.20</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>lavrentiya</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>100.0</td>
      <td>65.58</td>
      <td>-171.00</td>
      <td>23.33</td>
      <td>24.09</td>
    </tr>
    <tr>
      <th>erzin</th>
      <td>ru</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>53.0</td>
      <td>50.26</td>
      <td>95.16</td>
      <td>-13.57</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>dire</th>
      <td>ml</td>
      <td>NaN</td>
      <td>1.512831e+09</td>
      <td>0.0</td>
      <td>22.0</td>
      <td>12.28</td>
      <td>-10.97</td>
      <td>92.27</td>
      <td>8.32</td>
    </tr>
  </tbody>
</table>
<p>505 rows × 9 columns</p>
</div>




```python
# Formatting citySample dataframe
    #drop cases where row is all NaN just in case
citySample_df['Max Temp'].dropna(axis=0, inplace=True)

        #format date from unix timestamp 
citySample_df['TimeStamp'] = pd.to_datetime(citySample_df['TimeStamp'], unit='s')

citySample_df['Date'] = [d.date() for d in citySample_df['TimeStamp']]

#save to csv in same folder as code
citySample_df.to_csv('citySample_dataframe.csv')
```


```python
#Scatterplot of Temperature (F) vs. Latitude
sns.set_style('dark')

# Set figuresize, grid, and plot scatterplot
fig, ax = plt.subplots(figsize=(11.6, 8.25))
ax.grid(linestyle='-', linewidth=1)
plt.scatter(x=citySample_df['Latitude'], y=citySample_df['Max Temp'], edgecolor='black', alpha=0.5)

#Set titles and font sizes
plt.title('City Max Temperature vs. Latitude' + ' (' + str(citySample_df['Date'].iloc[0]) + ')', fontsize = 16)
plt.xlabel('Latitude', fontsize = 14)
plt.ylabel('Max Temperature (F)', fontsize = 14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.show()
#save as png file
fig.savefig('City Max Temperature vs. Latitude.png')
```


![png](output_7_0.png)



```python
# Scatterplot of Humidity (%) vs. Latitude

sns.set_style('dark')

# Set figuresize, grid, and plot scatterplot
fig, ax = plt.subplots(figsize=(11.6, 8.25))
ax.grid(linestyle='-', linewidth=1)
plt.scatter(x=citySample_df['Latitude'], y=citySample_df['Humidity'], edgecolor='black', alpha=0.5)

#Set titles and font sizes
plt.title('City Humidity vs. Latitude' + ' (' + str(citySample_df['Date'].iloc[0]) + ')', fontsize = 16)
plt.xlabel('Latitude', fontsize = 14)
plt.ylabel('Humidity (%)', fontsize = 14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.show()
#save as png file
fig.savefig('City Humidity vs. Latitude.png')
```


![png](output_8_0.png)



```python
#Scatterplot of Cloudiness (%) vs. Latitude

sns.set_style('dark')

# Set figuresize, grid, and plot scatterplot
fig, ax = plt.subplots(figsize=(11.6, 8.25))
ax.grid(linestyle='-', linewidth=1)
plt.scatter(x=citySample_df['Latitude'], y=citySample_df['Cloudiness'], edgecolor='black', alpha=0.5)

#Set titles
plt.title('City Cloudiness vs. Latitude' + ' (' + str(citySample_df['Date'].iloc[0]) + ')', fontsize = 16)
plt.xlabel('Latitude', fontsize = 14)
plt.ylabel('Cloudiness (%)', fontsize = 14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.show()
#save as png file
fig.savefig('City Cloudiness vs. Latitude.png')
```


![png](output_9_0.png)



```python
# Scatterplot of Wind Speed (mph) vs. Latitude

sns.set_style('dark')

# Set figuresize, grid, and plot scatterplot
fig, ax = plt.subplots(figsize=(11.6, 8.25))
ax.grid(linestyle='-', linewidth=1)
plt.scatter(x=citySample_df['Latitude'], y=citySample_df['Wind Speed (mph)'], edgecolor='black', alpha=0.5)

# Set titles
plt.title('City Wind Speed vs. Latitude' + ' (' + str(citySample_df['Date'].iloc[0]) + ')', fontsize = 16)
plt.xlabel('Latitude', fontsize = 14)
plt.ylabel('Wind Speed (mph)', fontsize = 14)
plt.xticks(fontsize=12)
plt.yticks(fontsize=12)
plt.show()
#save as png file
fig.savefig('City Wind Speed vs. Latitude.png')
```


![png](output_10_0.png)

