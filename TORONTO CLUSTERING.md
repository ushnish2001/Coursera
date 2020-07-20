This notebook is only for Clustering of the data
1)We import the libraries 
2)We get the Data from the source url
3)We merge, rename and group the data into suitable columns to make it understandable
4)We get the foursquare ID and and secret ID to run api
5)We group the venues and plot the map finally


```python
import pandas as pd
import numpy as np
import requests
from bs4 import BeautifulSoup
import os
from sklearn.cluster import KMeans
import folium 
from geopy.geocoders import Nominatim 
import matplotlib.cm as cm
import matplotlib.colors as colors


print('Libraries imported.')
```

    Libraries imported.



```python
List_url='https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M'
source = requests.get(List_url).text
```


```python
soup = BeautifulSoup(source, 'xml')
table=soup.find('table')
column_names=['Postalcode','Borough','Neighborhood']
df = pd.DataFrame(columns=column_names)
```


```python
for tr_cell in table.find_all('tr'):
    row_data=[]
    for td_cell in tr_cell.find_all('td'):
        row_data.append(td_cell.text.strip())
    if len(row_data)==3:
        df.loc[len(df)] = row_data
        df.head()
```


```python
df=df[df['Borough']!='Not assigned']
```


```python
df.head()
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
      <th>Postalcode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
    </tr>
  </tbody>
</table>
</div>




```python
temp_df=df.groupby('Postalcode')['Neighborhood'].apply(lambda x: "%s" % ', '.join(x))
temp_df=temp_df.reset_index(drop=False)
temp_df.rename(columns={'Neighborhood':'Neighborhood_joined'},inplace=True)
```


```python
df_merge = pd.merge(df, temp_df, on='Postalcode')
```


```python
df_merge.drop(['Neighborhood'],axis=1,inplace=True)
```


```python
df_merge.drop_duplicates(inplace=True)
```


```python
df_merge.rename(columns={'Neighborhood_joined':'Neighborhood'},inplace=True)
```


```python
df_merge.head()
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
      <th>Postalcode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_merge.shape
```




    (103, 3)




```python
def get_geocode(postal_code):
    # initialize your variable to None
    lat_lng_coords = None
    while(lat_lng_coords is None):
        g = geocoder.google('{}, Toronto, Ontario'.format(postal_code))
        lat_lng_coords = g.latlng
    latitude = lat_lng_coords[0]
    longitude = lat_lng_coords[1]
    return latitude,longitude
```


```python
geo_df=pd.read_csv('http://cocl.us/Geospatial_data')
```


```python
geo_df.head()
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
      <th>Postal Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
  </tbody>
</table>
</div>




```python
geo_df.rename(columns={'Postal Code':'Postalcode'},inplace=True)
geo_merged = pd.merge(geo_df, df_merge, on='Postalcode')
```


```python
geo_merged.head()
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
      <th>Postalcode</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>43.806686</td>
      <td>-79.194353</td>
      <td>Scarborough</td>
      <td>Malvern, Rouge</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>43.784535</td>
      <td>-79.160497</td>
      <td>Scarborough</td>
      <td>Rouge Hill, Port Union, Highland Creek</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>43.763573</td>
      <td>-79.188711</td>
      <td>Scarborough</td>
      <td>Guildwood, Morningside, West Hill</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>43.770992</td>
      <td>-79.216917</td>
      <td>Scarborough</td>
      <td>Woburn</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>43.773136</td>
      <td>-79.239476</td>
      <td>Scarborough</td>
      <td>Cedarbrae</td>
    </tr>
  </tbody>
</table>
</div>




```python
geo_data=geo_merged[['Postalcode','Borough','Neighborhood','Latitude','Longitude']]
geo_data.head()
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
      <th>Postalcode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>Malvern, Rouge</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>Rouge Hill, Port Union, Highland Creek</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>Scarborough</td>
      <td>Guildwood, Morningside, West Hill</td>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>Scarborough</td>
      <td>Woburn</td>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>Scarborough</td>
      <td>Cedarbrae</td>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
  </tbody>
</table>
</div>




```python
toronto_data=geo_data[geo_data['Borough'].str.contains("Toronto")]
toronto_data.head()
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
      <th>Postalcode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>37</th>
      <td>M4E</td>
      <td>East Toronto</td>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
    </tr>
    <tr>
      <th>41</th>
      <td>M4K</td>
      <td>East Toronto</td>
      <td>The Danforth West, Riverdale</td>
      <td>43.679557</td>
      <td>-79.352188</td>
    </tr>
    <tr>
      <th>42</th>
      <td>M4L</td>
      <td>East Toronto</td>
      <td>India Bazaar, The Beaches West</td>
      <td>43.668999</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>43</th>
      <td>M4M</td>
      <td>East Toronto</td>
      <td>Studio District</td>
      <td>43.659526</td>
      <td>-79.340923</td>
    </tr>
    <tr>
      <th>44</th>
      <td>M4N</td>
      <td>Central Toronto</td>
      <td>Lawrence Park</td>
      <td>43.728020</td>
      <td>-79.388790</td>
    </tr>
  </tbody>
</table>
</div>




```python
CLIENT_ID = 'QEF4SFROUUVOQKNPAFNNRDUW4ACAWSYYG312LM3BKEDDKPIZ' #  Foursquare ID
CLIENT_SECRET = 'CCBFW1WDVTUYDLPFX3SR0QBB5R2UKRYOZ3F1JDJ3PLQMPDRM' #  Foursquare Secret
VERSION = '20180604'
```


```python
def getNearbyVenues(names, latitudes, longitudes):
    radius=500
    LIMIT=100
    venues_list=[]
    for name, lat, lng in zip(names, latitudes, longitudes):
        print(name)
            
        # create the API request URL
        url = 'https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(
            CLIENT_ID, 
            CLIENT_SECRET, 
            VERSION, 
            lat, 
            lng, 
            radius, 
            LIMIT)
            
        # make the GET request
        results = requests.get(url).json()["response"]['groups'][0]['items']
        
        # return only relevant information for each nearby venue
        venues_list.append([(
            name, 
            lat, 
            lng, 
            v['venue']['name'], 
            v['venue']['location']['lat'], 
            v['venue']['location']['lng'],  
            v['venue']['categories'][0]['name']) for v in results])

    nearby_venues = pd.DataFrame([item for venue_list in venues_list for item in venue_list])
    nearby_venues.columns = ['Neighborhood', 
                  'Neighborhood Latitude', 
                  'Neighborhood Longitude', 
                  'Venue', 
                  'Venue Latitude', 
                  'Venue Longitude', 
                  'Venue Category']
    
    return(nearby_venues)
```


```python
toronto_venues = getNearbyVenues(names=toronto_data['Neighborhood'],
                                   latitudes=toronto_data['Latitude'],
                                   longitudes=toronto_data['Longitude']
                                  )
```

    The Beaches
    The Danforth West, Riverdale
    India Bazaar, The Beaches West
    Studio District
    Lawrence Park
    Davisville North
    North Toronto West,  Lawrence Park
    Davisville
    Moore Park, Summerhill East
    Summerhill West, Rathnelly, South Hill, Forest Hill SE, Deer Park
    Rosedale
    St. James Town, Cabbagetown
    Church and Wellesley
    Regent Park, Harbourfront
    Garden District, Ryerson
    St. James Town
    Berczy Park
    Central Bay Street
    Richmond, Adelaide, King
    Harbourfront East, Union Station, Toronto Islands
    Toronto Dominion Centre, Design Exchange
    Commerce Court, Victoria Hotel
    Roselawn
    Forest Hill North & West, Forest Hill Road Park
    The Annex, North Midtown, Yorkville
    University of Toronto, Harbord
    Kensington Market, Chinatown, Grange Park
    CN Tower, King and Spadina, Railway Lands, Harbourfront West, Bathurst Quay, South Niagara, Island airport
    Stn A PO Boxes
    First Canadian Place, Underground city
    Christie
    Dufferin, Dovercourt Village
    Little Portugal, Trinity
    Brockton, Parkdale Village, Exhibition Place
    High Park, The Junction South
    Parkdale, Roncesvalles
    Runnymede, Swansea
    Queen's Park, Ontario Provincial Government
    Business reply mail Processing Centre, South Central Letter Processing Plant Toronto



```python
toronto_venues.head()
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
      <th>Neighborhood</th>
      <th>Neighborhood Latitude</th>
      <th>Neighborhood Longitude</th>
      <th>Venue</th>
      <th>Venue Latitude</th>
      <th>Venue Longitude</th>
      <th>Venue Category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
      <td>Glen Manor Ravine</td>
      <td>43.676821</td>
      <td>-79.293942</td>
      <td>Trail</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
      <td>The Big Carrot Natural Food Market</td>
      <td>43.678879</td>
      <td>-79.297734</td>
      <td>Health Food Store</td>
    </tr>
    <tr>
      <th>2</th>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
      <td>Grover Pub and Grub</td>
      <td>43.679181</td>
      <td>-79.297215</td>
      <td>Pub</td>
    </tr>
    <tr>
      <th>3</th>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
      <td>Upper Beaches</td>
      <td>43.680563</td>
      <td>-79.292869</td>
      <td>Neighborhood</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
      <td>Seaspray Restaurant</td>
      <td>43.678888</td>
      <td>-79.298167</td>
      <td>Asian Restaurant</td>
    </tr>
  </tbody>
</table>
</div>




```python
toronto_venues.groupby('Neighborhood').count()
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
      <th>Neighborhood Latitude</th>
      <th>Neighborhood Longitude</th>
      <th>Venue</th>
      <th>Venue Latitude</th>
      <th>Venue Longitude</th>
      <th>Venue Category</th>
    </tr>
    <tr>
      <th>Neighborhood</th>
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
      <th>Berczy Park</th>
      <td>59</td>
      <td>59</td>
      <td>59</td>
      <td>59</td>
      <td>59</td>
      <td>59</td>
    </tr>
    <tr>
      <th>Brockton, Parkdale Village, Exhibition Place</th>
      <td>23</td>
      <td>23</td>
      <td>23</td>
      <td>23</td>
      <td>23</td>
      <td>23</td>
    </tr>
    <tr>
      <th>Business reply mail Processing Centre, South Central Letter Processing Plant Toronto</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>CN Tower, King and Spadina, Railway Lands, Harbourfront West, Bathurst Quay, South Niagara, Island airport</th>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <th>Central Bay Street</th>
      <td>65</td>
      <td>65</td>
      <td>65</td>
      <td>65</td>
      <td>65</td>
      <td>65</td>
    </tr>
    <tr>
      <th>Christie</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Church and Wellesley</th>
      <td>74</td>
      <td>74</td>
      <td>74</td>
      <td>74</td>
      <td>74</td>
      <td>74</td>
    </tr>
    <tr>
      <th>Commerce Court, Victoria Hotel</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Davisville</th>
      <td>33</td>
      <td>33</td>
      <td>33</td>
      <td>33</td>
      <td>33</td>
      <td>33</td>
    </tr>
    <tr>
      <th>Davisville North</th>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <th>Dufferin, Dovercourt Village</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>First Canadian Place, Underground city</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Forest Hill North &amp; West, Forest Hill Road Park</th>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Garden District, Ryerson</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Harbourfront East, Union Station, Toronto Islands</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>High Park, The Junction South</th>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
    </tr>
    <tr>
      <th>India Bazaar, The Beaches West</th>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th>Kensington Market, Chinatown, Grange Park</th>
      <td>68</td>
      <td>68</td>
      <td>68</td>
      <td>68</td>
      <td>68</td>
      <td>68</td>
    </tr>
    <tr>
      <th>Lawrence Park</th>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Little Portugal, Trinity</th>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
      <td>47</td>
    </tr>
    <tr>
      <th>Moore Park, Summerhill East</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>North Toronto West,  Lawrence Park</th>
      <td>17</td>
      <td>17</td>
      <td>17</td>
      <td>17</td>
      <td>17</td>
      <td>17</td>
    </tr>
    <tr>
      <th>Parkdale, Roncesvalles</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>Queen's Park, Ontario Provincial Government</th>
      <td>31</td>
      <td>31</td>
      <td>31</td>
      <td>31</td>
      <td>31</td>
      <td>31</td>
    </tr>
    <tr>
      <th>Regent Park, Harbourfront</th>
      <td>44</td>
      <td>44</td>
      <td>44</td>
      <td>44</td>
      <td>44</td>
      <td>44</td>
    </tr>
    <tr>
      <th>Richmond, Adelaide, King</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Rosedale</th>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Roselawn</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Runnymede, Swansea</th>
      <td>37</td>
      <td>37</td>
      <td>37</td>
      <td>37</td>
      <td>37</td>
      <td>37</td>
    </tr>
    <tr>
      <th>St. James Town</th>
      <td>88</td>
      <td>88</td>
      <td>88</td>
      <td>88</td>
      <td>88</td>
      <td>88</td>
    </tr>
    <tr>
      <th>St. James Town, Cabbagetown</th>
      <td>45</td>
      <td>45</td>
      <td>45</td>
      <td>45</td>
      <td>45</td>
      <td>45</td>
    </tr>
    <tr>
      <th>Stn A PO Boxes</th>
      <td>97</td>
      <td>97</td>
      <td>97</td>
      <td>97</td>
      <td>97</td>
      <td>97</td>
    </tr>
    <tr>
      <th>Studio District</th>
      <td>40</td>
      <td>40</td>
      <td>40</td>
      <td>40</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <th>Summerhill West, Rathnelly, South Hill, Forest Hill SE, Deer Park</th>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>The Annex, North Midtown, Yorkville</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>The Beaches</th>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>The Danforth West, Riverdale</th>
      <td>42</td>
      <td>42</td>
      <td>42</td>
      <td>42</td>
      <td>42</td>
      <td>42</td>
    </tr>
    <tr>
      <th>Toronto Dominion Centre, Design Exchange</th>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>University of Toronto, Harbord</th>
      <td>37</td>
      <td>37</td>
      <td>37</td>
      <td>37</td>
      <td>37</td>
      <td>37</td>
    </tr>
  </tbody>
</table>
</div>




```python
toronto_onehot = pd.get_dummies(toronto_venues[['Venue Category']], prefix="", prefix_sep="")
toronto_onehot.drop(['Neighborhood'],axis=1,inplace=True) 
toronto_onehot.insert(loc=0, column='Neighborhood', value=toronto_venues['Neighborhood'] )
toronto_onehot.shape
```




    (1633, 230)




```python
toronto_grouped = toronto_onehot.groupby('Neighborhood').mean().reset_index()
toronto_grouped.head()
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
      <th>Neighborhood</th>
      <th>Afghan Restaurant</th>
      <th>Airport</th>
      <th>Airport Food Court</th>
      <th>Airport Gate</th>
      <th>Airport Lounge</th>
      <th>Airport Service</th>
      <th>Airport Terminal</th>
      <th>American Restaurant</th>
      <th>Antique Shop</th>
      <th>...</th>
      <th>Toy / Game Store</th>
      <th>Trail</th>
      <th>Train Station</th>
      <th>Vegetarian / Vegan Restaurant</th>
      <th>Video Game Store</th>
      <th>Vietnamese Restaurant</th>
      <th>Wine Bar</th>
      <th>Wine Shop</th>
      <th>Women's Store</th>
      <th>Yoga Studio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Berczy Park</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.016949</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Brockton, Parkdale Village, Exhibition Place</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Business reply mail Processing Centre, South C...</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CN Tower, King and Spadina, Railway Lands, Har...</td>
      <td>0.0</td>
      <td>0.066667</td>
      <td>0.066667</td>
      <td>0.066667</td>
      <td>0.133333</td>
      <td>0.2</td>
      <td>0.066667</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Central Bay Street</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.015385</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.015385</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.015385</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 230 columns</p>
</div>




```python
def return_most_common_venues(row, num_top_venues):
    row_categories = row.iloc[1:]
    row_categories_sorted = row_categories.sort_values(ascending=False)
    
    return row_categories_sorted.index.values[0:num_top_venues]
```


```python
num_top_venues = 10

indicators = ['st', 'nd', 'rd']

# create columns according to number of top venues
columns = ['Neighborhood']
for ind in np.arange(num_top_venues):
    try:
        columns.append('{}{} Most Common Venue'.format(ind+1, indicators[ind]))
    except:
        columns.append('{}th Most Common Venue'.format(ind+1))

# create a new dataframe
neighborhoods_venues_sorted = pd.DataFrame(columns=columns)
neighborhoods_venues_sorted['Neighborhood'] = toronto_grouped['Neighborhood']

for ind in np.arange(toronto_grouped.shape[0]):
    neighborhoods_venues_sorted.iloc[ind, 1:] = return_most_common_venues(toronto_grouped.iloc[ind, :], num_top_venues)

neighborhoods_venues_sorted.head()
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
      <th>Neighborhood</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Berczy Park</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Restaurant</td>
      <td>Beer Bar</td>
      <td>Cheese Shop</td>
      <td>Pharmacy</td>
      <td>Seafood Restaurant</td>
      <td>Bakery</td>
      <td>Cocktail Bar</td>
      <td>Farmers Market</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Brockton, Parkdale Village, Exhibition Place</td>
      <td>Café</td>
      <td>Nightclub</td>
      <td>Breakfast Spot</td>
      <td>Coffee Shop</td>
      <td>Pet Store</td>
      <td>Stadium</td>
      <td>Restaurant</td>
      <td>Bar</td>
      <td>Italian Restaurant</td>
      <td>Bakery</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Business reply mail Processing Centre, South C...</td>
      <td>Light Rail Station</td>
      <td>Skate Park</td>
      <td>Burrito Place</td>
      <td>Park</td>
      <td>Restaurant</td>
      <td>Brewery</td>
      <td>Garden Center</td>
      <td>Gym / Fitness Center</td>
      <td>Garden</td>
      <td>Pizza Place</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CN Tower, King and Spadina, Railway Lands, Har...</td>
      <td>Airport Service</td>
      <td>Airport Lounge</td>
      <td>Boat or Ferry</td>
      <td>Airport</td>
      <td>Airport Food Court</td>
      <td>Airport Gate</td>
      <td>Airport Terminal</td>
      <td>Boutique</td>
      <td>Rental Car Location</td>
      <td>Sculpture Garden</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Central Bay Street</td>
      <td>Coffee Shop</td>
      <td>Italian Restaurant</td>
      <td>Café</td>
      <td>Sandwich Place</td>
      <td>Japanese Restaurant</td>
      <td>Burger Joint</td>
      <td>Department Store</td>
      <td>Bubble Tea Shop</td>
      <td>Salad Place</td>
      <td>Thai Restaurant</td>
    </tr>
  </tbody>
</table>
</div>




```python
kclusters = 5

toronto_grouped_clustering = toronto_grouped.drop('Neighborhood', 1)

# run k-means clustering
kmeans = KMeans(n_clusters=kclusters, random_state=0).fit(toronto_grouped_clustering)

# check cluster labels generated for each row in the dataframe
kmeans.labels_[0:10]
```




    array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0], dtype=int32)




```python
neighborhoods_venues_sorted.insert(0, 'Cluster Labels', kmeans.labels_)

toronto_merged = toronto_data

# merge toronto_grouped with toronto_data to add latitude/longitude for each neighborhood
toronto_merged = toronto_merged.join(neighborhoods_venues_sorted.set_index('Neighborhood'), on='Neighborhood')

toronto_merged.head()
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
      <th>Postalcode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Cluster Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>37</th>
      <td>M4E</td>
      <td>East Toronto</td>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
      <td>0</td>
      <td>Asian Restaurant</td>
      <td>Health Food Store</td>
      <td>Pub</td>
      <td>Trail</td>
      <td>Donut Shop</td>
      <td>Doner Restaurant</td>
      <td>Dog Run</td>
      <td>Distribution Center</td>
      <td>Cupcake Shop</td>
      <td>Dumpling Restaurant</td>
    </tr>
    <tr>
      <th>41</th>
      <td>M4K</td>
      <td>East Toronto</td>
      <td>The Danforth West, Riverdale</td>
      <td>43.679557</td>
      <td>-79.352188</td>
      <td>0</td>
      <td>Greek Restaurant</td>
      <td>Coffee Shop</td>
      <td>Italian Restaurant</td>
      <td>Ice Cream Shop</td>
      <td>Furniture / Home Store</td>
      <td>Restaurant</td>
      <td>Bubble Tea Shop</td>
      <td>Bakery</td>
      <td>Pub</td>
      <td>Pizza Place</td>
    </tr>
    <tr>
      <th>42</th>
      <td>M4L</td>
      <td>East Toronto</td>
      <td>India Bazaar, The Beaches West</td>
      <td>43.668999</td>
      <td>-79.315572</td>
      <td>0</td>
      <td>Park</td>
      <td>Pizza Place</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Pub</td>
      <td>Liquor Store</td>
      <td>Board Shop</td>
      <td>Sandwich Place</td>
      <td>Burrito Place</td>
      <td>Italian Restaurant</td>
      <td>Restaurant</td>
    </tr>
    <tr>
      <th>43</th>
      <td>M4M</td>
      <td>East Toronto</td>
      <td>Studio District</td>
      <td>43.659526</td>
      <td>-79.340923</td>
      <td>0</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Gastropub</td>
      <td>Brewery</td>
      <td>Bakery</td>
      <td>American Restaurant</td>
      <td>Sandwich Place</td>
      <td>Cheese Shop</td>
      <td>Clothing Store</td>
      <td>Pet Store</td>
    </tr>
    <tr>
      <th>44</th>
      <td>M4N</td>
      <td>Central Toronto</td>
      <td>Lawrence Park</td>
      <td>43.728020</td>
      <td>-79.388790</td>
      <td>4</td>
      <td>Park</td>
      <td>Swim School</td>
      <td>Bus Line</td>
      <td>Yoga Studio</td>
      <td>Dance Studio</td>
      <td>Dumpling Restaurant</td>
      <td>Donut Shop</td>
      <td>Doner Restaurant</td>
      <td>Dog Run</td>
      <td>Distribution Center</td>
    </tr>
  </tbody>
</table>
</div>




```python
neighborhoods_venues_sorted.head()

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
      <th>Cluster Labels</th>
      <th>Neighborhood</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Berczy Park</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Restaurant</td>
      <td>Beer Bar</td>
      <td>Cheese Shop</td>
      <td>Pharmacy</td>
      <td>Seafood Restaurant</td>
      <td>Bakery</td>
      <td>Cocktail Bar</td>
      <td>Farmers Market</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Brockton, Parkdale Village, Exhibition Place</td>
      <td>Café</td>
      <td>Nightclub</td>
      <td>Breakfast Spot</td>
      <td>Coffee Shop</td>
      <td>Pet Store</td>
      <td>Stadium</td>
      <td>Restaurant</td>
      <td>Bar</td>
      <td>Italian Restaurant</td>
      <td>Bakery</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Business reply mail Processing Centre, South C...</td>
      <td>Light Rail Station</td>
      <td>Skate Park</td>
      <td>Burrito Place</td>
      <td>Park</td>
      <td>Restaurant</td>
      <td>Brewery</td>
      <td>Garden Center</td>
      <td>Gym / Fitness Center</td>
      <td>Garden</td>
      <td>Pizza Place</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>CN Tower, King and Spadina, Railway Lands, Har...</td>
      <td>Airport Service</td>
      <td>Airport Lounge</td>
      <td>Boat or Ferry</td>
      <td>Airport</td>
      <td>Airport Food Court</td>
      <td>Airport Gate</td>
      <td>Airport Terminal</td>
      <td>Boutique</td>
      <td>Rental Car Location</td>
      <td>Sculpture Garden</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Central Bay Street</td>
      <td>Coffee Shop</td>
      <td>Italian Restaurant</td>
      <td>Café</td>
      <td>Sandwich Place</td>
      <td>Japanese Restaurant</td>
      <td>Burger Joint</td>
      <td>Department Store</td>
      <td>Bubble Tea Shop</td>
      <td>Salad Place</td>
      <td>Thai Restaurant</td>
    </tr>
  </tbody>
</table>
</div>




```python
address = 'Toronto, CA'

geolocator = Nominatim(user_agent="ny_explorer")
location = geolocator.geocode(address)
latitude = location.latitude
longitude = location.longitude
print('The geograpical coordinate of Toronto are {}, {}.'.format(latitude, longitude))

```

    The geograpical coordinate of Toronto are 43.6534817, -79.3839347.



```python
map_clusters = folium.Map(location=[latitude, longitude], zoom_start=11)

# set color scheme for the clusters
x = np.arange(kclusters)
ys = [i + x + (i*x)**2 for i in range(kclusters)]
colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
rainbow = [colors.rgb2hex(i) for i in colors_array]

# add markers to the map
markers_colors = []
for lat, lon, poi, cluster in zip(toronto_merged['Latitude'], toronto_merged['Longitude'], toronto_merged['Neighborhood'], toronto_merged['Cluster Labels']):
    label = folium.Popup(str(poi) + ' Cluster ' + str(cluster), parse_html=True)
    folium.CircleMarker(
        [lat, lon],
        radius=5,
        popup=label,
        color=rainbow[cluster-1],
        fill=True,
        fill_color=rainbow[cluster-1],
        fill_opacity=0.7).add_to(map_clusters)
       
map_clusters
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCcsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDMuNjUzNDgxNywtNzkuMzgzOTM0N10sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfMTgyNmJiZTQzNDFlNGE3OTg0MjU4ZGEwNTI5MmMxMzQgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk4MTgyY2JjOTE0OTRkMjFhY2I0YTc0NGNiYTU2MGFkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjc2MzU3Mzk5OTk5OTksLTc5LjI5MzAzMTJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTVhYjRlZTA0NjQ5NGEyZWE5YWVjYjE0ZGJiM2EzNTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfN2Q2ODJkZmEyMzA3NDlhMjhlYjk4OWM2N2E3ZTE0NzMgPSAkKCc8ZGl2IGlkPSJodG1sXzdkNjgyZGZhMjMwNzQ5YTI4ZWI5ODljNjdhN2UxNDczIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5UaGUgQmVhY2hlcyBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2U1YWI0ZWUwNDY0OTRhMmVhOWFlY2IxNGRiYjNhMzUyLnNldENvbnRlbnQoaHRtbF83ZDY4MmRmYTIzMDc0OWEyOGViOTg5YzY3YTdlMTQ3Myk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85ODE4MmNiYzkxNDk0ZDIxYWNiNGE3NDRjYmE1NjBhZC5iaW5kUG9wdXAocG9wdXBfZTVhYjRlZTA0NjQ5NGEyZWE5YWVjYjE0ZGJiM2EzNTIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMTkzMWI2NjRiYjY5NDgxODk2OTk1MTJjMDEyYWQzMmUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Nzk1NTcxLC03OS4zNTIxODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTA0MjJhMTcyMTI4NGI1MThkMzA2Yjc5NTM4Yjg2ZjggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODA3OTE5ZmUxZDM3NDk0MGE4M2Y4OTk4ZTgyYWQ4ZDUgPSAkKCc8ZGl2IGlkPSJodG1sXzgwNzkxOWZlMWQzNzQ5NDBhODNmODk5OGU4MmFkOGQ1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5UaGUgRGFuZm9ydGggV2VzdCwgUml2ZXJkYWxlIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYTA0MjJhMTcyMTI4NGI1MThkMzA2Yjc5NTM4Yjg2Zjguc2V0Q29udGVudChodG1sXzgwNzkxOWZlMWQzNzQ5NDBhODNmODk5OGU4MmFkOGQ1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzE5MzFiNjY0YmI2OTQ4MTg5Njk5NTEyYzAxMmFkMzJlLmJpbmRQb3B1cChwb3B1cF9hMDQyMmExNzIxMjg0YjUxOGQzMDZiNzk1MzhiODZmOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84M2Q3YjdjYzNjM2Y0NjMxYmQxMzNhNDEyN2MwZmMxNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2ODk5ODUsLTc5LjMxNTU3MTU5OTk5OTk4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2JlMGY0NjE1Y2UxNzQ3ZjNhNWEwMmY5MDViNmNkOGFhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2QxZDRhNTZjNjdlOTQ4NGI5YWZhNGNhNzYwZDVlZDBlID0gJCgnPGRpdiBpZD0iaHRtbF9kMWQ0YTU2YzY3ZTk0ODRiOWFmYTRjYTc2MGQ1ZWQwZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SW5kaWEgQmF6YWFyLCBUaGUgQmVhY2hlcyBXZXN0IENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYmUwZjQ2MTVjZTE3NDdmM2E1YTAyZjkwNWI2Y2Q4YWEuc2V0Q29udGVudChodG1sX2QxZDRhNTZjNjdlOTQ4NGI5YWZhNGNhNzYwZDVlZDBlKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzgzZDdiN2NjM2MzZjQ2MzFiZDEzM2E0MTI3YzBmYzE1LmJpbmRQb3B1cChwb3B1cF9iZTBmNDYxNWNlMTc0N2YzYTVhMDJmOTA1YjZjZDhhYSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kNmFhN2RmMzA0MGY0ZGQ3YTI5MTcyY2I2MDhlZWUwMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1OTUyNTUsLTc5LjM0MDkyM10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jZDkyMmZlMjRjOGE0MWE5OTM4MmU4ZjZjNDBmYWYxYSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zZDg0ZGI5ZWNkNmU0Njg2ODU1OGJkNjc2OWRjNWJkNSA9ICQoJzxkaXYgaWQ9Imh0bWxfM2Q4NGRiOWVjZDZlNDY4Njg1NThiZDY3NjlkYzViZDUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN0dWRpbyBEaXN0cmljdCBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NkOTIyZmUyNGM4YTQxYTk5MzgyZThmNmM0MGZhZjFhLnNldENvbnRlbnQoaHRtbF8zZDg0ZGI5ZWNkNmU0Njg2ODU1OGJkNjc2OWRjNWJkNSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kNmFhN2RmMzA0MGY0ZGQ3YTI5MTcyY2I2MDhlZWUwMy5iaW5kUG9wdXAocG9wdXBfY2Q5MjJmZTI0YzhhNDFhOTkzODJlOGY2YzQwZmFmMWEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNDQzM2Y2MmE1NWM5NDg1NzkxYWU4ZjUxZTkwYjI2ODIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MjgwMjA1LC03OS4zODg3OTAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZmIzNjAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmZiMzYwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzQ4YzEyNDgzYmEyMDQ1MzNhNTFjNjZiMjdkNjg5Y2JiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2UzMzYyODg2NTU0OTRiZDNhZWMyNGNkNGM1ODY5MTk0ID0gJCgnPGRpdiBpZD0iaHRtbF9lMzM2Mjg4NjU1NDk0YmQzYWVjMjRjZDRjNTg2OTE5NCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TGF3cmVuY2UgUGFyayBDbHVzdGVyIDQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQ4YzEyNDgzYmEyMDQ1MzNhNTFjNjZiMjdkNjg5Y2JiLnNldENvbnRlbnQoaHRtbF9lMzM2Mjg4NjU1NDk0YmQzYWVjMjRjZDRjNTg2OTE5NCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80NDMzZjYyYTU1Yzk0ODU3OTFhZThmNTFlOTBiMjY4Mi5iaW5kUG9wdXAocG9wdXBfNDhjMTI0ODNiYTIwNDUzM2E1MWM2NmIyN2Q2ODljYmIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjc3MDEwMzhkNWEyNGNjMmE0MjQ4ZDQyNjRhOTZiNWEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTI3NTExLC03OS4zOTAxOTc1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2RkNDg4Y2U5ODA1NzQ2OWRhMmExYjk0YWRlNzIwOGRkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2IxZDMwNTE1NzA1NTQ0MjM5MmU5OWE0N2ZkNDYzYjk5ID0gJCgnPGRpdiBpZD0iaHRtbF9iMWQzMDUxNTcwNTU0NDIzOTJlOTlhNDdmZDQ2M2I5OSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RGF2aXN2aWxsZSBOb3J0aCBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2RkNDg4Y2U5ODA1NzQ2OWRhMmExYjk0YWRlNzIwOGRkLnNldENvbnRlbnQoaHRtbF9iMWQzMDUxNTcwNTU0NDIzOTJlOTlhNDdmZDQ2M2I5OSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yNzcwMTAzOGQ1YTI0Y2MyYTQyNDhkNDI2NGE5NmI1YS5iaW5kUG9wdXAocG9wdXBfZGQ0ODhjZTk4MDU3NDY5ZGEyYTFiOTRhZGU3MjA4ZGQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWQ4NTA3MWVhNjZlNDc3NjhkZGMzOGZmMDU0NTUxYTkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTUzODM0LC03OS40MDU2Nzg0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yMmI2ZDJhMDE5NjE0NDUzODkyN2UwZjIwMzkyNWU5NiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zMGFkMTJiZjQ5YmY0NWIyOGYyZjUyOTdlMjBiZmNlOSA9ICQoJzxkaXYgaWQ9Imh0bWxfMzBhZDEyYmY0OWJmNDViMjhmMmY1Mjk3ZTIwYmZjZTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFRvcm9udG8gV2VzdCwgIExhd3JlbmNlIFBhcmsgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yMmI2ZDJhMDE5NjE0NDUzODkyN2UwZjIwMzkyNWU5Ni5zZXRDb250ZW50KGh0bWxfMzBhZDEyYmY0OWJmNDViMjhmMmY1Mjk3ZTIwYmZjZTkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOWQ4NTA3MWVhNjZlNDc3NjhkZGMzOGZmMDU0NTUxYTkuYmluZFBvcHVwKHBvcHVwXzIyYjZkMmEwMTk2MTQ0NTM4OTI3ZTBmMjAzOTI1ZTk2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzNmMmRiYTcwYjZlNjRiNWNhZmU3YjNmMDhlZmVjMWEzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA0MzI0NCwtNzkuMzg4NzkwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jODdmMmY0ZGUwYzY0ZTc1ODg0ZmQ1NTAwMDNiYmI2NiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yYzY3NmJjYzMyZDQ0OTFkYWE0ZjMzMTRkZjdiMjBkZiA9ICQoJzxkaXYgaWQ9Imh0bWxfMmM2NzZiY2MzMmQ0NDkxZGFhNGYzMzE0ZGY3YjIwZGYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRhdmlzdmlsbGUgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jODdmMmY0ZGUwYzY0ZTc1ODg0ZmQ1NTAwMDNiYmI2Ni5zZXRDb250ZW50KGh0bWxfMmM2NzZiY2MzMmQ0NDkxZGFhNGYzMzE0ZGY3YjIwZGYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfM2YyZGJhNzBiNmU2NGI1Y2FmZTdiM2YwOGVmZWMxYTMuYmluZFBvcHVwKHBvcHVwX2M4N2YyZjRkZTBjNjRlNzU4ODRmZDU1MDAwM2JiYjY2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U1ZGMwNDVhZWI4YzQyOThhN2VmZmRhYmRjZTZmMjk2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjg5NTc0MywtNzkuMzgzMTU5OTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfY2QwYjAzYjdiODI0NDA4YThiN2JlZTU1MmQ0NGJhZTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZTIxNjBiMTFiM2NiNDY5YjgyNTE4NWRlMzFjMTYyZmUgPSAkKCc8ZGl2IGlkPSJodG1sX2UyMTYwYjExYjNjYjQ2OWI4MjUxODVkZTMxYzE2MmZlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Nb29yZSBQYXJrLCBTdW1tZXJoaWxsIEVhc3QgQ2x1c3RlciAzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jZDBiMDNiN2I4MjQ0MDhhOGI3YmVlNTUyZDQ0YmFlMi5zZXRDb250ZW50KGh0bWxfZTIxNjBiMTFiM2NiNDY5YjgyNTE4NWRlMzFjMTYyZmUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTVkYzA0NWFlYjhjNDI5OGE3ZWZmZGFiZGNlNmYyOTYuYmluZFBvcHVwKHBvcHVwX2NkMGIwM2I3YjgyNDQwOGE4YjdiZWU1NTJkNDRiYWUyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzM3ZDkwNGE2ODU1YzRkYTlhOTY3YTNiNDM3MjNlY2ZkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjg2NDEyMjk5OTk5OTksLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZDBmNjM4YWU5ZjM5NGZiOWJlNTU4MmQ0ZmYzYjU0YzUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOWNhMjAwMjU0NzNkNDg0ZGFhODA3MGU1M2JhYjg2MGEgPSAkKCc8ZGl2IGlkPSJodG1sXzljYTIwMDI1NDczZDQ4NGRhYTgwNzBlNTNiYWI4NjBhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TdW1tZXJoaWxsIFdlc3QsIFJhdGhuZWxseSwgU291dGggSGlsbCwgRm9yZXN0IEhpbGwgU0UsIERlZXIgUGFyayBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2QwZjYzOGFlOWYzOTRmYjliZTU1ODJkNGZmM2I1NGM1LnNldENvbnRlbnQoaHRtbF85Y2EyMDAyNTQ3M2Q0ODRkYWE4MDcwZTUzYmFiODYwYSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zN2Q5MDRhNjg1NWM0ZGE5YTk2N2EzYjQzNzIzZWNmZC5iaW5kUG9wdXAocG9wdXBfZDBmNjM4YWU5ZjM5NGZiOWJlNTU4MmQ0ZmYzYjU0YzUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTlkZGQyMzlhNTE1NDZkZjhmOWQ5NGQyOTE2MWFmMmEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Nzk1NjI2LC03OS4zNzc1Mjk0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mOWYwNTNmYzlkZmI0MWQ0ODRmMjFmNjc4MjliMGQ4YSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80YzEyMTZiYTJhZTg0MjgxYWJlZTM4MmE5NzA5Y2ZlOSA9ICQoJzxkaXYgaWQ9Imh0bWxfNGMxMjE2YmEyYWU4NDI4MWFiZWUzODJhOTcwOWNmZTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJvc2VkYWxlIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZjlmMDUzZmM5ZGZiNDFkNDg0ZjIxZjY3ODI5YjBkOGEuc2V0Q29udGVudChodG1sXzRjMTIxNmJhMmFlODQyODFhYmVlMzgyYTk3MDljZmU5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2U5ZGRkMjM5YTUxNTQ2ZGY4ZjlkOTRkMjkxNjFhZjJhLmJpbmRQb3B1cChwb3B1cF9mOWYwNTNmYzlkZmI0MWQ0ODRmMjFmNjc4MjliMGQ4YSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xNmY0ZmFhMTgxNWU0OTE1OTdhMTc1NzQyMDQyNjQ0MSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2Nzk2NywtNzkuMzY3Njc1M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80NGM0ZTZmMzQ3ODg0MTVjYWI1ZTgwZTQ0ZDRkYTNmNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zMWMyYzViYTZlYjg0ZDM1YTlkNzBhNGIwMTM3MmVhYiA9ICQoJzxkaXYgaWQ9Imh0bWxfMzFjMmM1YmE2ZWI4NGQzNWE5ZDcwYTRiMDEzNzJlYWIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN0LiBKYW1lcyBUb3duLCBDYWJiYWdldG93biBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQ0YzRlNmYzNDc4ODQxNWNhYjVlODBlNDRkNGRhM2Y1LnNldENvbnRlbnQoaHRtbF8zMWMyYzViYTZlYjg0ZDM1YTlkNzBhNGIwMTM3MmVhYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xNmY0ZmFhMTgxNWU0OTE1OTdhMTc1NzQyMDQyNjQ0MS5iaW5kUG9wdXAocG9wdXBfNDRjNGU2ZjM0Nzg4NDE1Y2FiNWU4MGU0NGQ0ZGEzZjUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZmI5ZmVjZDcxMmZlNGJlZTk5ZDBjNGYxMGRjMGRkNTUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjU4NTk5LC03OS4zODMxNTk5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jM2YwZWViNDUzMDM0NjU5YmU1YTM4N2ExNTgwNWM1MSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jOGNkYzFmNzBhMzQ0MjcwODQzMjA4OTJkOTFkZWRlOSA9ICQoJzxkaXYgaWQ9Imh0bWxfYzhjZGMxZjcwYTM0NDI3MDg0MzIwODkyZDkxZGVkZTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNodXJjaCBhbmQgV2VsbGVzbGV5IENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYzNmMGVlYjQ1MzAzNDY1OWJlNWEzODdhMTU4MDVjNTEuc2V0Q29udGVudChodG1sX2M4Y2RjMWY3MGEzNDQyNzA4NDMyMDg5MmQ5MWRlZGU5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ZiOWZlY2Q3MTJmZTRiZWU5OWQwYzRmMTBkYzBkZDU1LmJpbmRQb3B1cChwb3B1cF9jM2YwZWViNDUzMDM0NjU5YmU1YTM4N2ExNTgwNWM1MSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85MDk0ZDg2NmVmNTM0YTQ3YjVkNzZlN2VjNGMyYTFlOSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1NDI1OTksLTc5LjM2MDYzNTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMTE5YWEyNTU4OTZjNDMyMDhlZmYzZTA1MDNjY2RjOTAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfY2E2NTZjOGNkODdkNDA3NjlkOGI3ZTBlMDgwZDAxYmIgPSAkKCc8ZGl2IGlkPSJodG1sX2NhNjU2YzhjZDg3ZDQwNzY5ZDhiN2UwZTA4MGQwMWJiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5SZWdlbnQgUGFyaywgSGFyYm91cmZyb250IENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTE5YWEyNTU4OTZjNDMyMDhlZmYzZTA1MDNjY2RjOTAuc2V0Q29udGVudChodG1sX2NhNjU2YzhjZDg3ZDQwNzY5ZDhiN2UwZTA4MGQwMWJiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzkwOTRkODY2ZWY1MzRhNDdiNWQ3NmU3ZWM0YzJhMWU5LmJpbmRQb3B1cChwb3B1cF8xMTlhYTI1NTg5NmM0MzIwOGVmZjNlMDUwM2NjZGM5MCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81MzJlMDk2ZWMwZDA0MjY4OGJlNjljYzkwZjk4ZjdkMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1NzE2MTgsLTc5LjM3ODkzNzA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVmMWI2MWNiNDI5NzQ0YmY5NWM1YmQ3ZWNiYTg0NTY2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzM3MzljOWUyZDkxYzRhODg5ZjBmNTM2YWNjZGVjZDExID0gJCgnPGRpdiBpZD0iaHRtbF8zNzM5YzllMmQ5MWM0YTg4OWYwZjUzNmFjY2RlY2QxMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+R2FyZGVuIERpc3RyaWN0LCBSeWVyc29uIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNWYxYjYxY2I0Mjk3NDRiZjk1YzViZDdlY2JhODQ1NjYuc2V0Q29udGVudChodG1sXzM3MzljOWUyZDkxYzRhODg5ZjBmNTM2YWNjZGVjZDExKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzUzMmUwOTZlYzBkMDQyNjg4YmU2OWNjOTBmOThmN2QwLmJpbmRQb3B1cChwb3B1cF81ZjFiNjFjYjQyOTc0NGJmOTVjNWJkN2VjYmE4NDU2Nik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84NjcwOTJhNDRiOWE0MjFhYWM2ZTE5MTFlOWQzMzZkMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MTQ5MzksLTc5LjM3NTQxNzldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzljZDhjMThlODE0NDUyNWEzZDEyM2I4ZmIyYmZhZjggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMWU0NjcwNjE1YmRjNDVhYjhjYTI1NWFmNjVjZDc5MDUgPSAkKCc8ZGl2IGlkPSJodG1sXzFlNDY3MDYxNWJkYzQ1YWI4Y2EyNTVhZjY1Y2Q3OTA1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TdC4gSmFtZXMgVG93biBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzc5Y2Q4YzE4ZTgxNDQ1MjVhM2QxMjNiOGZiMmJmYWY4LnNldENvbnRlbnQoaHRtbF8xZTQ2NzA2MTViZGM0NWFiOGNhMjU1YWY2NWNkNzkwNSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84NjcwOTJhNDRiOWE0MjFhYWM2ZTE5MTFlOWQzMzZkMy5iaW5kUG9wdXAocG9wdXBfNzljZDhjMThlODE0NDUyNWEzZDEyM2I4ZmIyYmZhZjgpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTMwNjY4Y2RmYzlhNDliODk5YTI5NDE0OTk3NjgzN2EgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDQ3NzA3OTk5OTk5OTYsLTc5LjM3MzMwNjRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfN2EzNWQ3NWUyNDM2NDMyYzhlMmY4ZDE1NzgxNWZhNTUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjViNzRjMDFkZGIzNDQzNDllYTkzYzlmY2FhZmJmYWUgPSAkKCc8ZGl2IGlkPSJodG1sX2Y1Yjc0YzAxZGRiMzQ0MzQ5ZWE5M2M5ZmNhYWZiZmFlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5CZXJjenkgUGFyayBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzdhMzVkNzVlMjQzNjQzMmM4ZTJmOGQxNTc4MTVmYTU1LnNldENvbnRlbnQoaHRtbF9mNWI3NGMwMWRkYjM0NDM0OWVhOTNjOWZjYWFmYmZhZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85MzA2NjhjZGZjOWE0OWI4OTlhMjk0MTQ5OTc2ODM3YS5iaW5kUG9wdXAocG9wdXBfN2EzNWQ3NWUyNDM2NDMyYzhlMmY4ZDE1NzgxNWZhNTUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOGRhYzkwZDU3YTBiNDM5YjhjNzg2ZTRkOTIzMGU2ZGYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTc5NTI0LC03OS4zODczODI2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzBjNjRkNGJiNmIyZjRiYzk5NjEzYmExMDg2ZmQyNjgyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzUzYjdlYjFkZTE1NTRlNDZiOTJhMWFmMjdlZjY5NmM4ID0gJCgnPGRpdiBpZD0iaHRtbF81M2I3ZWIxZGUxNTU0ZTQ2YjkyYTFhZjI3ZWY2OTZjOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2VudHJhbCBCYXkgU3RyZWV0IENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMGM2NGQ0YmI2YjJmNGJjOTk2MTNiYTEwODZmZDI2ODIuc2V0Q29udGVudChodG1sXzUzYjdlYjFkZTE1NTRlNDZiOTJhMWFmMjdlZjY5NmM4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzhkYWM5MGQ1N2EwYjQzOWI4Yzc4NmU0ZDkyMzBlNmRmLmJpbmRQb3B1cChwb3B1cF8wYzY0ZDRiYjZiMmY0YmM5OTYxM2JhMTA4NmZkMjY4Mik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zYmJjNmFiN2Q5MTY0ODMyYmJkNWZmNTgwNGY3ZTVkNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MDU3MTIwMDAwMDAxLC03OS4zODQ1Njc1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2RkMmVmNTJlMTBkNTRhZWY5NzE1M2VmY2FkNTY0NGM1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzU4MGQwZTdkMjhhYzQwN2Q4OGI1MDc0ODQ1ZjI2OTYyID0gJCgnPGRpdiBpZD0iaHRtbF81ODBkMGU3ZDI4YWM0MDdkODhiNTA3NDg0NWYyNjk2MiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UmljaG1vbmQsIEFkZWxhaWRlLCBLaW5nIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZGQyZWY1MmUxMGQ1NGFlZjk3MTUzZWZjYWQ1NjQ0YzUuc2V0Q29udGVudChodG1sXzU4MGQwZTdkMjhhYzQwN2Q4OGI1MDc0ODQ1ZjI2OTYyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzNiYmM2YWI3ZDkxNjQ4MzJiYmQ1ZmY1ODA0ZjdlNWQ3LmJpbmRQb3B1cChwb3B1cF9kZDJlZjUyZTEwZDU0YWVmOTcxNTNlZmNhZDU2NDRjNSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84ZjgwZDVlYWQxNzY0NDZjODg2MTNjMTYzMmQzY2U1NyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0MDgxNTcsLTc5LjM4MTc1MjI5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzZlODBhYTJiN2YzYzRmYmRhZTA2ZDE0MGI5MjAyZmFlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzk1MDM0YzVjZmFmODQ4MGViYmYzOGQ3MTJkNDRmMzA5ID0gJCgnPGRpdiBpZD0iaHRtbF85NTAzNGM1Y2ZhZjg0ODBlYmJmMzhkNzEyZDQ0ZjMwOSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SGFyYm91cmZyb250IEVhc3QsIFVuaW9uIFN0YXRpb24sIFRvcm9udG8gSXNsYW5kcyBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzZlODBhYTJiN2YzYzRmYmRhZTA2ZDE0MGI5MjAyZmFlLnNldENvbnRlbnQoaHRtbF85NTAzNGM1Y2ZhZjg0ODBlYmJmMzhkNzEyZDQ0ZjMwOSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84ZjgwZDVlYWQxNzY0NDZjODg2MTNjMTYzMmQzY2U1Ny5iaW5kUG9wdXAocG9wdXBfNmU4MGFhMmI3ZjNjNGZiZGFlMDZkMTQwYjkyMDJmYWUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTE5MjZhZmRjMTVjNDc0YWFhOWUwNDhjODgxZmVjNDYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDcxNzY4LC03OS4zODE1NzY0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lZjU2NWY0YTI3Mjk0OTYyODM4NWJjMWQ3YWJlYmM1MyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84N2ZmNjkyMjA3ZjI0YzQ1ODQwMjgwMDU0MDU1ODY1MyA9ICQoJzxkaXYgaWQ9Imh0bWxfODdmZjY5MjIwN2YyNGM0NTg0MDI4MDA1NDA1NTg2NTMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRvcm9udG8gRG9taW5pb24gQ2VudHJlLCBEZXNpZ24gRXhjaGFuZ2UgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lZjU2NWY0YTI3Mjk0OTYyODM4NWJjMWQ3YWJlYmM1My5zZXRDb250ZW50KGh0bWxfODdmZjY5MjIwN2YyNGM0NTg0MDI4MDA1NDA1NTg2NTMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTE5MjZhZmRjMTVjNDc0YWFhOWUwNDhjODgxZmVjNDYuYmluZFBvcHVwKHBvcHVwX2VmNTY1ZjRhMjcyOTQ5NjI4Mzg1YmMxZDdhYmViYzUzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzE1NTQ5NDQ1NTkzZjRiZDc5OTJmN2JhYTA5ZWM4NzFmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ4MTk4NSwtNzkuMzc5ODE2OTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTNjNjQyOTIwM2FlNDgzODgyZjE1ZDgxZDQxZjEwYmUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMGVjMzQ3ZWM1NzIzNGYyOGFlOTY3YTUwOWU3OTY5NmEgPSAkKCc8ZGl2IGlkPSJodG1sXzBlYzM0N2VjNTcyMzRmMjhhZTk2N2E1MDllNzk2OTZhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Db21tZXJjZSBDb3VydCwgVmljdG9yaWEgSG90ZWwgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hM2M2NDI5MjAzYWU0ODM4ODJmMTVkODFkNDFmMTBiZS5zZXRDb250ZW50KGh0bWxfMGVjMzQ3ZWM1NzIzNGYyOGFlOTY3YTUwOWU3OTY5NmEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMTU1NDk0NDU1OTNmNGJkNzk5MmY3YmFhMDllYzg3MWYuYmluZFBvcHVwKHBvcHVwX2EzYzY0MjkyMDNhZTQ4Mzg4MmYxNWQ4MWQ0MWYxMGJlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQyOTc5YzBjYmMyNDRjOTE5ZjZhYjRiMDUzYWJkYmFkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzExNjk0OCwtNzkuNDE2OTM1NTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYWI3NDExOTMwZjU5NGNlNmFhZDBmYzg0YmNjZWRhNTkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfY2U2ZmJmMjQ3NWFhNDNjNjk2NTM2MWMxNWY2OTE5MzEgPSAkKCc8ZGl2IGlkPSJodG1sX2NlNmZiZjI0NzVhYTQzYzY5NjUzNjFjMTVmNjkxOTMxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Sb3NlbGF3biBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2FiNzQxMTkzMGY1OTRjZTZhYWQwZmM4NGJjY2VkYTU5LnNldENvbnRlbnQoaHRtbF9jZTZmYmYyNDc1YWE0M2M2OTY1MzYxYzE1ZjY5MTkzMSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80Mjk3OWMwY2JjMjQ0YzkxOWY2YWI0YjA1M2FiZGJhZC5iaW5kUG9wdXAocG9wdXBfYWI3NDExOTMwZjU5NGNlNmFhZDBmYzg0YmNjZWRhNTkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTU0MjAzNzY5Njc4NGI1NzllZDlkNmU0MGE4MDdiNGMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTY5NDc2LC03OS40MTEzMDcyMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81Yzc5YmMyNTliOTU0MGQ5Yjg5ZTM5ODdkYjk3NzBiMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iYWE1YTJiMjdhYmM0ZDY0YTQ0ZWQ1NjMzMDRlMWU0MCA9ICQoJzxkaXYgaWQ9Imh0bWxfYmFhNWEyYjI3YWJjNGQ2NGE0NGVkNTYzMzA0ZTFlNDAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkZvcmVzdCBIaWxsIE5vcnRoICZhbXA7IFdlc3QsIEZvcmVzdCBIaWxsIFJvYWQgUGFyayBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzVjNzliYzI1OWI5NTQwZDliODllMzk4N2RiOTc3MGIwLnNldENvbnRlbnQoaHRtbF9iYWE1YTJiMjdhYmM0ZDY0YTQ0ZWQ1NjMzMDRlMWU0MCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lNTQyMDM3Njk2Nzg0YjU3OWVkOWQ2ZTQwYTgwN2I0Yy5iaW5kUG9wdXAocG9wdXBfNWM3OWJjMjU5Yjk1NDBkOWI4OWUzOTg3ZGI5NzcwYjApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWY4NDcwMzA2MzgzNDk1ZWJkOTgxZjcwYjI1ZTIyZmEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NzI3MDk3LC03OS40MDU2Nzg0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xYjgyNjAxNDcxZjM0ODYzODczZjg5NWRlZTI2ODk5ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zMjFhMGQyZTZiYWQ0MzYxYmI5MmZlNGQ3NmUzZTU4ZiA9ICQoJzxkaXYgaWQ9Imh0bWxfMzIxYTBkMmU2YmFkNDM2MWJiOTJmZTRkNzZlM2U1OGYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRoZSBBbm5leCwgTm9ydGggTWlkdG93biwgWW9ya3ZpbGxlIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMWI4MjYwMTQ3MWYzNDg2Mzg3M2Y4OTVkZWUyNjg5OWYuc2V0Q29udGVudChodG1sXzMyMWEwZDJlNmJhZDQzNjFiYjkyZmU0ZDc2ZTNlNThmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FmODQ3MDMwNjM4MzQ5NWViZDk4MWY3MGIyNWUyMmZhLmJpbmRQb3B1cChwb3B1cF8xYjgyNjAxNDcxZjM0ODYzODczZjg5NWRlZTI2ODk5Zik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84OGFmODFjZmE4ODY0MDc3YmQ3NDdiZGNlYWIzMjhhNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2MjY5NTYsLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDJkYjMyNGY0ZTZjNDM0ZDkzNmRmZGY2MTE0ZjMwZjAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZmUzN2EwMzRhYWViNDMyYzg3ZjhmNWY2YjEwNDNmMDEgPSAkKCc8ZGl2IGlkPSJodG1sX2ZlMzdhMDM0YWFlYjQzMmM4N2Y4ZjVmNmIxMDQzZjAxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Vbml2ZXJzaXR5IG9mIFRvcm9udG8sIEhhcmJvcmQgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wMmRiMzI0ZjRlNmM0MzRkOTM2ZGZkZjYxMTRmMzBmMC5zZXRDb250ZW50KGh0bWxfZmUzN2EwMzRhYWViNDMyYzg3ZjhmNWY2YjEwNDNmMDEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfODhhZjgxY2ZhODg2NDA3N2JkNzQ3YmRjZWFiMzI4YTcuYmluZFBvcHVwKHBvcHVwXzAyZGIzMjRmNGU2YzQzNGQ5MzZkZmRmNjExNGYzMGYwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2QwZTI4NDQ2MmJjNjRiNzFiMGRjZTRiZTFjMjI1NmVjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUzMjA1NywtNzkuNDAwMDQ5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81Yjk0YmY4OGRlZDQ0MjMxYmMzZjg0ZTNlNzExMTczMSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hMGQ0NzY2YzcyMzc0NzY5YjNlY2I1OGU4Y2FlNTViMSA9ICQoJzxkaXYgaWQ9Imh0bWxfYTBkNDc2NmM3MjM3NDc2OWIzZWNiNThlOGNhZTU1YjEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPktlbnNpbmd0b24gTWFya2V0LCBDaGluYXRvd24sIEdyYW5nZSBQYXJrIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNWI5NGJmODhkZWQ0NDIzMWJjM2Y4NGUzZTcxMTE3MzEuc2V0Q29udGVudChodG1sX2EwZDQ3NjZjNzIzNzQ3NjliM2VjYjU4ZThjYWU1NWIxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2QwZTI4NDQ2MmJjNjRiNzFiMGRjZTRiZTFjMjI1NmVjLmJpbmRQb3B1cChwb3B1cF81Yjk0YmY4OGRlZDQ0MjMxYmMzZjg0ZTNlNzExMTczMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83NWQzOWU2ZGY4OTk0OWY4YWMwNDZhNDY5OTc1M2U4ZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYyODk0NjcsLTc5LjM5NDQxOTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTExYzliYzQ5ZDEwNDFiZDg2M2UwZWZkMmM0YjdkMDQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNjg1YWFlOTEyMWE0NDdmY2I0NGU3Y2M1MmE3NTRhZmEgPSAkKCc8ZGl2IGlkPSJodG1sXzY4NWFhZTkxMjFhNDQ3ZmNiNDRlN2NjNTJhNzU0YWZhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DTiBUb3dlciwgS2luZyBhbmQgU3BhZGluYSwgUmFpbHdheSBMYW5kcywgSGFyYm91cmZyb250IFdlc3QsIEJhdGh1cnN0IFF1YXksIFNvdXRoIE5pYWdhcmEsIElzbGFuZCBhaXJwb3J0IENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTExYzliYzQ5ZDEwNDFiZDg2M2UwZWZkMmM0YjdkMDQuc2V0Q29udGVudChodG1sXzY4NWFhZTkxMjFhNDQ3ZmNiNDRlN2NjNTJhNzU0YWZhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzc1ZDM5ZTZkZjg5OTQ5ZjhhYzA0NmE0Njk5NzUzZThmLmJpbmRQb3B1cChwb3B1cF9lMTFjOWJjNDlkMTA0MWJkODYzZTBlZmQyYzRiN2QwNCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84ODBiOTM3NDFkYWU0YThjOTFlNjcwZmI5OTQyZWJkMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0NjQzNTIsLTc5LjM3NDg0NTk5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzQyYzU0NTkzMmU5ZjQyZTZiMDA0MTAwNWMzYTFhMTg3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2NkZDY0YzBlMzQxNzRlNDhiOTQzZmFlNDNkOTM5NWZlID0gJCgnPGRpdiBpZD0iaHRtbF9jZGQ2NGMwZTM0MTc0ZTQ4Yjk0M2ZhZTQzZDkzOTVmZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3RuIEEgUE8gQm94ZXMgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80MmM1NDU5MzJlOWY0MmU2YjAwNDEwMDVjM2ExYTE4Ny5zZXRDb250ZW50KGh0bWxfY2RkNjRjMGUzNDE3NGU0OGI5NDNmYWU0M2Q5Mzk1ZmUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfODgwYjkzNzQxZGFlNGE4YzkxZTY3MGZiOTk0MmViZDAuYmluZFBvcHVwKHBvcHVwXzQyYzU0NTkzMmU5ZjQyZTZiMDA0MTAwNWMzYTFhMTg3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2FmN2JlOTY0ZDk2MDQ4ZmM4ZDdiZDVlMGQyYzEyNDFkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ4NDI5MiwtNzkuMzgyMjgwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yN2E5MTUzNTVhMDM0Zjc0OTkwMDZiYjQzYzMyODhkMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83MmZlNjNiODMyZDY0MTIxYjJkODlhZGQyNGViYzc5NSA9ICQoJzxkaXYgaWQ9Imh0bWxfNzJmZTYzYjgzMmQ2NDEyMWIyZDg5YWRkMjRlYmM3OTUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkZpcnN0IENhbmFkaWFuIFBsYWNlLCBVbmRlcmdyb3VuZCBjaXR5IENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMjdhOTE1MzU1YTAzNGY3NDk5MDA2YmI0M2MzMjg4ZDIuc2V0Q29udGVudChodG1sXzcyZmU2M2I4MzJkNjQxMjFiMmQ4OWFkZDI0ZWJjNzk1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FmN2JlOTY0ZDk2MDQ4ZmM4ZDdiZDVlMGQyYzEyNDFkLmJpbmRQb3B1cChwb3B1cF8yN2E5MTUzNTVhMDM0Zjc0OTkwMDZiYjQzYzMyODhkMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zMDljN2VmNzI5MWU0Njc4ODdmOTUwMjZmNmQwZWM1YSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2OTU0MiwtNzkuNDIyNTYzN10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zNDg4Y2E0NGQ2ZTY0OGQzOTYyZWY3ODM0MDZmNzcwMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81MjY5MWMxMjY2YTk0YjhmODM2N2U1YzgyNzYwNWJiNSA9ICQoJzxkaXYgaWQ9Imh0bWxfNTI2OTFjMTI2NmE5NGI4ZjgzNjdlNWM4Mjc2MDViYjUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNocmlzdGllIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMzQ4OGNhNDRkNmU2NDhkMzk2MmVmNzgzNDA2Zjc3MDMuc2V0Q29udGVudChodG1sXzUyNjkxYzEyNjZhOTRiOGY4MzY3ZTVjODI3NjA1YmI1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzMwOWM3ZWY3MjkxZTQ2Nzg4N2Y5NTAyNmY2ZDBlYzVhLmJpbmRQb3B1cChwb3B1cF8zNDg4Y2E0NGQ2ZTY0OGQzOTYyZWY3ODM0MDZmNzcwMyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mZjJjYTcwNzZmOTU0YmMzYWQyN2U0OGIxYTliZjI2NyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2OTAwNTEwMDAwMDAxLC03OS40NDIyNTkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzNjMDVjZDdkNDM4NzRiOTlhY2FhOWE0NjJjM2ZkZjU1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2UwMDRhMTY5OTJlYjRkYzViNzAxZTUyODkwZjFjNTg4ID0gJCgnPGRpdiBpZD0iaHRtbF9lMDA0YTE2OTkyZWI0ZGM1YjcwMWU1Mjg5MGYxYzU4OCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RHVmZmVyaW4sIERvdmVyY291cnQgVmlsbGFnZSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzNjMDVjZDdkNDM4NzRiOTlhY2FhOWE0NjJjM2ZkZjU1LnNldENvbnRlbnQoaHRtbF9lMDA0YTE2OTkyZWI0ZGM1YjcwMWU1Mjg5MGYxYzU4OCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9mZjJjYTcwNzZmOTU0YmMzYWQyN2U0OGIxYTliZjI2Ny5iaW5kUG9wdXAocG9wdXBfM2MwNWNkN2Q0Mzg3NGI5OWFjYWE5YTQ2MmMzZmRmNTUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjlkYTU0Nzg3ZjM4NDY0OGFlNmFhZWRkZWJjNjFiNjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDc5MjY3MDAwMDAwMDYsLTc5LjQxOTc0OTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMTdlOWI0Yjc5NmE5NDA2Yzg3YTlkM2I2MWE5N2U5ZTcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZDFlNWJkYTU4NzE2NGEyMWIwNjRiMDc2OTg4MWZiZjggPSAkKCc8ZGl2IGlkPSJodG1sX2QxZTViZGE1ODcxNjRhMjFiMDY0YjA3Njk4ODFmYmY4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MaXR0bGUgUG9ydHVnYWwsIFRyaW5pdHkgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xN2U5YjRiNzk2YTk0MDZjODdhOWQzYjYxYTk3ZTllNy5zZXRDb250ZW50KGh0bWxfZDFlNWJkYTU4NzE2NGEyMWIwNjRiMDc2OTg4MWZiZjgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMjlkYTU0Nzg3ZjM4NDY0OGFlNmFhZWRkZWJjNjFiNjIuYmluZFBvcHVwKHBvcHVwXzE3ZTliNGI3OTZhOTQwNmM4N2E5ZDNiNjFhOTdlOWU3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U2MTg2MmY0MjIwOTQ4M2JhODA5ZjJmZWM4ZjBjZmY0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjM2ODQ3MiwtNzkuNDI4MTkxNDAwMDAwMDJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmNjYzRkMGRiYTI4NDU1YmE4YmU2ZDhhMjQ1OGQzNmQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjlmZDgzNmM2NmU1NDZiMDhjNTE4MTNlYzM4MzU4NGEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOGVjY2Q5Yzg4YjJiNDc0N2JkOGExMjE4ZDIxYmY2ZDkgPSAkKCc8ZGl2IGlkPSJodG1sXzhlY2NkOWM4OGIyYjQ3NDdiZDhhMTIxOGQyMWJmNmQ5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ccm9ja3RvbiwgUGFya2RhbGUgVmlsbGFnZSwgRXhoaWJpdGlvbiBQbGFjZSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2I5ZmQ4MzZjNjZlNTQ2YjA4YzUxODEzZWMzODM1ODRhLnNldENvbnRlbnQoaHRtbF84ZWNjZDljODhiMmI0NzQ3YmQ4YTEyMThkMjFiZjZkOSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lNjE4NjJmNDIyMDk0ODNiYTgwOWYyZmVjOGYwY2ZmNC5iaW5kUG9wdXAocG9wdXBfYjlmZDgzNmM2NmU1NDZiMDhjNTE4MTNlYzM4MzU4NGEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODAxYTdjMTYxNTJlNDkzY2I1YjUyMzgyOTg4ODZlMDYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjE2MDgzLC03OS40NjQ3NjMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mYTJmNTMzY2YyZDI0ZDZjYWM0Y2I4MzgxNDQ5ODBlOCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xODhlZTc5NTVjYWM0M2I4YjAzODEzMzdlZDFhMDIzOCA9ICQoJzxkaXYgaWQ9Imh0bWxfMTg4ZWU3OTU1Y2FjNDNiOGIwMzgxMzM3ZWQxYTAyMzgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkhpZ2ggUGFyaywgVGhlIEp1bmN0aW9uIFNvdXRoIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmEyZjUzM2NmMmQyNGQ2Y2FjNGNiODM4MTQ0OTgwZTguc2V0Q29udGVudChodG1sXzE4OGVlNzk1NWNhYzQzYjhiMDM4MTMzN2VkMWEwMjM4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzgwMWE3YzE2MTUyZTQ5M2NiNWI1MjM4Mjk4ODg2ZTA2LmJpbmRQb3B1cChwb3B1cF9mYTJmNTMzY2YyZDI0ZDZjYWM0Y2I4MzgxNDQ5ODBlOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wYzNkNTg3NWEzNmY0MTdhOTM4NGE1MTNhNDgwNjc4OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0ODk1OTcsLTc5LjQ1NjMyNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yOWY1MWE3ZmM4NDY0MmNlODk2NDg5ODg2N2YyNzQ3NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83NjVlZGNmY2I4MmQ0OTFjYjY4MGY0Mjk2OTM4NGJmZCA9ICQoJzxkaXYgaWQ9Imh0bWxfNzY1ZWRjZmNiODJkNDkxY2I2ODBmNDI5NjkzODRiZmQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlBhcmtkYWxlLCBSb25jZXN2YWxsZXMgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yOWY1MWE3ZmM4NDY0MmNlODk2NDg5ODg2N2YyNzQ3Ny5zZXRDb250ZW50KGh0bWxfNzY1ZWRjZmNiODJkNDkxY2I2ODBmNDI5NjkzODRiZmQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGMzZDU4NzVhMzZmNDE3YTkzODRhNTEzYTQ4MDY3ODkuYmluZFBvcHVwKHBvcHVwXzI5ZjUxYTdmYzg0NjQyY2U4OTY0ODk4ODY3ZjI3NDc3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2I2NzNhZTQ1ZjVkNDQ2NDk5Mzc2ZTBhMDNjOGM1ZTg3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUxNTcwNiwtNzkuNDg0NDQ5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mY2NjNGQwZGJhMjg0NTViYThiZTZkOGEyNDU4ZDM2ZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mNzZjODkxNTczZWI0MTgxYjFiMGM0YmU3ZDU5NjljMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85NTQ3M2I0NzI5ZTY0ZGEwODU2NDliY2UwNTQ2ZjI5NyA9ICQoJzxkaXYgaWQ9Imh0bWxfOTU0NzNiNDcyOWU2NGRhMDg1NjQ5YmNlMDU0NmYyOTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJ1bm55bWVkZSwgU3dhbnNlYSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Y3NmM4OTE1NzNlYjQxODFiMWIwYzRiZTdkNTk2OWMyLnNldENvbnRlbnQoaHRtbF85NTQ3M2I0NzI5ZTY0ZGEwODU2NDliY2UwNTQ2ZjI5Nyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iNjczYWU0NWY1ZDQ0NjQ5OTM3NmUwYTAzYzhjNWU4Ny5iaW5kUG9wdXAocG9wdXBfZjc2Yzg5MTU3M2ViNDE4MWIxYjBjNGJlN2Q1OTY5YzIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZGEzODBkNjk2Mzg0NDNkOGIzNDkyYjVkY2Y3MWNhYmQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjIzMDE1LC03OS4zODk0OTM4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzRiOTI5MGYyYmM1YzRlYThhNTIzZDk3ZjdmZWYyMDI4ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzVlYmUxMzE5NjAyNjQyNGViYWEzZjQ5ZTI1ZDU2MTk2ID0gJCgnPGRpdiBpZD0iaHRtbF81ZWJlMTMxOTYwMjY0MjRlYmFhM2Y0OWUyNWQ1NjE5NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UXVlZW4mIzM5O3MgUGFyaywgT250YXJpbyBQcm92aW5jaWFsIEdvdmVybm1lbnQgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80YjkyOTBmMmJjNWM0ZWE4YTUyM2Q5N2Y3ZmVmMjAyOC5zZXRDb250ZW50KGh0bWxfNWViZTEzMTk2MDI2NDI0ZWJhYTNmNDllMjVkNTYxOTYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZGEzODBkNjk2Mzg0NDNkOGIzNDkyYjVkY2Y3MWNhYmQuYmluZFBvcHVwKHBvcHVwXzRiOTI5MGYyYmM1YzRlYThhNTIzZDk3ZjdmZWYyMDI4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2RjYzk4YzMzM2VkNTRkODViMzE4ODJiYjg5YmMzYTU5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjYyNzQzOSwtNzkuMzIxNTU4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZjY2M0ZDBkYmEyODQ1NWJhOGJlNmQ4YTI0NThkMzZkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzU5OGNkZGU3MmRmNzQ5NDhhYzk5ZDlhYTRhZDI4MWQwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2I5ZDNjNTZkYjZmMTQzMDdiMjcyZWE1MTE2ZTI3NTUyID0gJCgnPGRpdiBpZD0iaHRtbF9iOWQzYzU2ZGI2ZjE0MzA3YjI3MmVhNTExNmUyNzU1MiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QnVzaW5lc3MgcmVwbHkgbWFpbCBQcm9jZXNzaW5nIENlbnRyZSwgU291dGggQ2VudHJhbCBMZXR0ZXIgUHJvY2Vzc2luZyBQbGFudCBUb3JvbnRvIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTk4Y2RkZTcyZGY3NDk0OGFjOTlkOWFhNGFkMjgxZDAuc2V0Q29udGVudChodG1sX2I5ZDNjNTZkYjZmMTQzMDdiMjcyZWE1MTE2ZTI3NTUyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2RjYzk4YzMzM2VkNTRkODViMzE4ODJiYjg5YmMzYTU5LmJpbmRQb3B1cChwb3B1cF81OThjZGRlNzJkZjc0OTQ4YWM5OWQ5YWE0YWQyODFkMCk7CgogICAgICAgICAgICAKICAgICAgICAKPC9zY3JpcHQ+ onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



The different colours here indicate the various boroughs in the city of Toronto. Based on the location of a particular  borough we plot the map.
