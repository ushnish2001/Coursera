THE TASKS OF WEB SCRAPING,CLEANING ARE IMPLEMENTED IN THE SAME NOTEBOOK FOR THE EASE OF EVALUATION


```python
!pip install beautifulsoup4
!pip install lxml
import requests # library to handle requests
import pandas as pd # library for data analsysis
import numpy as np # library to handle data in a vectorized manner
import random # library for random number generation

#!conda install -c conda-forge geopy --yes 
from geopy.geocoders import Nominatim # module to convert an address into latitude and longitude values

# libraries for displaying images
from IPython.display import Image 
from IPython.core.display import HTML 


from IPython.display import display_html
import pandas as pd
import numpy as np
    
# tranforming json file into a pandas dataframe library
from pandas.io.json import json_normalize

!conda install -c conda-forge folium=0.5.0 --yes
import folium # plotting library
from bs4 import BeautifulSoup
from sklearn.cluster import KMeans
import matplotlib.cm as cm
import matplotlib.colors as colors

print('Folium and Libraries Installed')
```

    Requirement already satisfied: beautifulsoup4 in /opt/anaconda3/lib/python3.7/site-packages (4.8.2)
    Requirement already satisfied: soupsieve>=1.2 in /opt/anaconda3/lib/python3.7/site-packages (from beautifulsoup4) (1.9.5)
    Requirement already satisfied: lxml in /opt/anaconda3/lib/python3.7/site-packages (4.5.0)
    Collecting package metadata (current_repodata.json): done
    Solving environment: / 
    Warning: 4 possible package resolutions (only showing differing packages):
      - anaconda/osx-64::ca-certificates-2020.1.1-0, anaconda/osx-64::openssl-1.1.1d-h1de35cc_4
      - anaconda/osx-64::ca-certificates-2020.1.1-0, defaults/osx-64::openssl-1.1.1d-h1de35cc_4
      - anaconda/osx-64::openssl-1.1.1d-h1de35cc_4, defaults/osx-64::ca-certificates-2020.1.1-0
      - defaults/osx-64::ca-certificates-2020.1.1-0, defaults/osx-64::openssl-1.1.1d-h1de35ccdone
    
    # All requested packages already installed.
    
    Folium and Libraries Installed



```python
source = requests.get('https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M').text
soup=BeautifulSoup(source,'lxml')
print(soup.title)
from IPython.display import display_html
tab = str(soup.table)
display_html(tab,raw=True)
```

    <title>List of postal codes of Canada: M - Wikipedia</title>



<table class="wikitable sortable">
<tbody><tr>
<th>Post Code
</th>
<th>Borough
</th>
<th>Neighborhood
</th></tr>
<tr>
<td>M1A
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M2A
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3A
</td>
<td>North York
</td>
<td>Parkwoods
</td></tr>
<tr>
<td>M4A
</td>
<td>North York
</td>
<td>Victoria Village
</td></tr>
<tr>
<td>M5A
</td>
<td>Downtown Toronto
</td>
<td>Regent Park, Harbourfront
</td></tr>
<tr>
<td>M6A
</td>
<td>North York
</td>
<td>Lawrence Manor, Lawrence Heights
</td></tr>
<tr>
<td>M7A
</td>
<td>Downtown Toronto
</td>
<td>Queen's Park, Ontario Provincial Government
</td></tr>
<tr>
<td>M8A
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9A
</td>
<td>Etobicoke
</td>
<td>Islington Avenue, Humber Valley Village
</td></tr>
<tr>
<td>M1B
</td>
<td>Scarborough
</td>
<td>Malvern, Rouge
</td></tr>
<tr>
<td>M2B
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3B
</td>
<td>North York
</td>
<td>Don Mills
</td></tr>
<tr>
<td>M4B
</td>
<td>East York
</td>
<td>Parkview Hill, Woodbine Gardens
</td></tr>
<tr>
<td>M5B
</td>
<td>Downtown Toronto
</td>
<td>Garden District, Ryerson
</td></tr>
<tr>
<td>M6B
</td>
<td>North York
</td>
<td>Glencairn
</td></tr>
<tr>
<td>M7B
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8B
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9B
</td>
<td>Etobicoke
</td>
<td>West Deane Park, Princess Gardens, Martin Grove, Islington, Cloverdale
</td></tr>
<tr>
<td>M1C
</td>
<td>Scarborough
</td>
<td>Rouge Hill, Port Union, Highland Creek
</td></tr>
<tr>
<td>M2C
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3C
</td>
<td>North York
</td>
<td>Don Mills
</td></tr>
<tr>
<td>M4C
</td>
<td>East York
</td>
<td>Woodbine Heights
</td></tr>
<tr>
<td>M5C
</td>
<td>Downtown Toronto
</td>
<td>St. James Town
</td></tr>
<tr>
<td>M6C
</td>
<td>York
</td>
<td>Humewood-Cedarvale
</td></tr>
<tr>
<td>M7C
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8C
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9C
</td>
<td>Etobicoke
</td>
<td>Eringate, Bloordale Gardens, Old Burnhamthorpe, Markland Wood
</td></tr>
<tr>
<td>M1E
</td>
<td>Scarborough
</td>
<td>Guildwood, Morningside, West Hill
</td></tr>
<tr>
<td>M2E
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3E
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4E
</td>
<td>East Toronto
</td>
<td>The Beaches
</td></tr>
<tr>
<td>M5E
</td>
<td>Downtown Toronto
</td>
<td>Berczy Park
</td></tr>
<tr>
<td>M6E
</td>
<td>York
</td>
<td>Caledonia-Fairbanks
</td></tr>
<tr>
<td>M7E
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8E
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9E
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1G
</td>
<td>Scarborough
</td>
<td>Woburn
</td></tr>
<tr>
<td>M2G
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3G
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4G
</td>
<td>East York
</td>
<td>Leaside
</td></tr>
<tr>
<td>M5G
</td>
<td>Downtown Toronto
</td>
<td>Central Bay Street
</td></tr>
<tr>
<td>M6G
</td>
<td>Downtown Toronto
</td>
<td>Christie
</td></tr>
<tr>
<td>M7G
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8G
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9G
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1H
</td>
<td>Scarborough
</td>
<td>Cedarbrae
</td></tr>
<tr>
<td>M2H
</td>
<td>North York
</td>
<td>Hillcrest Village
</td></tr>
<tr>
<td>M3H
</td>
<td>North York
</td>
<td>Bathurst Manor, Wilson Heights, Downsview North
</td></tr>
<tr>
<td>M4H
</td>
<td>East York
</td>
<td>Thorncliffe Park
</td></tr>
<tr>
<td>M5H
</td>
<td>Downtown Toronto
</td>
<td>Richmond, Adelaide, King
</td></tr>
<tr>
<td>M6H
</td>
<td>West Toronto
</td>
<td>Dufferin, Dovercourt Village
</td></tr>
<tr>
<td>M7H
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8H
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9H
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1J
</td>
<td>Scarborough
</td>
<td>Scarborough Village
</td></tr>
<tr>
<td>M2J
</td>
<td>North York
</td>
<td>Fairview, Henry Farm, Oriole
</td></tr>
<tr>
<td>M3J
</td>
<td>North York
</td>
<td>Northwood Park, York University
</td></tr>
<tr>
<td>M4J
</td>
<td>East York
</td>
<td>East Toronto, Broadview North (Old East York)
</td></tr>
<tr>
<td>M5J
</td>
<td>Downtown Toronto
</td>
<td>Harbourfront East, Union Station, Toronto Islands
</td></tr>
<tr>
<td>M6J
</td>
<td>West Toronto
</td>
<td>Little Portugal, Trinity
</td></tr>
<tr>
<td>M7J
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8J
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9J
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1K
</td>
<td>Scarborough
</td>
<td>Kennedy Park, Ionview, East Birchmount Park
</td></tr>
<tr>
<td>M2K
</td>
<td>North York
</td>
<td>Bayview Village
</td></tr>
<tr>
<td>M3K
</td>
<td>North York
</td>
<td>Downsview
</td></tr>
<tr>
<td>M4K
</td>
<td>East Toronto
</td>
<td>The Danforth West, Riverdale
</td></tr>
<tr>
<td>M5K
</td>
<td>Downtown Toronto
</td>
<td>Toronto Dominion Centre, Design Exchange
</td></tr>
<tr>
<td>M6K
</td>
<td>West Toronto
</td>
<td>Brockton, Parkdale Village, Exhibition Place
</td></tr>
<tr>
<td>M7K
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8K
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9K
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1L
</td>
<td>Scarborough
</td>
<td>Golden Mile, Clairlea, Oakridge
</td></tr>
<tr>
<td>M2L
</td>
<td>North York
</td>
<td>York Mills, Silver Hills
</td></tr>
<tr>
<td>M3L
</td>
<td>North York
</td>
<td>Downsview
</td></tr>
<tr>
<td>M4L
</td>
<td>East Toronto
</td>
<td>India Bazaar, The Beaches West
</td></tr>
<tr>
<td>M5L
</td>
<td>Downtown Toronto
</td>
<td>Commerce Court, Victoria Hotel
</td></tr>
<tr>
<td>M6L
</td>
<td>North York
</td>
<td>North Park, Maple Leaf Park, Upwood Park
</td></tr>
<tr>
<td>M7L
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8L
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9L
</td>
<td>North York
</td>
<td>Humber Summit
</td></tr>
<tr>
<td>M1M
</td>
<td>Scarborough
</td>
<td>Cliffside, Cliffcrest, Scarborough Village West
</td></tr>
<tr>
<td>M2M
</td>
<td>North York
</td>
<td>Willowdale, Newtonbrook
</td></tr>
<tr>
<td>M3M
</td>
<td>North York
</td>
<td>Downsview
</td></tr>
<tr>
<td>M4M
</td>
<td>East Toronto
</td>
<td>Studio District
</td></tr>
<tr>
<td>M5M
</td>
<td>North York
</td>
<td>Bedford Park, Lawrence Manor East
</td></tr>
<tr>
<td>M6M
</td>
<td>York
</td>
<td>Del Ray, Mount Dennis, Keelsdale and Silverthorn
</td></tr>
<tr>
<td>M7M
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8M
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9M
</td>
<td>North York
</td>
<td>Humberlea, Emery
</td></tr>
<tr>
<td>M1N
</td>
<td>Scarborough
</td>
<td>Birch Cliff, Cliffside West
</td></tr>
<tr>
<td>M2N
</td>
<td>North York
</td>
<td>Willowdale, Willowdale East
</td></tr>
<tr>
<td>M3N
</td>
<td>North York
</td>
<td>Downsview
</td></tr>
<tr>
<td>M4N
</td>
<td>Central Toronto
</td>
<td>Lawrence Park
</td></tr>
<tr>
<td>M5N
</td>
<td>Central Toronto
</td>
<td>Roselawn
</td></tr>
<tr>
<td>M6N
</td>
<td>York
</td>
<td>Runnymede, The Junction North
</td></tr>
<tr>
<td>M7N
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8N
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9N
</td>
<td>York
</td>
<td>Weston
</td></tr>
<tr>
<td>M1P
</td>
<td>Scarborough
</td>
<td>Dorset Park, Wexford Heights, Scarborough Town Centre
</td></tr>
<tr>
<td>M2P
</td>
<td>North York
</td>
<td>York Mills West
</td></tr>
<tr>
<td>M3P
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4P
</td>
<td>Central Toronto
</td>
<td>Davisville North
</td></tr>
<tr>
<td>M5P
</td>
<td>Central Toronto
</td>
<td>Forest Hill North &amp; West, Forest Hill Road Park
</td></tr>
<tr>
<td>M6P
</td>
<td>West Toronto
</td>
<td>High Park, The Junction South
</td></tr>
<tr>
<td>M7P
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8P
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9P
</td>
<td>Etobicoke
</td>
<td>Westmount
</td></tr>
<tr>
<td>M1R
</td>
<td>Scarborough
</td>
<td>Wexford, Maryvale
</td></tr>
<tr>
<td>M2R
</td>
<td>North York
</td>
<td>Willowdale, Willowdale West
</td></tr>
<tr>
<td>M3R
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4R
</td>
<td>Central Toronto
</td>
<td>North Toronto West,  Lawrence Park
</td></tr>
<tr>
<td>M5R
</td>
<td>Central Toronto
</td>
<td>The Annex, North Midtown, Yorkville
</td></tr>
<tr>
<td>M6R
</td>
<td>West Toronto
</td>
<td>Parkdale, Roncesvalles
</td></tr>
<tr>
<td>M7R
</td>
<td>Mississauga
</td>
<td>Canada Post Gateway Processing Centre
</td></tr>
<tr>
<td>M8R
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9R
</td>
<td>Etobicoke
</td>
<td>Kingsview Village, St. Phillips, Martin Grove Gardens, Richview Gardens
</td></tr>
<tr>
<td>M1S
</td>
<td>Scarborough
</td>
<td>Agincourt
</td></tr>
<tr>
<td>M2S
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3S
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4S
</td>
<td>Central Toronto
</td>
<td>Davisville
</td></tr>
<tr>
<td>M5S
</td>
<td>Downtown Toronto
</td>
<td>University of Toronto, Harbord
</td></tr>
<tr>
<td>M6S
</td>
<td>West Toronto
</td>
<td>Runnymede, Swansea
</td></tr>
<tr>
<td>M7S
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8S
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9S
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1T
</td>
<td>Scarborough
</td>
<td>Clarks Corners, Tam O'Shanter, Sullivan
</td></tr>
<tr>
<td>M2T
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3T
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4T
</td>
<td>Central Toronto
</td>
<td>Moore Park, Summerhill East
</td></tr>
<tr>
<td>M5T
</td>
<td>Downtown Toronto
</td>
<td>Kensington Market, Chinatown, Grange Park
</td></tr>
<tr>
<td>M6T
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M7T
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8T
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M9T
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1V
</td>
<td>Scarborough
</td>
<td>Milliken, Agincourt North, Steeles East, L'Amoreaux East
</td></tr>
<tr>
<td>M2V
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3V
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4V
</td>
<td>Central Toronto
</td>
<td>Summerhill West, Rathnelly, South Hill, Forest Hill SE, Deer Park
</td></tr>
<tr>
<td>M5V
</td>
<td>Downtown Toronto
</td>
<td>CN Tower, King and Spadina, Railway Lands, Harbourfront West, Bathurst Quay, South Niagara, Island airport
</td></tr>
<tr>
<td>M6V
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M7V
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8V
</td>
<td>Etobicoke
</td>
<td>New Toronto, Mimico South, Humber Bay Shores
</td></tr>
<tr>
<td>M9V
</td>
<td>Etobicoke
</td>
<td>South Steeles, Silverstone, Humbergate, Jamestown, Mount Olive, Beaumond Heights, Thistletown, Albion Gardens
</td></tr>
<tr>
<td>M1W
</td>
<td>Scarborough
</td>
<td>Steeles West, L'Amoreaux West
</td></tr>
<tr>
<td>M2W
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3W
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4W
</td>
<td>Downtown Toronto
</td>
<td>Rosedale
</td></tr>
<tr>
<td>M5W
</td>
<td>Downtown Toronto
</td>
<td>Stn A PO Boxes
</td></tr>
<tr>
<td>M6W
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M7W
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8W
</td>
<td>Etobicoke
</td>
<td>Alderwood, Long Branch
</td></tr>
<tr>
<td>M9W
</td>
<td>Etobicoke
</td>
<td>Northwest, West Humber - Clairville
</td></tr>
<tr>
<td>M1X
</td>
<td>Scarborough
</td>
<td>Upper Rouge
</td></tr>
<tr>
<td>M2X
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3X
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4X
</td>
<td>Downtown Toronto
</td>
<td>St. James Town, Cabbagetown
</td></tr>
<tr>
<td>M5X
</td>
<td>Downtown Toronto
</td>
<td>First Canadian Place, Underground city
</td></tr>
<tr>
<td>M6X
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M7X
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8X
</td>
<td>Etobicoke
</td>
<td>The Kingsway, Montgomery Road, Old Mill North
</td></tr>
<tr>
<td>M9X
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1Y
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M2Y
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3Y
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4Y
</td>
<td>Downtown Toronto
</td>
<td>Church and Wellesley
</td></tr>
<tr>
<td>M5Y
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M6Y
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M7Y
</td>
<td>East Toronto
</td>
<td>Business reply mail Processing Centre, South Central Letter Processing Plant Toronto
</td></tr>
<tr>
<td>M8Y
</td>
<td>Etobicoke
</td>
<td>Old Mill South, King's Mill Park, Sunnylea, Humber Bay, Mimico NE, The Queensway East, Royal York South East, Kingsway Park South East
</td></tr>
<tr>
<td>M9Y
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M1Z
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M2Z
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3Z
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M4Z
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M5Z
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M6Z
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M7Z
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr>
<tr>
<td>M8Z
</td>
<td>Etobicoke
</td>
<td>Mimico NW, The Queensway West, South of Bloor, Kingsway Park South West, Royal York South West
</td></tr>
<tr>
<td>M9Z
</td>
<td>Not assigned
</td>
<td>Not assigned
</td></tr></tbody></table>



```python
dfs = pd.read_html(tab)
df=dfs[0]
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
      <th>Post Code</th>
      <th>Borough</th>
      <th>Neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M2A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
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
  </tbody>
</table>
</div>




```python
df1 = df[df.Borough != 'Not assigned']

# Combining the neighbourhoods with same Postalcode
df2 = df1.groupby(['Post Code','Borough'], sort=False).agg(', '.join)
df2.reset_index(inplace=True)
df2.rename(columns={'Post Code':'Postcode'},inplace=True)

# Replacing the name of the neighbourhoods which are 'Not assigned' with names of Borough
#df2['Neighbourhood'] = np.where(df2['Neighbourhood'] == 'Not assigned',df2['Borough'], df2['Neighbourhood'])

df2
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
      <th>Postcode</th>
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
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>98</th>
      <td>M8X</td>
      <td>Etobicoke</td>
      <td>The Kingsway, Montgomery Road, Old Mill North</td>
    </tr>
    <tr>
      <th>99</th>
      <td>M4Y</td>
      <td>Downtown Toronto</td>
      <td>Church and Wellesley</td>
    </tr>
    <tr>
      <th>100</th>
      <td>M7Y</td>
      <td>East Toronto</td>
      <td>Business reply mail Processing Centre, South C...</td>
    </tr>
    <tr>
      <th>101</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Old Mill South, King's Mill Park, Sunnylea, Hu...</td>
    </tr>
    <tr>
      <th>102</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>Mimico NW, The Queensway West, South of Bloor,...</td>
    </tr>
  </tbody>
</table>
<p>103 rows × 3 columns</p>
</div>




```python
df2.shape
```




    (103, 3)




```python
lat_lon = pd.read_csv('https://cocl.us/Geospatial_data')
lat_lon.head()
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
lat_lon.rename(columns={'Postal Code':'Postcode'},inplace=True)
df3 = pd.merge(df2,lat_lon,on='Postcode')
df3.head()
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
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
      <td>43.753259</td>
      <td>-79.329656</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
      <td>43.725882</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
      <td>43.718518</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
      <td>43.662301</td>
      <td>-79.389494</td>
    </tr>
  </tbody>
</table>
</div>




```python
df3
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
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighborhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
      <td>43.753259</td>
      <td>-79.329656</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
      <td>43.725882</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
      <td>43.718518</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
      <td>43.662301</td>
      <td>-79.389494</td>
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
      <th>98</th>
      <td>M8X</td>
      <td>Etobicoke</td>
      <td>The Kingsway, Montgomery Road, Old Mill North</td>
      <td>43.653654</td>
      <td>-79.506944</td>
    </tr>
    <tr>
      <th>99</th>
      <td>M4Y</td>
      <td>Downtown Toronto</td>
      <td>Church and Wellesley</td>
      <td>43.665860</td>
      <td>-79.383160</td>
    </tr>
    <tr>
      <th>100</th>
      <td>M7Y</td>
      <td>East Toronto</td>
      <td>Business reply mail Processing Centre, South C...</td>
      <td>43.662744</td>
      <td>-79.321558</td>
    </tr>
    <tr>
      <th>101</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Old Mill South, King's Mill Park, Sunnylea, Hu...</td>
      <td>43.636258</td>
      <td>-79.498509</td>
    </tr>
    <tr>
      <th>102</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>Mimico NW, The Queensway West, South of Bloor,...</td>
      <td>43.628841</td>
      <td>-79.520999</td>
    </tr>
  </tbody>
</table>
<p>103 rows × 5 columns</p>
</div>




```python
map_toronto = folium.Map(location=[43.651070,-79.347015],zoom_start=10)

for lat,lng,borough in zip(df3['Latitude'],df3['Longitude'],df3['Borough']):
    label = '{}'.format( borough)
    label = folium.Popup(label, parse_html=True)
    folium.CircleMarker(
    [lat,lng],
    radius=5,
    popup=label,
    color='blue',
    fill=True,
    fill_color='#3186cc',
    fill_opacity=0.7,
    parse_html=False).add_to(map_toronto)
map_toronto
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDAgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCcsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDMuNjUxMDcsLTc5LjM0NzAxNV0sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMCwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfZTZjMDgzY2Y1YmU5NGVkN2E3NmEyZTQ2MDA1ZjdmNTYgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzc2NGMzZWVjYzhjYTQ4OWNhMDI5OTY2NjJkYmY1MDUxID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzUzMjU4NiwtNzkuMzI5NjU2NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80YzY4MDhkZTAwZTM0MWY1YmMwYThhYmY3NzM1NzYxZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85NjFiMTZmNzU2NDc0MzM1OWJlZTgxMTZiNzk0NDNhNiA9ICQoJzxkaXYgaWQ9Imh0bWxfOTYxYjE2Zjc1NjQ3NDMzNTliZWU4MTE2Yjc5NDQzYTYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzRjNjgwOGRlMDBlMzQxZjViYzBhOGFiZjc3MzU3NjFlLnNldENvbnRlbnQoaHRtbF85NjFiMTZmNzU2NDc0MzM1OWJlZTgxMTZiNzk0NDNhNik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83NjRjM2VlY2M4Y2E0ODljYTAyOTk2NjYyZGJmNTA1MS5iaW5kUG9wdXAocG9wdXBfNGM2ODA4ZGUwMGUzNDFmNWJjMGE4YWJmNzczNTc2MWUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNmNhYzdmMmE5ODUzNGMyOGIwMjUyMDlhZGRmZGZmOTMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MjU4ODIyOTk5OTk5OTUsLTc5LjMxNTU3MTU5OTk5OTk4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzkxMmVjYjg2YTJjYzQ4MzM4MWZhN2U2MmIwYzZkOWMxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2RlZjU2YTNmZTE0YTRkN2Q5OTMwNWZjODc2MzUzNjM4ID0gJCgnPGRpdiBpZD0iaHRtbF9kZWY1NmEzZmUxNGE0ZDdkOTkzMDVmYzg3NjM1MzYzOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOTEyZWNiODZhMmNjNDgzMzgxZmE3ZTYyYjBjNmQ5YzEuc2V0Q29udGVudChodG1sX2RlZjU2YTNmZTE0YTRkN2Q5OTMwNWZjODc2MzUzNjM4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzZjYWM3ZjJhOTg1MzRjMjhiMDI1MjA5YWRkZmRmZjkzLmJpbmRQb3B1cChwb3B1cF85MTJlY2I4NmEyY2M0ODMzODFmYTdlNjJiMGM2ZDljMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mODcyOTRhZjRlMjA0NWYwOWFjYmYyMjNiMGI4NGY2YSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1NDI1OTksLTc5LjM2MDYzNTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTZmZTFlYzg3Y2JhNDIyMGFkMGQ0MTBkOTlhNzczZjcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMjc5MmUzNDUyYjZlNDI1NWIxZjgzYTY5Y2NkMTlkOGIgPSAkKCc8ZGl2IGlkPSJodG1sXzI3OTJlMzQ1MmI2ZTQyNTViMWY4M2E2OWNjZDE5ZDhiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81NmZlMWVjODdjYmE0MjIwYWQwZDQxMGQ5OWE3NzNmNy5zZXRDb250ZW50KGh0bWxfMjc5MmUzNDUyYjZlNDI1NWIxZjgzYTY5Y2NkMTlkOGIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZjg3Mjk0YWY0ZTIwNDVmMDlhY2JmMjIzYjBiODRmNmEuYmluZFBvcHVwKHBvcHVwXzU2ZmUxZWM4N2NiYTQyMjBhZDBkNDEwZDk5YTc3M2Y3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2NkYjA3Y2E1MjIzNzRmMzc4ZGY4NTE3ZjgzNzQ4NzllID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzE4NTE3OTk5OTk5OTk2LC03OS40NjQ3NjMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hOGU0MTQ5ZDNkMzU0N2RhYmVjYWNkYzViZWMzM2RhMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wMjc3OWEyOWVhMGE0Njk3OWIyNGI4YzkzYjBhMzkyOCA9ICQoJzxkaXYgaWQ9Imh0bWxfMDI3NzlhMjllYTBhNDY5NzliMjRiOGM5M2IwYTM5MjgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2E4ZTQxNDlkM2QzNTQ3ZGFiZWNhY2RjNWJlYzMzZGEwLnNldENvbnRlbnQoaHRtbF8wMjc3OWEyOWVhMGE0Njk3OWIyNGI4YzkzYjBhMzkyOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jZGIwN2NhNTIyMzc0ZjM3OGRmODUxN2Y4Mzc0ODc5ZS5iaW5kUG9wdXAocG9wdXBfYThlNDE0OWQzZDM1NDdkYWJlY2FjZGM1YmVjMzNkYTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMGZlMTBlZmYwOWJmNDhkNmI2NWI0ZDUxYjg5OTJhZDIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjIzMDE1LC03OS4zODk0OTM4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzAyNmU1NTRkOGNjMDQ0Y2JhNWEwYzQ3NDM1OGEyNzNjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzdiMWVjYjZhNDZhNzRlOGRhMDBjNTZiYWU3NjA5MDY2ID0gJCgnPGRpdiBpZD0iaHRtbF83YjFlY2I2YTQ2YTc0ZThkYTAwYzU2YmFlNzYwOTA2NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDI2ZTU1NGQ4Y2MwNDRjYmE1YTBjNDc0MzU4YTI3M2Muc2V0Q29udGVudChodG1sXzdiMWVjYjZhNDZhNzRlOGRhMDBjNTZiYWU3NjA5MDY2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzBmZTEwZWZmMDliZjQ4ZDZiNjViNGQ1MWI4OTkyYWQyLmJpbmRQb3B1cChwb3B1cF8wMjZlNTU0ZDhjYzA0NGNiYTVhMGM0NzQzNThhMjczYyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81MTFjYzYzZjk5NzM0MzYzYjc0N2MxNmIxNTA3NzkyMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2Nzg1NTYsLTc5LjUzMjI0MjQwMDAwMDAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzY2MDVjN2Q1ZjliNDRiNDFhNTgyNDYxNTQxMjNmZjBiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2Y3MWQ2OGI5ZDQ1NDQ2MWI5MDZiNDAzYTI0YzQ3ODlhID0gJCgnPGRpdiBpZD0iaHRtbF9mNzFkNjhiOWQ0NTQ0NjFiOTA2YjQwM2EyNGM0Nzg5YSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82NjA1YzdkNWY5YjQ0YjQxYTU4MjQ2MTU0MTIzZmYwYi5zZXRDb250ZW50KGh0bWxfZjcxZDY4YjlkNDU0NDYxYjkwNmI0MDNhMjRjNDc4OWEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNTExY2M2M2Y5OTczNDM2M2I3NDdjMTZiMTUwNzc5MjEuYmluZFBvcHVwKHBvcHVwXzY2MDVjN2Q1ZjliNDRiNDFhNTgyNDYxNTQxMjNmZjBiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2JkMjU4ZDcwM2FmYjRhOTdiMGM1YTgyZjIyYzYzZDQzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuODA2Njg2Mjk5OTk5OTk2LC03OS4xOTQzNTM0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mYWEzYTIxODk5Y2M0NjRiYWNlODJhMmE4YTNkZGI1ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zYzJkY2RkMjU4OWM0OTZkYjdmNGIxZjRjMmUwOTI2NSA9ICQoJzxkaXYgaWQ9Imh0bWxfM2MyZGNkZDI1ODljNDk2ZGI3ZjRiMWY0YzJlMDkyNjUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNjYXJib3JvdWdoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mYWEzYTIxODk5Y2M0NjRiYWNlODJhMmE4YTNkZGI1ZC5zZXRDb250ZW50KGh0bWxfM2MyZGNkZDI1ODljNDk2ZGI3ZjRiMWY0YzJlMDkyNjUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYmQyNThkNzAzYWZiNGE5N2IwYzVhODJmMjJjNjNkNDMuYmluZFBvcHVwKHBvcHVwX2ZhYTNhMjE4OTljYzQ2NGJhY2U4MmEyYThhM2RkYjVkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U1ZTUzMzM4MzhmYzRmZjNhMjM3ZjU0NmFlMzQyZGI3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzQ1OTA1Nzk5OTk5OTk2LC03OS4zNTIxODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODQ0ZTZjMzZhNTU1NDc4ZmJiODNjOGQ1ZWEwNDAwYmEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZTBmNjJiMGQ3YTQ4NDBmMThjMDY3OWU0MjA1ZGFmMTEgPSAkKCc8ZGl2IGlkPSJodG1sX2UwZjYyYjBkN2E0ODQwZjE4YzA2NzllNDIwNWRhZjExIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ob3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF84NDRlNmMzNmE1NTU0NzhmYmI4M2M4ZDVlYTA0MDBiYS5zZXRDb250ZW50KGh0bWxfZTBmNjJiMGQ3YTQ4NDBmMThjMDY3OWU0MjA1ZGFmMTEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTVlNTMzMzgzOGZjNGZmM2EyMzdmNTQ2YWUzNDJkYjcuYmluZFBvcHVwKHBvcHVwXzg0NGU2YzM2YTU1NTQ3OGZiYjgzYzhkNWVhMDQwMGJhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzVlYzZiZTM2ZWJmZjQ3YzU5ZGQ3NmVhNmMwMDExYTYyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA2Mzk3MiwtNzkuMzA5OTM3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FiYWIxYjAyODFlOTQ3NTI5NTNkNDAzZTM0NzE3NDc4ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzMyZDQ4YTZjYjk2NTRkOWQ5OTExODgxMjNhODRhODdhID0gJCgnPGRpdiBpZD0iaHRtbF8zMmQ0OGE2Y2I5NjU0ZDlkOTkxMTg4MTIzYTg0YTg3YSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RWFzdCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hYmFiMWIwMjgxZTk0NzUyOTUzZDQwM2UzNDcxNzQ3OC5zZXRDb250ZW50KGh0bWxfMzJkNDhhNmNiOTY1NGQ5ZDk5MTE4ODEyM2E4NGE4N2EpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNWVjNmJlMzZlYmZmNDdjNTlkZDc2ZWE2YzAwMTFhNjIuYmluZFBvcHVwKHBvcHVwX2FiYWIxYjAyODFlOTQ3NTI5NTNkNDAzZTM0NzE3NDc4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E4MWY2M2NjNDY0NDRlODY5OGVmZTYyMzk0YzhmNDYwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU3MTYxOCwtNzkuMzc4OTM3MDk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMmZhN2M5OTlhOTZkNDFmMGFmZjQxNjY1Y2JhNzQ1MjEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNDZhNTI4ZTg5ZThjNGY2YWE2NzhhYTBiOTMyOTAyMTggPSAkKCc8ZGl2IGlkPSJodG1sXzQ2YTUyOGU4OWU4YzRmNmFhNjc4YWEwYjkzMjkwMjE4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yZmE3Yzk5OWE5NmQ0MWYwYWZmNDE2NjVjYmE3NDUyMS5zZXRDb250ZW50KGh0bWxfNDZhNTI4ZTg5ZThjNGY2YWE2NzhhYTBiOTMyOTAyMTgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYTgxZjYzY2M0NjQ0NGU4Njk4ZWZlNjIzOTRjOGY0NjAuYmluZFBvcHVwKHBvcHVwXzJmYTdjOTk5YTk2ZDQxZjBhZmY0MTY2NWNiYTc0NTIxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzE4NTM4YzlhYTQ4YzQ0NGQ5ZDUzYzAwZmExNDBlNjQ5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA5NTc3LC03OS40NDUwNzI1OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jOWZiODk1ZjhhYTI0NjMzYWUzZjk3YWIwM2JlYTFjZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hZWUzYTZkMTI3YWQ0MjAxYmI4NjJhNTMzZmQ2N2NmZCA9ICQoJzxkaXYgaWQ9Imh0bWxfYWVlM2E2ZDEyN2FkNDIwMWJiODYyYTUzM2ZkNjdjZmQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2M5ZmI4OTVmOGFhMjQ2MzNhZTNmOTdhYjAzYmVhMWNkLnNldENvbnRlbnQoaHRtbF9hZWUzYTZkMTI3YWQ0MjAxYmI4NjJhNTMzZmQ2N2NmZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xODUzOGM5YWE0OGM0NDRkOWQ1M2MwMGZhMTQwZTY0OS5iaW5kUG9wdXAocG9wdXBfYzlmYjg5NWY4YWEyNDYzM2FlM2Y5N2FiMDNiZWExY2QpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYmVmZGFmOTRmY2ExNDVmM2FmYjFmNTg5MjYzZWVkNTkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTA5NDMyLC03OS41NTQ3MjQ0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF84NDE3YTI5ODZjYjA0ZTRiODUxNGI2OWRjYjY4Y2Q3NiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wOTJkMTFhOGE0YTU0YWIxYWQ5ZjU4MDFmYTk5N2ZlMyA9ICQoJzxkaXYgaWQ9Imh0bWxfMDkyZDExYThhNGE1NGFiMWFkOWY1ODAxZmE5OTdmZTMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkV0b2JpY29rZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODQxN2EyOTg2Y2IwNGU0Yjg1MTRiNjlkY2I2OGNkNzYuc2V0Q29udGVudChodG1sXzA5MmQxMWE4YTRhNTRhYjFhZDlmNTgwMWZhOTk3ZmUzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2JlZmRhZjk0ZmNhMTQ1ZjNhZmIxZjU4OTI2M2VlZDU5LmJpbmRQb3B1cChwb3B1cF84NDE3YTI5ODZjYjA0ZTRiODUxNGI2OWRjYjY4Y2Q3Nik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82NDg1NmM3MjQ2Mjk0ZWFkODdlMmRhMDBmZTU0YTViNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc4NDUzNTEsLTc5LjE2MDQ5NzA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2M5MmMxNjhmMGQ5ZDQxMDI4MDA2YzQ1MzJlZTFhZDA0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2YwODY2MjYwNzkwOTQ2MmNiMWIzMDQzZTRiYzMyOGZiID0gJCgnPGRpdiBpZD0iaHRtbF9mMDg2NjI2MDc5MDk0NjJjYjFiMzA0M2U0YmMzMjhmYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2M5MmMxNjhmMGQ5ZDQxMDI4MDA2YzQ1MzJlZTFhZDA0LnNldENvbnRlbnQoaHRtbF9mMDg2NjI2MDc5MDk0NjJjYjFiMzA0M2U0YmMzMjhmYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82NDg1NmM3MjQ2Mjk0ZWFkODdlMmRhMDBmZTU0YTViNi5iaW5kUG9wdXAocG9wdXBfYzkyYzE2OGYwZDlkNDEwMjgwMDZjNDUzMmVlMWFkMDQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOGUxNmRiZDZiNzI2NDAyNzhmODVmZGRmZGYzY2RmN2YgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MjU4OTk3MDAwMDAwMSwtNzkuMzQwOTIzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2QyNmZiZmMwMDgyNDRiOGQ5OWU1YTdkMGVhYTgxNWE2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzVlNzdiMzE0NjM5OTQ4MjlhMjdjZDE2NDc0MmQzNTFkID0gJCgnPGRpdiBpZD0iaHRtbF81ZTc3YjMxNDYzOTk0ODI5YTI3Y2QxNjQ3NDJkMzUxZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDI2ZmJmYzAwODI0NGI4ZDk5ZTVhN2QwZWFhODE1YTYuc2V0Q29udGVudChodG1sXzVlNzdiMzE0NjM5OTQ4MjlhMjdjZDE2NDc0MmQzNTFkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzhlMTZkYmQ2YjcyNjQwMjc4Zjg1ZmRkZmRmM2NkZjdmLmJpbmRQb3B1cChwb3B1cF9kMjZmYmZjMDA4MjQ0YjhkOTllNWE3ZDBlYWE4MTVhNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jM2M4ZjZjM2RhOTM0Y2IwODhlMThhMzU4NjcxYTNkZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY5NTM0MzkwMDAwMDAwNSwtNzkuMzE4Mzg4N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zNjg5ZmMyMjJiNjY0NDMwOGYyZGNlNGFiN2QwZGE4NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hZTE3YjVmMjk3YzI0Yjc5OTMyODg4MDdmNjZlYzZkNSA9ICQoJzxkaXYgaWQ9Imh0bWxfYWUxN2I1ZjI5N2MyNGI3OTkzMjg4ODA3ZjY2ZWM2ZDUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkVhc3QgWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMzY4OWZjMjIyYjY2NDQzMDhmMmRjZTRhYjdkMGRhODcuc2V0Q29udGVudChodG1sX2FlMTdiNWYyOTdjMjRiNzk5MzI4ODgwN2Y2NmVjNmQ1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2MzYzhmNmMzZGE5MzRjYjA4OGUxOGEzNTg2NzFhM2RmLmJpbmRQb3B1cChwb3B1cF8zNjg5ZmMyMjJiNjY0NDMwOGYyZGNlNGFiN2QwZGE4Nyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jN2U0ZTg0ODA2MjI0YTBkYmNjNzUxYzQwZWEzOTRiNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MTQ5MzksLTc5LjM3NTQxNzldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTdlNDA4NmE5MDQ4NGQ0M2IwMDgzMDY3YmIyZmUxNTMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTExNmU0ZDIwMzljNDBlZmFhMTg5ZmZjNGIwMzVmNDMgPSAkKCc8ZGl2IGlkPSJodG1sX2ExMTZlNGQyMDM5YzQwZWZhYTE4OWZmYzRiMDM1ZjQzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81N2U0MDg2YTkwNDg0ZDQzYjAwODMwNjdiYjJmZTE1My5zZXRDb250ZW50KGh0bWxfYTExNmU0ZDIwMzljNDBlZmFhMTg5ZmZjNGIwMzVmNDMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYzdlNGU4NDgwNjIyNGEwZGJjYzc1MWM0MGVhMzk0YjYuYmluZFBvcHVwKHBvcHVwXzU3ZTQwODZhOTA0ODRkNDNiMDA4MzA2N2JiMmZlMTUzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzMzNWM0ZGE3ZDdjNTQ0OTRhOWFjYjc1Yzc4NWE5M2E4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjkzNzgxMywtNzkuNDI4MTkxNDAwMDAwMDJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNDVmMzM5ZjUzNzUzNGViY2I2MDU2OGM1MDRkMDRjYjYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZDBjYjY2N2I2Mzc3NDMyOWE4YjZlNTk3MzkxNmFmYmIgPSAkKCc8ZGl2IGlkPSJodG1sX2QwY2I2NjdiNjM3NzQzMjlhOGI2ZTU5NzM5MTZhZmJiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Zb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80NWYzMzlmNTM3NTM0ZWJjYjYwNTY4YzUwNGQwNGNiNi5zZXRDb250ZW50KGh0bWxfZDBjYjY2N2I2Mzc3NDMyOWE4YjZlNTk3MzkxNmFmYmIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzM1YzRkYTdkN2M1NDQ5NGE5YWNiNzVjNzg1YTkzYTguYmluZFBvcHVwKHBvcHVwXzQ1ZjMzOWY1Mzc1MzRlYmNiNjA1NjhjNTA0ZDA0Y2I2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q5Y2RjYzkyZTYxMjRhZjk4ZGIwZWEyNjAyMThlZjgxID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQzNTE1MiwtNzkuNTc3MjAwNzk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNjgyMjNmNWY0YTgwNDExZTliNjI4NjBmZjYyZTBmNzcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNWQ4OGU2ZDhlNDBmNGRjYmEyZTdiMzhlYWZhMGU4ZmYgPSAkKCc8ZGl2IGlkPSJodG1sXzVkODhlNmQ4ZTQwZjRkY2JhMmU3YjM4ZWFmYTBlOGZmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FdG9iaWNva2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzY4MjIzZjVmNGE4MDQxMWU5YjYyODYwZmY2MmUwZjc3LnNldENvbnRlbnQoaHRtbF81ZDg4ZTZkOGU0MGY0ZGNiYTJlN2IzOGVhZmEwZThmZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kOWNkY2M5MmU2MTI0YWY5OGRiMGVhMjYwMjE4ZWY4MS5iaW5kUG9wdXAocG9wdXBfNjgyMjNmNWY0YTgwNDExZTliNjI4NjBmZjYyZTBmNzcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjQxOGE1NDk4M2E5NDBkNWE5NmU4MTgxOTRmMGJmZmIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NjM1NzI2LC03OS4xODg3MTE1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Y0ODNhNzRiZTEwOTRlNDc4MDI1NDk3ZTI2ZTQ2MGEwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzMyMTJhYjE5YjI3ZTQxZGU5ZDdkMGJkMjFkNmNkNzU5ID0gJCgnPGRpdiBpZD0iaHRtbF8zMjEyYWIxOWIyN2U0MWRlOWQ3ZDBiZDIxZDZjZDc1OSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Y0ODNhNzRiZTEwOTRlNDc4MDI1NDk3ZTI2ZTQ2MGEwLnNldENvbnRlbnQoaHRtbF8zMjEyYWIxOWIyN2U0MWRlOWQ3ZDBiZDIxZDZjZDc1OSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yNDE4YTU0OTgzYTk0MGQ1YTk2ZTgxODE5NGYwYmZmYi5iaW5kUG9wdXAocG9wdXBfZjQ4M2E3NGJlMTA5NGU0NzgwMjU0OTdlMjZlNDYwYTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNmYxYTFmOGQ2NTA4NDIyZmE2YjM2OGNkM2NmYWI3ZDMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NzYzNTczOTk5OTk5OSwtNzkuMjkzMDMxMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82N2RjM2M4NTcyY2U0NTNjODNmYTY0ZDk1Y2NjYWE2ZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kMDA4YzBhOThiNmM0YjU0YjhkMTc3OWEzMjFlM2UzNSA9ICQoJzxkaXYgaWQ9Imh0bWxfZDAwOGMwYTk4YjZjNGI1NGI4ZDE3NzlhMzIxZTNlMzUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkVhc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNjdkYzNjODU3MmNlNDUzYzgzZmE2NGQ5NWNjY2FhNmUuc2V0Q29udGVudChodG1sX2QwMDhjMGE5OGI2YzRiNTRiOGQxNzc5YTMyMWUzZTM1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzZmMWExZjhkNjUwODQyMmZhNmIzNjhjZDNjZmFiN2QzLmJpbmRQb3B1cChwb3B1cF82N2RjM2M4NTcyY2U0NTNjODNmYTY0ZDk1Y2NjYWE2ZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hNzI1NDk1NWQ2YTI0YjA2ODdiMzNiYTc3ZDIwNjBhMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0NDc3MDc5OTk5OTk5NiwtNzkuMzczMzA2NF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xNzhlMTRlNzNhMDQ0MDE5YjM3NDQ5OWYxMjRkMjY3ZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lNjc3ZGJkMmJlNWU0YzM3OTc1NDhhMDMyNmUyYjIzOCA9ICQoJzxkaXYgaWQ9Imh0bWxfZTY3N2RiZDJiZTVlNGMzNzk3NTQ4YTAzMjZlMmIyMzgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzE3OGUxNGU3M2EwNDQwMTliMzc0NDk5ZjEyNGQyNjdlLnNldENvbnRlbnQoaHRtbF9lNjc3ZGJkMmJlNWU0YzM3OTc1NDhhMDMyNmUyYjIzOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hNzI1NDk1NWQ2YTI0YjA2ODdiMzNiYTc3ZDIwNjBhMC5iaW5kUG9wdXAocG9wdXBfMTc4ZTE0ZTczYTA0NDAxOWIzNzQ0OTlmMTI0ZDI2N2UpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjQ0NTI0NWQwNjAzNGJjOWIzOGM2M2JmZjc1NTA5NGYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42ODkwMjU2LC03OS40NTM1MTJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMGM0NmJjZGVmZmY5NDQ0ZGFlNDZiMWZjOWVmMTE5M2IgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjkwZWNmMzdlZDkyNGZjMmFmOGM5MTc5M2NjNzIyNWIgPSAkKCc8ZGl2IGlkPSJodG1sX2Y5MGVjZjM3ZWQ5MjRmYzJhZjhjOTE3OTNjYzcyMjViIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Zb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wYzQ2YmNkZWZmZjk0NDRkYWU0NmIxZmM5ZWYxMTkzYi5zZXRDb250ZW50KGh0bWxfZjkwZWNmMzdlZDkyNGZjMmFmOGM5MTc5M2NjNzIyNWIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNjQ0NTI0NWQwNjAzNGJjOWIzOGM2M2JmZjc1NTA5NGYuYmluZFBvcHVwKHBvcHVwXzBjNDZiY2RlZmZmOTQ0NGRhZTQ2YjFmYzllZjExOTNiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzNhNTc5ZDMyNzBhZTQzZGU4ZjdkNjI2N2E3MWVjYTJlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzcwOTkyMSwtNzkuMjE2OTE3NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMTIxOGQ4MjczY2E1NDczOGI5M2Q4MzQ4YTc4NmY1ZTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOWI2NmViYjI3MmNkNDQ0ZmJhNDk5YmRmMDFkODU0OGMgPSAkKCc8ZGl2IGlkPSJodG1sXzliNjZlYmIyNzJjZDQ0NGZiYTQ5OWJkZjAxZDg1NDhjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTIxOGQ4MjczY2E1NDczOGI5M2Q4MzQ4YTc4NmY1ZTIuc2V0Q29udGVudChodG1sXzliNjZlYmIyNzJjZDQ0NGZiYTQ5OWJkZjAxZDg1NDhjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzNhNTc5ZDMyNzBhZTQzZGU4ZjdkNjI2N2E3MWVjYTJlLmJpbmRQb3B1cChwb3B1cF8xMjE4ZDgyNzNjYTU0NzM4YjkzZDgzNDhhNzg2ZjVlMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80ODEyZjJiNWNmNWI0NGQ0ODNiMGQ4NjM3OWVkYTgwMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcwOTA2MDQsLTc5LjM2MzQ1MTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMzhiM2QxODAwODNmNDBjNWFhNzBhYTQ0ZDI3YzY1MjggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTExNTA5NzdkZjQ0NDk0NzlkOWYxM2I0YWJhYThlZWYgPSAkKCc8ZGl2IGlkPSJodG1sXzkxMTUwOTc3ZGY0NDQ5NDc5ZDlmMTNiNGFiYWE4ZWVmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FYXN0IFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzM4YjNkMTgwMDgzZjQwYzVhYTcwYWE0NGQyN2M2NTI4LnNldENvbnRlbnQoaHRtbF85MTE1MDk3N2RmNDQ0OTQ3OWQ5ZjEzYjRhYmFhOGVlZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80ODEyZjJiNWNmNWI0NGQ0ODNiMGQ4NjM3OWVkYTgwMC5iaW5kUG9wdXAocG9wdXBfMzhiM2QxODAwODNmNDBjNWFhNzBhYTQ0ZDI3YzY1MjgpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNTVkNDZjNjY4MTM5NDg3ZTk3OGRkMmFlYzVkM2IzOWMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTc5NTI0LC03OS4zODczODI2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzgxMGMyYWQ0ODcyMDQxM2ViMGMwZTE4ODVkNjFmZjA2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2IxMGNjODUzMWNlYzQ3NGZhZjM3MTQ5Y2JiOGVkMDhiID0gJCgnPGRpdiBpZD0iaHRtbF9iMTBjYzg1MzFjZWM0NzRmYWYzNzE0OWNiYjhlZDA4YiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODEwYzJhZDQ4NzIwNDEzZWIwYzBlMTg4NWQ2MWZmMDYuc2V0Q29udGVudChodG1sX2IxMGNjODUzMWNlYzQ3NGZhZjM3MTQ5Y2JiOGVkMDhiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzU1ZDQ2YzY2ODEzOTQ4N2U5NzhkZDJhZWM1ZDNiMzljLmJpbmRQb3B1cChwb3B1cF84MTBjMmFkNDg3MjA0MTNlYjBjMGUxODg1ZDYxZmYwNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85ZDA4ZDUyMWVkOWM0MzBkYWFiZDQxZWI5ZWMyYmZhOSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2OTU0MiwtNzkuNDIyNTYzN10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82Nzc0MDYzMmE5MWU0MjYwYTMzNDM4MGMwZTcxZWRmYyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85MDBlYmQ3MWYyODg0OTdlOTRmNGYwOGQzMzY5MzdlYSA9ICQoJzxkaXYgaWQ9Imh0bWxfOTAwZWJkNzFmMjg4NDk3ZTk0ZjRmMDhkMzM2OTM3ZWEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzY3NzQwNjMyYTkxZTQyNjBhMzM0MzgwYzBlNzFlZGZjLnNldENvbnRlbnQoaHRtbF85MDBlYmQ3MWYyODg0OTdlOTRmNGYwOGQzMzY5MzdlYSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85ZDA4ZDUyMWVkOWM0MzBkYWFiZDQxZWI5ZWMyYmZhOS5iaW5kUG9wdXAocG9wdXBfNjc3NDA2MzJhOTFlNDI2MGEzMzQzODBjMGU3MWVkZmMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzM4MDgxMDM0NWU2NGJhNTk2YThlOTk3ZjUyM2VjZGUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NzMxMzYsLTc5LjIzOTQ3NjA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2UzNjE3Y2ZkZmUyODRjOWQ4NjQ0ZWJmNGE2Nzg1OWEwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2YzMzhhMzQ4MzA2ZjQxMGNhZDQzYzlmYWZiMzJlZTRmID0gJCgnPGRpdiBpZD0iaHRtbF9mMzM4YTM0ODMwNmY0MTBjYWQ0M2M5ZmFmYjMyZWU0ZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2UzNjE3Y2ZkZmUyODRjOWQ4NjQ0ZWJmNGE2Nzg1OWEwLnNldENvbnRlbnQoaHRtbF9mMzM4YTM0ODMwNmY0MTBjYWQ0M2M5ZmFmYjMyZWU0Zik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zMzgwODEwMzQ1ZTY0YmE1OTZhOGU5OTdmNTIzZWNkZS5iaW5kUG9wdXAocG9wdXBfZTM2MTdjZmRmZTI4NGM5ZDg2NDRlYmY0YTY3ODU5YTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNTdmMmIyZmU5ZTdkNDQ1MTllYjFlYTVjYzBkYWU3YzUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My44MDM3NjIyLC03OS4zNjM0NTE3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzhlYThiNTc2ODYzMTRkZDNiMTRjOGRjMjA0ZDNmMTk5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2QwNDA4ZmI0NmQ2NjQ5MGQ4Njg3NTBjOTY1MzdkNzkyID0gJCgnPGRpdiBpZD0iaHRtbF9kMDQwOGZiNDZkNjY0OTBkODY4NzUwYzk2NTM3ZDc5MiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOGVhOGI1NzY4NjMxNGRkM2IxNGM4ZGMyMDRkM2YxOTkuc2V0Q29udGVudChodG1sX2QwNDA4ZmI0NmQ2NjQ5MGQ4Njg3NTBjOTY1MzdkNzkyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzU3ZjJiMmZlOWU3ZDQ0NTE5ZWIxZWE1Y2MwZGFlN2M1LmJpbmRQb3B1cChwb3B1cF84ZWE4YjU3Njg2MzE0ZGQzYjE0YzhkYzIwNGQzZjE5OSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84ZGU3MTQ4ODk3ZjA0YzQ5YTU5YmI1OWViMWQ5YTlhYSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc1NDMyODMsLTc5LjQ0MjI1OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOTlmMjUzNzY3ZmQ2NDhkOTk4ZTg0MTQwY2U2NDk2Y2YgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZGViNmEzMjBjZGYwNDQxODg3MDg3YTA4OWJjMGI4ZmYgPSAkKCc8ZGl2IGlkPSJodG1sX2RlYjZhMzIwY2RmMDQ0MTg4NzA4N2EwODliYzBiOGZmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ob3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85OWYyNTM3NjdmZDY0OGQ5OThlODQxNDBjZTY0OTZjZi5zZXRDb250ZW50KGh0bWxfZGViNmEzMjBjZGYwNDQxODg3MDg3YTA4OWJjMGI4ZmYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOGRlNzE0ODg5N2YwNGM0OWE1OWJiNTllYjFkOWE5YWEuYmluZFBvcHVwKHBvcHVwXzk5ZjI1Mzc2N2ZkNjQ4ZDk5OGU4NDE0MGNlNjQ5NmNmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q2ZGQxNzE5ZTkwNTQwOWY5Zjc5OGY2MjA3NjYxZjFlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA1MzY4OSwtNzkuMzQ5MzcxOTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjhkYmQ3N2QwODYyNGI3NWI1MDlmODdlYWVjMDFkNGMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZDBjMTJjM2Y3MDRiNGIxNDhjYzg0ZjUwOTA1MjQzYjYgPSAkKCc8ZGl2IGlkPSJodG1sX2QwYzEyYzNmNzA0YjRiMTQ4Y2M4NGY1MDkwNTI0M2I2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FYXN0IFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2I4ZGJkNzdkMDg2MjRiNzViNTA5Zjg3ZWFlYzAxZDRjLnNldENvbnRlbnQoaHRtbF9kMGMxMmMzZjcwNGI0YjE0OGNjODRmNTA5MDUyNDNiNik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kNmRkMTcxOWU5MDU0MDlmOWY3OThmNjIwNzY2MWYxZS5iaW5kUG9wdXAocG9wdXBfYjhkYmQ3N2QwODYyNGI3NWI1MDlmODdlYWVjMDFkNGMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYjA2NjYwNzlkYWQwNDJlYjk4YWM0MWE1NzZhOTViYmIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTA1NzEyMDAwMDAwMSwtNzkuMzg0NTY3NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iYTZjOTk2YWYxZTQ0YjA1ODBiNzYyZmNjN2QxNzFhMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jMGQwNTdmOWYyYTQ0YmNjODdmNzY0YjY4ODAzMjAwNCA9ICQoJzxkaXYgaWQ9Imh0bWxfYzBkMDU3ZjlmMmE0NGJjYzg3Zjc2NGI2ODgwMzIwMDQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2JhNmM5OTZhZjFlNDRiMDU4MGI3NjJmY2M3ZDE3MWEyLnNldENvbnRlbnQoaHRtbF9jMGQwNTdmOWYyYTQ0YmNjODdmNzY0YjY4ODAzMjAwNCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iMDY2NjA3OWRhZDA0MmViOThhYzQxYTU3NmE5NWJiYi5iaW5kUG9wdXAocG9wdXBfYmE2Yzk5NmFmMWU0NGIwNTgwYjc2MmZjYzdkMTcxYTIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTQ1NDE0NTlmMTMyNDQwNzlhYjgwMjA4MTVjZDdhZDAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjkwMDUxMDAwMDAwMSwtNzkuNDQyMjU5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kYTQ5NGUzNjY4NDk0NDIxOTRkOWIxYmI5NzdjZTk3NCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83MGQ3YTY5NTNjMzg0ZmZiYmI1YjYxYTMyMGU2OWUxZiA9ICQoJzxkaXYgaWQ9Imh0bWxfNzBkN2E2OTUzYzM4NGZmYmJiNWI2MWEzMjBlNjllMWYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZGE0OTRlMzY2ODQ5NDQyMTk0ZDliMWJiOTc3Y2U5NzQuc2V0Q29udGVudChodG1sXzcwZDdhNjk1M2MzODRmZmJiYjViNjFhMzIwZTY5ZTFmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzk0NTQxNDU5ZjEzMjQ0MDc5YWI4MDIwODE1Y2Q3YWQwLmJpbmRQb3B1cChwb3B1cF9kYTQ5NGUzNjY4NDk0NDIxOTRkOWIxYmI5NzdjZTk3NCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zYjU4YTI4YWQ2M2Y0ZGExYWI1YjY4NWFmYjMzNjk5ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc0NDczNDIsLTc5LjIzOTQ3NjA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzlkMzgzZGU3ZDczZTQ0N2Q5Y2Y2NDA0ZmM1YjQ1OWI0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzIyOTk5NWYwYmMyMzQ4ZGNiNzg4NWM3N2QzMmUxNDhjID0gJCgnPGRpdiBpZD0iaHRtbF8yMjk5OTVmMGJjMjM0OGRjYjc4ODVjNzdkMzJlMTQ4YyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzlkMzgzZGU3ZDczZTQ0N2Q5Y2Y2NDA0ZmM1YjQ1OWI0LnNldENvbnRlbnQoaHRtbF8yMjk5OTVmMGJjMjM0OGRjYjc4ODVjNzdkMzJlMTQ4Yyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zYjU4YTI4YWQ2M2Y0ZGExYWI1YjY4NWFmYjMzNjk5ZS5iaW5kUG9wdXAocG9wdXBfOWQzODNkZTdkNzNlNDQ3ZDljZjY0MDRmYzViNDU5YjQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODlmZmI2NmY3OThiNDg0NGI2MTFjNmM1NjgzMmNkNjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43Nzg1MTc1LC03OS4zNDY1NTU3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzBiNTE1NjA0YjAxMTQ0Zjc5MzU5MmIyZTk1ODNmNzJhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzFlMDEzMjBjZjRkMTQ1OWY4MGM4MjcxNGE3NGIxZTAzID0gJCgnPGRpdiBpZD0iaHRtbF8xZTAxMzIwY2Y0ZDE0NTlmODBjODI3MTRhNzRiMWUwMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMGI1MTU2MDRiMDExNDRmNzkzNTkyYjJlOTU4M2Y3MmEuc2V0Q29udGVudChodG1sXzFlMDEzMjBjZjRkMTQ1OWY4MGM4MjcxNGE3NGIxZTAzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzg5ZmZiNjZmNzk4YjQ4NDRiNjExYzZjNTY4MzJjZDYyLmJpbmRQb3B1cChwb3B1cF8wYjUxNTYwNGIwMTE0NGY3OTM1OTJiMmU5NTgzZjcyYSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yZmNkNWNkNjA5MTg0ZDcxOTA5YTdlMTczNGY1ZDU0OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc2Nzk4MDMsLTc5LjQ4NzI2MTkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzcxYmUzOWU4ZmUyMzRiNjY4OGQxNzFkZDA1NWZhZDRjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzhhODI3YWZkYWMxMTQ5Yzg4MDU5YWFhMjYxZDczYWE3ID0gJCgnPGRpdiBpZD0iaHRtbF84YTgyN2FmZGFjMTE0OWM4ODA1OWFhYTI2MWQ3M2FhNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNzFiZTM5ZThmZTIzNGI2Njg4ZDE3MWRkMDU1ZmFkNGMuc2V0Q29udGVudChodG1sXzhhODI3YWZkYWMxMTQ5Yzg4MDU5YWFhMjYxZDczYWE3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzJmY2Q1Y2Q2MDkxODRkNzE5MDlhN2UxNzM0ZjVkNTQ5LmJpbmRQb3B1cChwb3B1cF83MWJlMzllOGZlMjM0YjY2ODhkMTcxZGQwNTVmYWQ0Yyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82NDJkMmM3NTRiZDI0NzE3ODIyZjY0M2Y0ODlkOTZmMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4NTM0NywtNzkuMzM4MTA2NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zMjQxMWZlNDg5ZWY0NWQyOTYxNzE3NTMzMGM3NmE4MyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83YzcyOTRlMTc4NWY0YzdkYjE5NTU4YWY3NTNmNWVkNSA9ICQoJzxkaXYgaWQ9Imh0bWxfN2M3Mjk0ZTE3ODVmNGM3ZGIxOTU1OGFmNzUzZjVlZDUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkVhc3QgWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMzI0MTFmZTQ4OWVmNDVkMjk2MTcxNzUzMzBjNzZhODMuc2V0Q29udGVudChodG1sXzdjNzI5NGUxNzg1ZjRjN2RiMTk1NThhZjc1M2Y1ZWQ1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzY0MmQyYzc1NGJkMjQ3MTc4MjJmNjQzZjQ4OWQ5NmYxLmJpbmRQb3B1cChwb3B1cF8zMjQxMWZlNDg5ZWY0NWQyOTYxNzE3NTMzMGM3NmE4Myk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hMWMxYTFjMmY4NjQ0ODE5OWM5MDg2ZGI2MGVkMzdiZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0MDgxNTcsLTc5LjM4MTc1MjI5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2JiMzVhZDkzYzlkODRiMTQ5OTBlMzZiOWE2ODI0MWEyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzg3M2MwYWVmMzhkMDRjYmNhMDE5YjdkYWZlMWFiMTEzID0gJCgnPGRpdiBpZD0iaHRtbF84NzNjMGFlZjM4ZDA0Y2JjYTAxOWI3ZGFmZTFhYjExMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYmIzNWFkOTNjOWQ4NGIxNDk5MGUzNmI5YTY4MjQxYTIuc2V0Q29udGVudChodG1sXzg3M2MwYWVmMzhkMDRjYmNhMDE5YjdkYWZlMWFiMTEzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ExYzFhMWMyZjg2NDQ4MTk5YzkwODZkYjYwZWQzN2JlLmJpbmRQb3B1cChwb3B1cF9iYjM1YWQ5M2M5ZDg0YjE0OTkwZTM2YjlhNjgyNDFhMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hYThhZjRkNmUwYzU0ODk5YmZkYTFmZDIxODhlMTNhZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0NzkyNjcwMDAwMDAwNiwtNzkuNDE5NzQ5N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xNDc2MDQ0OTcyM2Q0ZjBkYTY2MWI2YWI5Y2JhMmE3ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jYTY2MjI4M2NmZjI0MTIxYTQ4NWZlOGE2NjQ0NWIxNyA9ICQoJzxkaXYgaWQ9Imh0bWxfY2E2NjIyODNjZmYyNDEyMWE0ODVmZThhNjY0NDViMTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTQ3NjA0NDk3MjNkNGYwZGE2NjFiNmFiOWNiYTJhN2Quc2V0Q29udGVudChodG1sX2NhNjYyMjgzY2ZmMjQxMjFhNDg1ZmU4YTY2NDQ1YjE3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FhOGFmNGQ2ZTBjNTQ4OTliZmRhMWZkMjE4OGUxM2FmLmJpbmRQb3B1cChwb3B1cF8xNDc2MDQ0OTcyM2Q0ZjBkYTY2MWI2YWI5Y2JhMmE3ZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zNjAyZmRkNDRkMjI0NjFiYTQ2NzY3ODJmM2E1ZGYwNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcyNzkyOTIsLTc5LjI2MjAyOTQwMDAwMDAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Q0MWUzN2M2OTJiMTRjMmRiYzczMTAyZjc4MzlhMWMyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzA4NmVmNzFmMzg2MDQ0ZWZhMDdjNjUzNmYzZmEwZDMxID0gJCgnPGRpdiBpZD0iaHRtbF8wODZlZjcxZjM4NjA0NGVmYTA3YzY1MzZmM2ZhMGQzMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Q0MWUzN2M2OTJiMTRjMmRiYzczMTAyZjc4MzlhMWMyLnNldENvbnRlbnQoaHRtbF8wODZlZjcxZjM4NjA0NGVmYTA3YzY1MzZmM2ZhMGQzMSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zNjAyZmRkNDRkMjI0NjFiYTQ2NzY3ODJmM2E1ZGYwNC5iaW5kUG9wdXAocG9wdXBfZDQxZTM3YzY5MmIxNGMyZGJjNzMxMDJmNzgzOWExYzIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjY1MjhlZjEwZmVhNGY1ZmEzODI0MTIxMWU0OGMwMDggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43ODY5NDczLC03OS4zODU5NzVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOGIwODM3NmIyODRmNGIzNjgxNTUwZDA0NzI3MGMwZmUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNDcwNDMxZDM2NmJiNDQyZjllNzRhNzdlYTZjMTk3YmYgPSAkKCc8ZGl2IGlkPSJodG1sXzQ3MDQzMWQzNjZiYjQ0MmY5ZTc0YTc3ZWE2YzE5N2JmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ob3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF84YjA4Mzc2YjI4NGY0YjM2ODE1NTBkMDQ3MjcwYzBmZS5zZXRDb250ZW50KGh0bWxfNDcwNDMxZDM2NmJiNDQyZjllNzRhNzdlYTZjMTk3YmYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNjY1MjhlZjEwZmVhNGY1ZmEzODI0MTIxMWU0OGMwMDguYmluZFBvcHVwKHBvcHVwXzhiMDgzNzZiMjg0ZjRiMzY4MTU1MGQwNDcyNzBjMGZlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U5NzIzZDIwNThhMDQxZjRiYTMyOWE3ZWJmYjliY2QwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzM3NDczMjAwMDAwMDA0LC03OS40NjQ3NjMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jY2I0MDhlZGM4NzA0M2RlYWNiOWEyZDllYjg1NWM2ZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83ZGVkMDY2MGFmNGE0MTdjYjAwMDVjODZlNjU2MzlkMSA9ICQoJzxkaXYgaWQ9Imh0bWxfN2RlZDA2NjBhZjRhNDE3Y2IwMDA1Yzg2ZTY1NjM5ZDEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NjYjQwOGVkYzg3MDQzZGVhY2I5YTJkOWViODU1YzZlLnNldENvbnRlbnQoaHRtbF83ZGVkMDY2MGFmNGE0MTdjYjAwMDVjODZlNjU2MzlkMSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lOTcyM2QyMDU4YTA0MWY0YmEzMjlhN2ViZmI5YmNkMC5iaW5kUG9wdXAocG9wdXBfY2NiNDA4ZWRjODcwNDNkZWFjYjlhMmQ5ZWI4NTVjNmUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWViZWQxZDg2MjgyNDIyZTg4YWI5MzY4YjRhYTdhNTggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Nzk1NTcxLC03OS4zNTIxODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzM4OGYzMTFiODNhNDgzOGIxZjU1N2NiMDdkOWZjMjIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMzNjOTliMDQ4ZjZhNGU2NzljNGNjZDVjZGYxOTI3YjkgPSAkKCc8ZGl2IGlkPSJodG1sXzMzYzk5YjA0OGY2YTRlNjc5YzRjY2Q1Y2RmMTkyN2I5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzczODhmMzExYjgzYTQ4MzhiMWY1NTdjYjA3ZDlmYzIyLnNldENvbnRlbnQoaHRtbF8zM2M5OWIwNDhmNmE0ZTY3OWM0Y2NkNWNkZjE5MjdiOSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hZWJlZDFkODYyODI0MjJlODhhYjkzNjhiNGFhN2E1OC5iaW5kUG9wdXAocG9wdXBfNzM4OGYzMTFiODNhNDgzOGIxZjU1N2NiMDdkOWZjMjIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWVjZmIxMDRiNTYyNDM0NWI4MGE2NjMzNTM5NTgyNDAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDcxNzY4LC03OS4zODE1NzY0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kOGRkZjIzNzdjMGI0YWFhYjM0NTE4ZTVjYTI2NWE3YyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82OGEzYjNkN2RkZDQ0NzY2ODUxYzdlZTIxNzg2MzY5YiA9ICQoJzxkaXYgaWQ9Imh0bWxfNjhhM2IzZDdkZGQ0NDc2Njg1MWM3ZWUyMTc4NjM2OWIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Q4ZGRmMjM3N2MwYjRhYWFiMzQ1MThlNWNhMjY1YTdjLnNldENvbnRlbnQoaHRtbF82OGEzYjNkN2RkZDQ0NzY2ODUxYzdlZTIxNzg2MzY5Yik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85ZWNmYjEwNGI1NjI0MzQ1YjgwYTY2MzM1Mzk1ODI0MC5iaW5kUG9wdXAocG9wdXBfZDhkZGYyMzc3YzBiNGFhYWIzNDUxOGU1Y2EyNjVhN2MpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYmYyYmMxODhkM2IxNGJlNGI4NDVkZGMxZmUzZDE1ODAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42MzY4NDcyLC03OS40MjgxOTE0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85Mzg3YWZhNjNhNzU0ZmZmYTdhMThhMDEzOTA1OGI5NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xYzJiYjQ5YmY0YmY0YTBlOTFkOGFmYjE3MWZlZjJjZiA9ICQoJzxkaXYgaWQ9Imh0bWxfMWMyYmI0OWJmNGJmNGEwZTkxZDhhZmIxNzFmZWYyY2YiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOTM4N2FmYTYzYTc1NGZmZmE3YTE4YTAxMzkwNThiOTcuc2V0Q29udGVudChodG1sXzFjMmJiNDliZjRiZjRhMGU5MWQ4YWZiMTcxZmVmMmNmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2JmMmJjMTg4ZDNiMTRiZTRiODQ1ZGRjMWZlM2QxNTgwLmJpbmRQb3B1cChwb3B1cF85Mzg3YWZhNjNhNzU0ZmZmYTdhMThhMDEzOTA1OGI5Nyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85N2U4MjhhNTIwNGE0ZWE4OWJjNDkwN2Y4MTIzYmVjNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxMTExMTcwMDAwMDAwNCwtNzkuMjg0NTc3Ml0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81N2RmZjE0ZjY1ZDU0NTcxODk1YTc2MzRjZWIzYTI1MSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zZTNiMzdkODgxODE0ODliYTdkYWY4ODI1OTVkMjQ0NiA9ICQoJzxkaXYgaWQ9Imh0bWxfM2UzYjM3ZDg4MTgxNDg5YmE3ZGFmODgyNTk1ZDI0NDYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNjYXJib3JvdWdoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81N2RmZjE0ZjY1ZDU0NTcxODk1YTc2MzRjZWIzYTI1MS5zZXRDb250ZW50KGh0bWxfM2UzYjM3ZDg4MTgxNDg5YmE3ZGFmODgyNTk1ZDI0NDYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOTdlODI4YTUyMDRhNGVhODliYzQ5MDdmODEyM2JlYzcuYmluZFBvcHVwKHBvcHVwXzU3ZGZmMTRmNjVkNTQ1NzE4OTVhNzYzNGNlYjNhMjUxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzVlZGI3OWU3YjczNDQ3YmJhMTM0NjhiZGIwMmE5OWZjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzU3NDkwMiwtNzkuMzc0NzE0MDk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMmYyMzBjZGNmM2MxNDkzMTk0MzdmYTY2YmZiNTQzMDggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfY2FiOTU2NmZmNDkxNDY2YTk4ZWMzMWE4YTlhMjc5ZDEgPSAkKCc8ZGl2IGlkPSJodG1sX2NhYjk1NjZmZjQ5MTQ2NmE5OGVjMzFhOGE5YTI3OWQxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ob3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yZjIzMGNkY2YzYzE0OTMxOTQzN2ZhNjZiZmI1NDMwOC5zZXRDb250ZW50KGh0bWxfY2FiOTU2NmZmNDkxNDY2YTk4ZWMzMWE4YTlhMjc5ZDEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNWVkYjc5ZTdiNzM0NDdiYmExMzQ2OGJkYjAyYTk5ZmMuYmluZFBvcHVwKHBvcHVwXzJmMjMwY2RjZjNjMTQ5MzE5NDM3ZmE2NmJmYjU0MzA4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U5M2M0ODNkZjNhODQ0OWViNTk4MTQyYTVjNzVlNmQ5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzM5MDE0NiwtNzkuNTA2OTQzNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lZTI1MGFhYmUzYTc0MjA4OGZkMGM0YWFkOTAxZjU3NCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xZTA3MzZmODM4N2E0ZTM5YTA4ZjQxZDE5MTIxM2VjMiA9ICQoJzxkaXYgaWQ9Imh0bWxfMWUwNzM2ZjgzODdhNGUzOWEwOGY0MWQxOTEyMTNlYzIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2VlMjUwYWFiZTNhNzQyMDg4ZmQwYzRhYWQ5MDFmNTc0LnNldENvbnRlbnQoaHRtbF8xZTA3MzZmODM4N2E0ZTM5YTA4ZjQxZDE5MTIxM2VjMik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lOTNjNDgzZGYzYTg0NDllYjU5ODE0MmE1Yzc1ZTZkOS5iaW5kUG9wdXAocG9wdXBfZWUyNTBhYWJlM2E3NDIwODhmZDBjNGFhZDkwMWY1NzQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzZlM2YwYjFlNGFkNDA0OGFmZGY4ZmYxNjRlYmY0YjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njg5OTg1LC03OS4zMTU1NzE1OTk5OTk5OF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yOWVjMjBjNWRlMzY0Mjg0YTBiMGFmZjg3ZDFjYWJmYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83OWNhZjZhYTg1ZTE0OTFjODNlN2MzZjI2MDhmMDQ1NyA9ICQoJzxkaXYgaWQ9Imh0bWxfNzljYWY2YWE4NWUxNDkxYzgzZTdjM2YyNjA4ZjA0NTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkVhc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMjllYzIwYzVkZTM2NDI4NGEwYjBhZmY4N2QxY2FiZmIuc2V0Q29udGVudChodG1sXzc5Y2FmNmFhODVlMTQ5MWM4M2U3YzNmMjYwOGYwNDU3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzM2ZTNmMGIxZTRhZDQwNDhhZmRmOGZmMTY0ZWJmNGIyLmJpbmRQb3B1cChwb3B1cF8yOWVjMjBjNWRlMzY0Mjg0YTBiMGFmZjg3ZDFjYWJmYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wYmRiOTZlNzNhMTU0MTQ0YjUwMjA3ZGE5Yjk3M2RjMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0ODE5ODUsLTc5LjM3OTgxNjkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2MzOWU3ZmNlZDdiNTQ1YTFiN2UzYzhjMWY0YzJlYzUyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2I2MmQ4NmI1YzA2MjQzZjc4YWI1ZjQwYzAzNTIzOTI0ID0gJCgnPGRpdiBpZD0iaHRtbF9iNjJkODZiNWMwNjI0M2Y3OGFiNWY0MGMwMzUyMzkyNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYzM5ZTdmY2VkN2I1NDVhMWI3ZTNjOGMxZjRjMmVjNTIuc2V0Q29udGVudChodG1sX2I2MmQ4NmI1YzA2MjQzZjc4YWI1ZjQwYzAzNTIzOTI0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzBiZGI5NmU3M2ExNTQxNDRiNTAyMDdkYTliOTczZGMwLmJpbmRQb3B1cChwb3B1cF9jMzllN2ZjZWQ3YjU0NWExYjdlM2M4YzFmNGMyZWM1Mik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kZDRmODMyODk3Nzk0NzhkYTk2NmNkZjJmZmFkNGJmMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxMzc1NjIwMDAwMDAwNiwtNzkuNDkwMDczOF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jZmIzNWY2NDFmNDk0ZmQ0ODVlZTgzOGRkNTBiYmRmNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iNDRmZjc2ODY2ZTQ0NWRhYjg2MWIwZTBhYTY4M2MzNSA9ICQoJzxkaXYgaWQ9Imh0bWxfYjQ0ZmY3Njg2NmU0NDVkYWI4NjFiMGUwYWE2ODNjMzUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NmYjM1ZjY0MWY0OTRmZDQ4NWVlODM4ZGQ1MGJiZGY2LnNldENvbnRlbnQoaHRtbF9iNDRmZjc2ODY2ZTQ0NWRhYjg2MWIwZTBhYTY4M2MzNSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kZDRmODMyODk3Nzk0NzhkYTk2NmNkZjJmZmFkNGJmMy5iaW5kUG9wdXAocG9wdXBfY2ZiMzVmNjQxZjQ5NGZkNDg1ZWU4MzhkZDUwYmJkZjYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODU3NzQ5NTIzZDcyNDQ5NWIzYTRlNGRiMmZkYzBiZGUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NTYzMDMzLC03OS41NjU5NjMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xMjRlZDIwYTk2YzU0YzExYjE2ODUxNTVjYTYzZmFjNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hMDRlZjc3YWM4MDg0MTM0OTg1YjMzYWVkYWFiNzEzZCA9ICQoJzxkaXYgaWQ9Imh0bWxfYTA0ZWY3N2FjODA4NDEzNDk4NWIzM2FlZGFhYjcxM2QiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzEyNGVkMjBhOTZjNTRjMTFiMTY4NTE1NWNhNjNmYWM2LnNldENvbnRlbnQoaHRtbF9hMDRlZjc3YWM4MDg0MTM0OTg1YjMzYWVkYWFiNzEzZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84NTc3NDk1MjNkNzI0NDk1YjNhNGU0ZGIyZmRjMGJkZS5iaW5kUG9wdXAocG9wdXBfMTI0ZWQyMGE5NmM1NGMxMWIxNjg1MTU1Y2E2M2ZhYzYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZDU1YzMwODg0NDlmNDNhNzhiMzYxMDgyOTg2MWU3Y2EgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTYzMTYsLTc5LjIzOTQ3NjA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzUwMGMxNGMwN2EzZDRiZTA5ODkwMGVmNDVhYTI1MmIxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzc2OTUxNmQ5ODBhNjQwMGJhM2UxNGRhMWNkODVlYzBjID0gJCgnPGRpdiBpZD0iaHRtbF83Njk1MTZkOTgwYTY0MDBiYTNlMTRkYTFjZDg1ZWMwYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzUwMGMxNGMwN2EzZDRiZTA5ODkwMGVmNDVhYTI1MmIxLnNldENvbnRlbnQoaHRtbF83Njk1MTZkOTgwYTY0MDBiYTNlMTRkYTFjZDg1ZWMwYyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kNTVjMzA4ODQ0OWY0M2E3OGIzNjEwODI5ODYxZTdjYS5iaW5kUG9wdXAocG9wdXBfNTAwYzE0YzA3YTNkNGJlMDk4OTAwZWY0NWFhMjUyYjEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYjBjYWM2NWFiOWMxNDk1N2JkY2UyYmRmN2NmMDdkOGEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43ODkwNTMsLTc5LjQwODQ5Mjc5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2ZiN2VmZDdlYWU2NTQxMzhhMjBmNDI4YjgzY2RkYzU1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzhhNmRjM2YwZTc3NDRkODBhMWI5NTI1OTA4NDIyMzZjID0gJCgnPGRpdiBpZD0iaHRtbF84YTZkYzNmMGU3NzQ0ZDgwYTFiOTUyNTkwODQyMjM2YyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmI3ZWZkN2VhZTY1NDEzOGEyMGY0MjhiODNjZGRjNTUuc2V0Q29udGVudChodG1sXzhhNmRjM2YwZTc3NDRkODBhMWI5NTI1OTA4NDIyMzZjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2IwY2FjNjVhYjljMTQ5NTdiZGNlMmJkZjdjZjA3ZDhhLmJpbmRQb3B1cChwb3B1cF9mYjdlZmQ3ZWFlNjU0MTM4YTIwZjQyOGI4M2NkZGM1NSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83YjY0NjNlMDEyNmU0MzkxOTQwZGFjOGZiN2VjODI1NyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcyODQ5NjQsLTc5LjQ5NTY5NzQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FlNjI1ZGE4YWU4YjQ0ODM4MTRmYTQxNWFlNjZhZjVjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZhZTE2MWRjOWNiMzRiNGJiMjlhODRmMTkzMDdlNTc3ID0gJCgnPGRpdiBpZD0iaHRtbF82YWUxNjFkYzljYjM0YjRiYjI5YTg0ZjE5MzA3ZTU3NyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYWU2MjVkYThhZThiNDQ4MzgxNGZhNDE1YWU2NmFmNWMuc2V0Q29udGVudChodG1sXzZhZTE2MWRjOWNiMzRiNGJiMjlhODRmMTkzMDdlNTc3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzdiNjQ2M2UwMTI2ZTQzOTE5NDBkYWM4ZmI3ZWM4MjU3LmJpbmRQb3B1cChwb3B1cF9hZTYyNWRhOGFlOGI0NDgzODE0ZmE0MTVhZTY2YWY1Yyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iZmY2ZDM3YzgyOTg0MTQ3OTk3NmI1YjY5NTk3NjBmZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1OTUyNTUsLTc5LjM0MDkyM10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85ZmY2YzFmZTJiMWE0MWVhOTNjZjY5Nzk4ZTgzMzZkMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iYmM0MjljZTdiYzQ0ZmJjYmVkYzEyOTFlNmRmY2ZkYyA9ICQoJzxkaXYgaWQ9Imh0bWxfYmJjNDI5Y2U3YmM0NGZiY2JlZGMxMjkxZTZkZmNmZGMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkVhc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWZmNmMxZmUyYjFhNDFlYTkzY2Y2OTc5OGU4MzM2ZDAuc2V0Q29udGVudChodG1sX2JiYzQyOWNlN2JjNDRmYmNiZWRjMTI5MWU2ZGZjZmRjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2JmZjZkMzdjODI5ODQxNDc5OTc2YjViNjk1OTc2MGZlLmJpbmRQb3B1cChwb3B1cF85ZmY2YzFmZTJiMWE0MWVhOTNjZjY5Nzk4ZTgzMzZkMCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yZWE4NDBkMDRjZTI0Mjc4OGE4NTlkNmJjMGVkMTVlOCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjczMzI4MjUsLTc5LjQxOTc0OTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTAzOGE2MjY5ZmJmNGY3OTk4YzBmOTExNmE0M2JkZmYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjI3NThmYTlhNDE5NGI4MWIyNzAyNWZlOWFmZGJlNjUgPSAkKCc8ZGl2IGlkPSJodG1sX2YyNzU4ZmE5YTQxOTRiODFiMjcwMjVmZTlhZmRiZTY1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ob3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81MDM4YTYyNjlmYmY0Zjc5OThjMGY5MTE2YTQzYmRmZi5zZXRDb250ZW50KGh0bWxfZjI3NThmYTlhNDE5NGI4MWIyNzAyNWZlOWFmZGJlNjUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMmVhODQwZDA0Y2UyNDI3ODhhODU5ZDZiYzBlZDE1ZTguYmluZFBvcHVwKHBvcHVwXzUwMzhhNjI2OWZiZjRmNzk5OGMwZjkxMTZhNDNiZGZmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2NiZmJhMmZmZjZhMzQ4ZWViYWVjN2EzNTk3MGJhNWUzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjkxMTE1OCwtNzkuNDc2MDEzMjk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZjQ2NmE1M2RlOTNkNDE2MTg5Y2ExOTIyOGEwMGQ4ZDcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNjNiODA5Yzk0OWIxNDlmNGFkMTVlMDVmNjBmNGE0ODIgPSAkKCc8ZGl2IGlkPSJodG1sXzYzYjgwOWM5NDliMTQ5ZjRhZDE1ZTA1ZjYwZjRhNDgyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Zb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mNDY2YTUzZGU5M2Q0MTYxODljYTE5MjI4YTAwZDhkNy5zZXRDb250ZW50KGh0bWxfNjNiODA5Yzk0OWIxNDlmNGFkMTVlMDVmNjBmNGE0ODIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfY2JmYmEyZmZmNmEzNDhlZWJhZWM3YTM1OTcwYmE1ZTMuYmluZFBvcHVwKHBvcHVwX2Y0NjZhNTNkZTkzZDQxNjE4OWNhMTkyMjhhMDBkOGQ3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzgyNmQ1OWJkZDUzMTQ2NjVhNzY1NzQzZjE0ZWY3OWMxID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzI0NzY1OSwtNzkuNTMyMjQyNDAwMDAwMDJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMGMyYjYxY2JjZDJjNGZlOWJlZjViMzEzMTYzYjM0YmIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNGU3ZWE1NDNjYmMzNDJkMDlkYmQ1MzJhNmE5MTNiMTYgPSAkKCc8ZGl2IGlkPSJodG1sXzRlN2VhNTQzY2JjMzQyZDA5ZGJkNTMyYTZhOTEzYjE2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ob3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wYzJiNjFjYmNkMmM0ZmU5YmVmNWIzMTMxNjNiMzRiYi5zZXRDb250ZW50KGh0bWxfNGU3ZWE1NDNjYmMzNDJkMDlkYmQ1MzJhNmE5MTNiMTYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfODI2ZDU5YmRkNTMxNDY2NWE3NjU3NDNmMTRlZjc5YzEuYmluZFBvcHVwKHBvcHVwXzBjMmI2MWNiY2QyYzRmZTliZWY1YjMxMzE2M2IzNGJiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzRlOTZiYjZkOWM3NjRjOWM4MzVkNTFhNTM5OGFmNzc4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjkyNjU3MDAwMDAwMDA0LC03OS4yNjQ4NDgxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2QxMjM0OTJmZjgwODQzN2VhZjNjZjhkMTgxYWNjNzNkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzA5ZGM3YWE0YjlmZDQzNDE4NjkwNDI3ZTNlY2ZjMTA3ID0gJCgnPGRpdiBpZD0iaHRtbF8wOWRjN2FhNGI5ZmQ0MzQxODY5MDQyN2UzZWNmYzEwNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2QxMjM0OTJmZjgwODQzN2VhZjNjZjhkMTgxYWNjNzNkLnNldENvbnRlbnQoaHRtbF8wOWRjN2FhNGI5ZmQ0MzQxODY5MDQyN2UzZWNmYzEwNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80ZTk2YmI2ZDljNzY0YzljODM1ZDUxYTUzOThhZjc3OC5iaW5kUG9wdXAocG9wdXBfZDEyMzQ5MmZmODA4NDM3ZWFmM2NmOGQxODFhY2M3M2QpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYTU2NDIwOGVlMDk1NGNkMzllNjU2MjIxNWI5ODY1ZWYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NzAxMTk5LC03OS40MDg0OTI3OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85YzA3Yzk2MDM4ZDg0ZTI1OTgxNzUxMTQ3ODljYTU4ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yZWZmNzliM2JlZDc0YWE1YWE4Y2NmMWFhNWM0MGVlNSA9ICQoJzxkaXYgaWQ9Imh0bWxfMmVmZjc5YjNiZWQ3NGFhNWFhOGNjZjFhYTVjNDBlZTUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzljMDdjOTYwMzhkODRlMjU5ODE3NTExNDc4OWNhNThmLnNldENvbnRlbnQoaHRtbF8yZWZmNzliM2JlZDc0YWE1YWE4Y2NmMWFhNWM0MGVlNSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hNTY0MjA4ZWUwOTU0Y2QzOWU2NTYyMjE1Yjk4NjVlZi5iaW5kUG9wdXAocG9wdXBfOWMwN2M5NjAzOGQ4NGUyNTk4MTc1MTE0Nzg5Y2E1OGYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWQwNzg4NTI2Zjg3NGJkZjgzOWM0YmRkNjJlZjRhMzQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NjE2MzEzLC03OS41MjA5OTk0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jZTJhYTFkNmQ0Zjk0ZDkyYjVkN2I1MDI5NDk5MGZiOCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84YjM2YzBlOGYxM2U0MjE4YTYyMjAzYzlhMjcyMTliZiA9ICQoJzxkaXYgaWQ9Imh0bWxfOGIzNmMwZThmMTNlNDIxOGE2MjIwM2M5YTI3MjE5YmYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NlMmFhMWQ2ZDRmOTRkOTJiNWQ3YjUwMjk0OTkwZmI4LnNldENvbnRlbnQoaHRtbF84YjM2YzBlOGYxM2U0MjE4YTYyMjAzYzlhMjcyMTliZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xZDA3ODg1MjZmODc0YmRmODM5YzRiZGQ2MmVmNGEzNC5iaW5kUG9wdXAocG9wdXBfY2UyYWExZDZkNGY5NGQ5MmI1ZDdiNTAyOTQ5OTBmYjgpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNDUyYzI5M2ZhMTkzNGNhMWFhODIxMDVkOWQxMDI3YTQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MjgwMjA1LC03OS4zODg3OTAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVkM2FmNWVkNDU0NzQ4MGU5ZTFmODNkMDBlZDFjMjM2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzYxNjM0ZDQ4Y2ZjOTQ1MTFiYjc3ZTQ3NTc1Y2M5ZjZjID0gJCgnPGRpdiBpZD0iaHRtbF82MTYzNGQ0OGNmYzk0NTExYmI3N2U0NzU3NWNjOWY2YyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81ZDNhZjVlZDQ1NDc0ODBlOWUxZjgzZDAwZWQxYzIzNi5zZXRDb250ZW50KGh0bWxfNjE2MzRkNDhjZmM5NDUxMWJiNzdlNDc1NzVjYzlmNmMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDUyYzI5M2ZhMTkzNGNhMWFhODIxMDVkOWQxMDI3YTQuYmluZFBvcHVwKHBvcHVwXzVkM2FmNWVkNDU0NzQ4MGU5ZTFmODNkMDBlZDFjMjM2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzdkZDdiM2FhOGIzYzRhMjZiMzY4MzkyYWEwNzI3Mjk1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzExNjk0OCwtNzkuNDE2OTM1NTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTRjOTlmZGFhMmYxNGEzODkyYjQ2YmRiOGJhYTg0MTYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNjY5ODZmYmVjNDQyNGRlMmIxYzAwYjQwZGUzZThiZmYgPSAkKCc8ZGl2IGlkPSJodG1sXzY2OTg2ZmJlYzQ0MjRkZTJiMWMwMGI0MGRlM2U4YmZmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DZW50cmFsIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2E0Yzk5ZmRhYTJmMTRhMzg5MmI0NmJkYjhiYWE4NDE2LnNldENvbnRlbnQoaHRtbF82Njk4NmZiZWM0NDI0ZGUyYjFjMDBiNDBkZTNlOGJmZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83ZGQ3YjNhYThiM2M0YTI2YjM2ODM5MmFhMDcyNzI5NS5iaW5kUG9wdXAocG9wdXBfYTRjOTlmZGFhMmYxNGEzODkyYjQ2YmRiOGJhYTg0MTYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfY2FkZTdmNmM3YWY5NGFlNjg5MzAzN2M0YWU4YjRkYTkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NzMxODUyOTk5OTk5OSwtNzkuNDg3MjYxOTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDdkODJlZTQyNjIxNGQ0N2I1ZjgwYzJmZjNhNjUwZTcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYmNiZGNlMWMxMmMwNDhmNGFlMTc1MDc1YjA3ZWJkYjEgPSAkKCc8ZGl2IGlkPSJodG1sX2JjYmRjZTFjMTJjMDQ4ZjRhZTE3NTA3NWIwN2ViZGIxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Zb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wN2Q4MmVlNDI2MjE0ZDQ3YjVmODBjMmZmM2E2NTBlNy5zZXRDb250ZW50KGh0bWxfYmNiZGNlMWMxMmMwNDhmNGFlMTc1MDc1YjA3ZWJkYjEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfY2FkZTdmNmM3YWY5NGFlNjg5MzAzN2M0YWU4YjRkYTkuYmluZFBvcHVwKHBvcHVwXzA3ZDgyZWU0MjYyMTRkNDdiNWY4MGMyZmYzYTY1MGU3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzU3ODZkMjljNzQ1YzRjM2E5YzA5MzljZWFiNWIwMzExID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA2ODc2LC03OS41MTgxODg0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wOTlmYmQ1ZmYzMjQ0OGVmODgzNzIzZjk5ZDFjNGEyOSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zNmM5Mzg5MjdkN2M0MzJiYmU5NzU4NDY2MmRjNjZmNCA9ICQoJzxkaXYgaWQ9Imh0bWxfMzZjOTM4OTI3ZDdjNDMyYmJlOTc1ODQ2NjJkYzY2ZjQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPllvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzA5OWZiZDVmZjMyNDQ4ZWY4ODM3MjNmOTlkMWM0YTI5LnNldENvbnRlbnQoaHRtbF8zNmM5Mzg5MjdkN2M0MzJiYmU5NzU4NDY2MmRjNjZmNCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81Nzg2ZDI5Yzc0NWM0YzNhOWMwOTM5Y2VhYjViMDMxMS5iaW5kUG9wdXAocG9wdXBfMDk5ZmJkNWZmMzI0NDhlZjg4MzcyM2Y5OWQxYzRhMjkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzYzZWNhNjA5ZTgyNGE3MTlmZTQzNWFhYTQ3YjNiNjggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NTc0MDk2LC03OS4yNzMzMDQwMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kZWFjYzU0YjY4ZDI0ODJkYTU2NTU2NWZmN2ZkOTAwYyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xNzY4ZTIxODMxZDQ0ZmE5OTM1NDUxMTNjYmM0NDRlYSA9ICQoJzxkaXYgaWQ9Imh0bWxfMTc2OGUyMTgzMWQ0NGZhOTkzNTQ1MTEzY2JjNDQ0ZWEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNjYXJib3JvdWdoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9kZWFjYzU0YjY4ZDI0ODJkYTU2NTU2NWZmN2ZkOTAwYy5zZXRDb250ZW50KGh0bWxfMTc2OGUyMTgzMWQ0NGZhOTkzNTQ1MTEzY2JjNDQ0ZWEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzYzZWNhNjA5ZTgyNGE3MTlmZTQzNWFhYTQ3YjNiNjguYmluZFBvcHVwKHBvcHVwX2RlYWNjNTRiNjhkMjQ4MmRhNTY1NTY1ZmY3ZmQ5MDBjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2VjNzk3NjlhNWY4YjRjYjNiNGVhZDA4NzUyZDI5OGNiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzUyNzU4Mjk5OTk5OTk2LC03OS40MDAwNDkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzljYjQ2NjA1NmM2NTRhZWZiNGEyM2JmZDE4YWM0NzU3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzI0NWExOGZlMzUwODQ3MjBiMDZjNzVjMmUyYmY5NGQzID0gJCgnPGRpdiBpZD0iaHRtbF8yNDVhMThmZTM1MDg0NzIwYjA2Yzc1YzJlMmJmOTRkMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWNiNDY2MDU2YzY1NGFlZmI0YTIzYmZkMThhYzQ3NTcuc2V0Q29udGVudChodG1sXzI0NWExOGZlMzUwODQ3MjBiMDZjNzVjMmUyYmY5NGQzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2VjNzk3NjlhNWY4YjRjYjNiNGVhZDA4NzUyZDI5OGNiLmJpbmRQb3B1cChwb3B1cF85Y2I0NjYwNTZjNjU0YWVmYjRhMjNiZmQxOGFjNDc1Nyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82YTBhYWY4YzE5YWU0NGZmOTI0NDYwMWZjMGUxZTVmMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxMjc1MTEsLTc5LjM5MDE5NzVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZWM4NDkwNjM2NDVmNGY0NjgwMzFiMzEzMzhmMmFkNGEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNGEwY2M3NzM4ZjBiNGEyYzlhZTdmMmFjMzI0MDNlYTggPSAkKCc8ZGl2IGlkPSJodG1sXzRhMGNjNzczOGYwYjRhMmM5YWU3ZjJhYzMyNDAzZWE4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DZW50cmFsIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2VjODQ5MDYzNjQ1ZjRmNDY4MDMxYjMxMzM4ZjJhZDRhLnNldENvbnRlbnQoaHRtbF80YTBjYzc3MzhmMGI0YTJjOWFlN2YyYWMzMjQwM2VhOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82YTBhYWY4YzE5YWU0NGZmOTI0NDYwMWZjMGUxZTVmMC5iaW5kUG9wdXAocG9wdXBfZWM4NDkwNjM2NDVmNGY0NjgwMzFiMzEzMzhmMmFkNGEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWZhNzVmZDRjNTFlNDMyYTk2MzViNWYxMjVhMmIxMTEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTY5NDc2LC03OS40MTEzMDcyMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jMzg1NzM5MTU0YWI0NDUyYTA5MzI5NzM2ODZiYzJkNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80OWE3N2M0Njc3YmU0OTU2ODliNzAyMGRlMzIxMjQ1YyA9ICQoJzxkaXYgaWQ9Imh0bWxfNDlhNzdjNDY3N2JlNDk1Njg5YjcwMjBkZTMyMTI0NWMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYzM4NTczOTE1NGFiNDQ1MmEwOTMyOTczNjg2YmMyZDcuc2V0Q29udGVudChodG1sXzQ5YTc3YzQ2NzdiZTQ5NTY4OWI3MDIwZGUzMjEyNDVjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzlmYTc1ZmQ0YzUxZTQzMmE5NjM1YjVmMTI1YTJiMTExLmJpbmRQb3B1cChwb3B1cF9jMzg1NzM5MTU0YWI0NDUyYTA5MzI5NzM2ODZiYzJkNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zNzhkYzkyZTg0MjI0ZWVhOGI1NjM2YmFiNzIzYjUzMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2MTYwODMsLTc5LjQ2NDc2MzI5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzUwNjgxNWY5MTAxZDQzNTA5NmE5NmRiNTQ2MTFjZTQ0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2Y2ZDAyYmRlNWFlZTQ0NGM5MjY3ODJjOGU0OGQwZjc5ID0gJCgnPGRpdiBpZD0iaHRtbF9mNmQwMmJkZTVhZWU0NDRjOTI2NzgyYzhlNDhkMGY3OSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2VzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81MDY4MTVmOTEwMWQ0MzUwOTZhOTZkYjU0NjExY2U0NC5zZXRDb250ZW50KGh0bWxfZjZkMDJiZGU1YWVlNDQ0YzkyNjc4MmM4ZTQ4ZDBmNzkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzc4ZGM5MmU4NDIyNGVlYThiNTYzNmJhYjcyM2I1MzMuYmluZFBvcHVwKHBvcHVwXzUwNjgxNWY5MTAxZDQzNTA5NmE5NmRiNTQ2MTFjZTQ0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzgwY2RiOGVhODQ4MzRiNDJhNjZlNDZmMTZlZTRkNjkwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjk2MzE5LC03OS41MzIyNDI0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zZTAyNjlhZjZlY2Y0MjgxOTIwYmRlYmU4NjkxYzliOSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zMjhjMjk3NjdmNTQ0MGUzYjlhMDBiMjllN2M1YzM0MCA9ICQoJzxkaXYgaWQ9Imh0bWxfMzI4YzI5NzY3ZjU0NDBlM2I5YTAwYjI5ZTdjNWMzNDAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkV0b2JpY29rZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfM2UwMjY5YWY2ZWNmNDI4MTkyMGJkZWJlODY5MWM5Yjkuc2V0Q29udGVudChodG1sXzMyOGMyOTc2N2Y1NDQwZTNiOWEwMGIyOWU3YzVjMzQwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzgwY2RiOGVhODQ4MzRiNDJhNjZlNDZmMTZlZTRkNjkwLmJpbmRQb3B1cChwb3B1cF8zZTAyNjlhZjZlY2Y0MjgxOTIwYmRlYmU4NjkxYzliOSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hMmQ1Yjc2OGU2YjU0MzJkYjhlNWY5MTc3YzZmZmU1OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc1MDA3MTUwMDAwMDAwNCwtNzkuMjk1ODQ5MV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kZWY4MmE2NWI1ZWY0MDIwYmUzMGFjM2UzOGJjNTdiNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84M2NkZWNjMGMzMzc0NThiODZkNzY2ZGY1Y2U5NThlMSA9ICQoJzxkaXYgaWQ9Imh0bWxfODNjZGVjYzBjMzM3NDU4Yjg2ZDc2NmRmNWNlOTU4ZTEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNjYXJib3JvdWdoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9kZWY4MmE2NWI1ZWY0MDIwYmUzMGFjM2UzOGJjNTdiNS5zZXRDb250ZW50KGh0bWxfODNjZGVjYzBjMzM3NDU4Yjg2ZDc2NmRmNWNlOTU4ZTEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYTJkNWI3NjhlNmI1NDMyZGI4ZTVmOTE3N2M2ZmZlNTkuYmluZFBvcHVwKHBvcHVwX2RlZjgyYTY1YjVlZjQwMjBiZTMwYWMzZTM4YmM1N2I1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ1MDhlYmU5MzUyMjQ3NmFhNzY2OTJhYmYzOWQ2ZmNhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzgyNzM2NCwtNzkuNDQyMjU5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85MGU2MWZmOGU2Nzg0YmQ3YTdmYzE0MjAxNzA3MzE3NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jOTg1Mzc2Y2IzNjQ0ZmJlYmU0OWMwOTk2MTA3NDI4NiA9ICQoJzxkaXYgaWQ9Imh0bWxfYzk4NTM3NmNiMzY0NGZiZWJlNDljMDk5NjEwNzQyODYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzkwZTYxZmY4ZTY3ODRiZDdhN2ZjMTQyMDE3MDczMTc3LnNldENvbnRlbnQoaHRtbF9jOTg1Mzc2Y2IzNjQ0ZmJlYmU0OWMwOTk2MTA3NDI4Nik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80NTA4ZWJlOTM1MjI0NzZhYTc2NjkyYWJmMzlkNmZjYS5iaW5kUG9wdXAocG9wdXBfOTBlNjFmZjhlNjc4NGJkN2E3ZmMxNDIwMTcwNzMxNzcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYzM5MzQ2YjhjNTBmNGJjYTg0ZGUzZGRlMjU2MTAzZTYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTUzODM0LC03OS40MDU2Nzg0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lN2QxOTg1YTMzYzc0YzlhYjY3NTY3MWI5NmU5Zjk3YyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82ODgxZWY1ZGJmYTY0NWM2YjNhN2VlNjgwNzQwZmM1MSA9ICQoJzxkaXYgaWQ9Imh0bWxfNjg4MWVmNWRiZmE2NDVjNmIzYTdlZTY4MDc0MGZjNTEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTdkMTk4NWEzM2M3NGM5YWI2NzU2NzFiOTZlOWY5N2Muc2V0Q29udGVudChodG1sXzY4ODFlZjVkYmZhNjQ1YzZiM2E3ZWU2ODA3NDBmYzUxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2MzOTM0NmI4YzUwZjRiY2E4NGRlM2RkZTI1NjEwM2U2LmJpbmRQb3B1cChwb3B1cF9lN2QxOTg1YTMzYzc0YzlhYjY3NTY3MWI5NmU5Zjk3Yyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82YzgwYjM1NDM3YzE0N2RiOWE0MTI0NDNhMzc2MTY4MiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY3MjcwOTcsLTc5LjQwNTY3ODQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzkyYTg2NTdmOWUwYTRiNTFhODlmZTQ3YWVmOWViYjc3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzg0NzhmMzJmMTkwMDRjZjViNzM3ZjM4Nzk0YWE3ODM3ID0gJCgnPGRpdiBpZD0iaHRtbF84NDc4ZjMyZjE5MDA0Y2Y1YjczN2YzODc5NGFhNzgzNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85MmE4NjU3ZjllMGE0YjUxYTg5ZmU0N2FlZjllYmI3Ny5zZXRDb250ZW50KGh0bWxfODQ3OGYzMmYxOTAwNGNmNWI3MzdmMzg3OTRhYTc4MzcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNmM4MGIzNTQzN2MxNDdkYjlhNDEyNDQzYTM3NjE2ODIuYmluZFBvcHVwKHBvcHVwXzkyYTg2NTdmOWUwYTRiNTFhODlmZTQ3YWVmOWViYjc3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzdmMjM2MDAwN2M2MDRhYWU5ZjBlNThkNGI5NWYyYzk5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ4OTU5NywtNzkuNDU2MzI1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzI4NzYxNjFkYmFiZDRiODk5ZDk2OWJjYzBkNWI4YmEzID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2JjNDg2Nzk0MTJjMjQwMmE4ODk3Yzk5ZTRkZDBiYmY1ID0gJCgnPGRpdiBpZD0iaHRtbF9iYzQ4Njc5NDEyYzI0MDJhODg5N2M5OWU0ZGQwYmJmNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2VzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yODc2MTYxZGJhYmQ0Yjg5OWQ5NjliY2MwZDViOGJhMy5zZXRDb250ZW50KGh0bWxfYmM0ODY3OTQxMmMyNDAyYTg4OTdjOTllNGRkMGJiZjUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfN2YyMzYwMDA3YzYwNGFhZTlmMGU1OGQ0Yjk1ZjJjOTkuYmluZFBvcHVwKHBvcHVwXzI4NzYxNjFkYmFiZDRiODk5ZDk2OWJjYzBkNWI4YmEzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzkwN2UwN2JlOWY5MzRlMzhhNjIyMGY0ZmQzMzI0OWRmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjM2OTY1NiwtNzkuNjE1ODE4OTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjg0MTQ4NTU5NTRmNDFkN2IwMjNlYjIwMzYzMDFhNWIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNTY4MGJhYzQ1NGNhNDZhYjkyZTg1YmM5MDk1MjU5YTAgPSAkKCc8ZGl2IGlkPSJodG1sXzU2ODBiYWM0NTRjYTQ2YWI5MmU4NWJjOTA5NTI1OWEwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5NaXNzaXNzYXVnYTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYjg0MTQ4NTU5NTRmNDFkN2IwMjNlYjIwMzYzMDFhNWIuc2V0Q29udGVudChodG1sXzU2ODBiYWM0NTRjYTQ2YWI5MmU4NWJjOTA5NTI1OWEwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzkwN2UwN2JlOWY5MzRlMzhhNjIyMGY0ZmQzMzI0OWRmLmJpbmRQb3B1cChwb3B1cF9iODQxNDg1NTk1NGY0MWQ3YjAyM2ViMjAzNjMwMWE1Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wODE1MjQ3ODk0ZGM0ZTU5ODI2MzQxNjgxOWQ5ZGEzNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4ODkwNTQsLTc5LjU1NDcyNDQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzRjMjdiNWU0Nzk2MTQ0NGVhN2JmMjAwYzA3MjM5MmFhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzM2ZmY3OTU2N2Y1NDRhMTc5ZWFlYzZiYWFiZmFhMmI3ID0gJCgnPGRpdiBpZD0iaHRtbF8zNmZmNzk1NjdmNTQ0YTE3OWVhZWM2YmFhYmZhYTJiNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80YzI3YjVlNDc5NjE0NDRlYTdiZjIwMGMwNzIzOTJhYS5zZXRDb250ZW50KGh0bWxfMzZmZjc5NTY3ZjU0NGExNzllYWVjNmJhYWJmYWEyYjcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDgxNTI0Nzg5NGRjNGU1OTgyNjM0MTY4MTlkOWRhMzQuYmluZFBvcHVwKHBvcHVwXzRjMjdiNWU0Nzk2MTQ0NGVhN2JmMjAwYzA3MjM5MmFhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzAyM2IxNDY5MmFmYTRhNTc5YjEwYTFkMmI2NDlhNGY1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzk0MjAwMywtNzkuMjYyMDI5NDAwMDAwMDJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDM4Mjc1MGM5NDE1NGRjYWI2YmFlYjExYmNkZWQ5NjggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjBiNWM4MzI1N2IzNDhmYzhkYTI4Y2YxYTY1ODRkYTIgPSAkKCc8ZGl2IGlkPSJodG1sX2YwYjVjODMyNTdiMzQ4ZmM4ZGEyOGNmMWE2NTg0ZGEyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDM4Mjc1MGM5NDE1NGRjYWI2YmFlYjExYmNkZWQ5Njguc2V0Q29udGVudChodG1sX2YwYjVjODMyNTdiMzQ4ZmM4ZGEyOGNmMWE2NTg0ZGEyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzAyM2IxNDY5MmFmYTRhNTc5YjEwYTFkMmI2NDlhNGY1LmJpbmRQb3B1cChwb3B1cF8wMzgyNzUwYzk0MTU0ZGNhYjZiYWViMTFiY2RlZDk2OCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wNTdjOGYyYjNjYTI0ZDU0ODBkYTc5MzMzZDYwOTAwZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcwNDMyNDQsLTc5LjM4ODc5MDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZjE0NmI1ZjI5Y2ZiNDcyNTk5ZWUzMDU4NzI4ZWVlNWEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMmM5MzRlNDViYjIzNGUyMDk2N2M2YzU2Y2E4ODViNWUgPSAkKCc8ZGl2IGlkPSJodG1sXzJjOTM0ZTQ1YmIyMzRlMjA5NjdjNmM1NmNhODg1YjVlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DZW50cmFsIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2YxNDZiNWYyOWNmYjQ3MjU5OWVlMzA1ODcyOGVlZTVhLnNldENvbnRlbnQoaHRtbF8yYzkzNGU0NWJiMjM0ZTIwOTY3YzZjNTZjYTg4NWI1ZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wNTdjOGYyYjNjYTI0ZDU0ODBkYTc5MzMzZDYwOTAwZi5iaW5kUG9wdXAocG9wdXBfZjE0NmI1ZjI5Y2ZiNDcyNTk5ZWUzMDU4NzI4ZWVlNWEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWEzZTI2YTY1ZTdlNDY0ZDg4ODhmNzI3YzZlOWI2OTQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjI2OTU2LC03OS40MDAwNDkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzllYzY0YmYzODAyZjQ5N2U4ZTY3YmFiNWJjOTBjY2M2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzA2YTJlYmJlZmJlNTQ2ZDBhOWI0NjMzNjk1Y2YyNmE0ID0gJCgnPGRpdiBpZD0iaHRtbF8wNmEyZWJiZWZiZTU0NmQwYTliNDYzMzY5NWNmMjZhNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWVjNjRiZjM4MDJmNDk3ZThlNjdiYWI1YmM5MGNjYzYuc2V0Q29udGVudChodG1sXzA2YTJlYmJlZmJlNTQ2ZDBhOWI0NjMzNjk1Y2YyNmE0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FhM2UyNmE2NWU3ZTQ2NGQ4ODg4ZjcyN2M2ZTliNjk0LmJpbmRQb3B1cChwb3B1cF85ZWM2NGJmMzgwMmY0OTdlOGU2N2JhYjViYzkwY2NjNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84YzIzMDcxMWRjM2Y0NGM3YTRlYWI0N2Y0YTkwYTAyMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MTU3MDYsLTc5LjQ4NDQ0OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDRhMWU1MWQ3YzBhNDViN2IwYTBkYTYwMDQ0YjcxOGUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYzdmZWJkMWQ3MWUyNGZkNWIzOGI1YmUzYjQxNGExYTEgPSAkKCc8ZGl2IGlkPSJodG1sX2M3ZmViZDFkNzFlMjRmZDViMzhiNWJlM2I0MTRhMWExIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XZXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzA0YTFlNTFkN2MwYTQ1YjdiMGEwZGE2MDA0NGI3MThlLnNldENvbnRlbnQoaHRtbF9jN2ZlYmQxZDcxZTI0ZmQ1YjM4YjViZTNiNDE0YTFhMSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84YzIzMDcxMWRjM2Y0NGM3YTRlYWI0N2Y0YTkwYTAyMy5iaW5kUG9wdXAocG9wdXBfMDRhMWU1MWQ3YzBhNDViN2IwYTBkYTYwMDQ0YjcxOGUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZmFlMTFkYWEyMzNlNDIyZTg0MTQ3YWMyOTU2MmI0NTkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43ODE2Mzc1LC03OS4zMDQzMDIxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzQzN2UzYmI0NzdmZjQ5MWQ5YTk2Y2RlOTFiZGJhNzRhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQxZGJlZTYzZWUxNDRmMDFiMTdiY2NmNzAwMDg5YmRjID0gJCgnPGRpdiBpZD0iaHRtbF80MWRiZWU2M2VlMTQ0ZjAxYjE3YmNjZjcwMDA4OWJkYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQzN2UzYmI0NzdmZjQ5MWQ5YTk2Y2RlOTFiZGJhNzRhLnNldENvbnRlbnQoaHRtbF80MWRiZWU2M2VlMTQ0ZjAxYjE3YmNjZjcwMDA4OWJkYyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9mYWUxMWRhYTIzM2U0MjJlODQxNDdhYzI5NTYyYjQ1OS5iaW5kUG9wdXAocG9wdXBfNDM3ZTNiYjQ3N2ZmNDkxZDlhOTZjZGU5MWJkYmE3NGEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZjZkMmQyYTJmNjM5NGVlMTllODVlOTBkNmZmMmRlNjcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42ODk1NzQzLC03OS4zODMxNTk5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wZjg3YTNiNzI5NmY0NjgzOTE0MjMyZjE0NjNhZWVkOSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83MjQ1OWI1ZThhNWI0YjIzYWVhMjBlZjg3MDYwNmU2YSA9ICQoJzxkaXYgaWQ9Imh0bWxfNzI0NTliNWU4YTViNGIyM2FlYTIwZWY4NzA2MDZlNmEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMGY4N2EzYjcyOTZmNDY4MzkxNDIzMmYxNDYzYWVlZDkuc2V0Q29udGVudChodG1sXzcyNDU5YjVlOGE1YjRiMjNhZWEyMGVmODcwNjA2ZTZhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Y2ZDJkMmEyZjYzOTRlZTE5ZTg1ZTkwZDZmZjJkZTY3LmJpbmRQb3B1cChwb3B1cF8wZjg3YTNiNzI5NmY0NjgzOTE0MjMyZjE0NjNhZWVkOSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kNzFmMTdiZTc5MDU0OWY5ODA2YjhhNGM4ZThiOWRlNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MzIwNTcsLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNjQ3MWQzZTQ1MTJkNDNjMzk5MWIwNDFlNzIxNmQxYWQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNmFiZGE1YWY0ZjEzNDBmMjkyMGM2M2IyZDVkODRkMzUgPSAkKCc8ZGl2IGlkPSJodG1sXzZhYmRhNWFmNGYxMzQwZjI5MjBjNjNiMmQ1ZDg0ZDM1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82NDcxZDNlNDUxMmQ0M2MzOTkxYjA0MWU3MjE2ZDFhZC5zZXRDb250ZW50KGh0bWxfNmFiZGE1YWY0ZjEzNDBmMjkyMGM2M2IyZDVkODRkMzUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDcxZjE3YmU3OTA1NDlmOTgwNmI4YTRjOGU4YjlkZTUuYmluZFBvcHVwKHBvcHVwXzY0NzFkM2U0NTEyZDQzYzM5OTFiMDQxZTcyMTZkMWFkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q1NjFjMmI1YTJlODRjNjBiZDRkMjc0ZWYwNzBiZGEyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuODE1MjUyMiwtNzkuMjg0NTc3Ml0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hNGJkYTBmNWU0ZWQ0ZTlhOWNiZGI0MGU0OGE2MGExMSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jNTE3MmRiMDEwZDg0M2Q0YmEwMjkyNWEwN2Q1MGE4MSA9ICQoJzxkaXYgaWQ9Imh0bWxfYzUxNzJkYjAxMGQ4NDNkNGJhMDI5MjVhMDdkNTBhODEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNjYXJib3JvdWdoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hNGJkYTBmNWU0ZWQ0ZTlhOWNiZGI0MGU0OGE2MGExMS5zZXRDb250ZW50KGh0bWxfYzUxNzJkYjAxMGQ4NDNkNGJhMDI5MjVhMDdkNTBhODEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDU2MWMyYjVhMmU4NGM2MGJkNGQyNzRlZjA3MGJkYTIuYmluZFBvcHVwKHBvcHVwX2E0YmRhMGY1ZTRlZDRlOWE5Y2JkYjQwZTQ4YTYwYTExKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzExNzU4NzBlNGY2ZTRkZDU4MDU0MzMzMGNkYmI5YjU5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjg2NDEyMjk5OTk5OTksLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZjliODYyODc0OWYzNGZlOTgwYjgxODY4ZTZkNzc0OGEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTYzOWNkMTZmMTQ4NGY2NGE3NThhYjRiMmJkNTQzYjYgPSAkKCc8ZGl2IGlkPSJodG1sXzk2MzljZDE2ZjE0ODRmNjRhNzU4YWI0YjJiZDU0M2I2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DZW50cmFsIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Y5Yjg2Mjg3NDlmMzRmZTk4MGI4MTg2OGU2ZDc3NDhhLnNldENvbnRlbnQoaHRtbF85NjM5Y2QxNmYxNDg0ZjY0YTc1OGFiNGIyYmQ1NDNiNik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xMTc1ODcwZTRmNmU0ZGQ1ODA1NDMzMzBjZGJiOWI1OS5iaW5kUG9wdXAocG9wdXBfZjliODYyODc0OWYzNGZlOTgwYjgxODY4ZTZkNzc0OGEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMGMxM2NkNmNlNjc3NGUxM2E3ZjNkZjQxZDY4ZDNmZTMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Mjg5NDY3LC03OS4zOTQ0MTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzliNGY1NzJmMzIyNzQyYjViOTVlMmI2YTA1MmU3YTYwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzhjMGUyMjNmZDlkMzRiYTNhOWIxZjY5YzBkYmRiMmJjID0gJCgnPGRpdiBpZD0iaHRtbF84YzBlMjIzZmQ5ZDM0YmEzYTliMWY2OWMwZGJkYjJiYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWI0ZjU3MmYzMjI3NDJiNWI5NWUyYjZhMDUyZTdhNjAuc2V0Q29udGVudChodG1sXzhjMGUyMjNmZDlkMzRiYTNhOWIxZjY5YzBkYmRiMmJjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzBjMTNjZDZjZTY3NzRlMTNhN2YzZGY0MWQ2OGQzZmUzLmJpbmRQb3B1cChwb3B1cF85YjRmNTcyZjMyMjc0MmI1Yjk1ZTJiNmEwNTJlN2E2MCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kYWZkY2VkMGFkYmQ0NzQyYjg0Njc2YmUxZjZiMGE5NiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYwNTY0NjYsLTc5LjUwMTMyMDcwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzE3MjI1YjFmYmNhYTQyMzliYmYxODM4OWZjY2M3NDk1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2JiOTM0MjIyMDUxMDQxMmZiZTJmNzkwYTgzMDAyMTJiID0gJCgnPGRpdiBpZD0iaHRtbF9iYjkzNDIyMjA1MTA0MTJmYmUyZjc5MGE4MzAwMjEyYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xNzIyNWIxZmJjYWE0MjM5YmJmMTgzODlmY2NjNzQ5NS5zZXRDb250ZW50KGh0bWxfYmI5MzQyMjIwNTEwNDEyZmJlMmY3OTBhODMwMDIxMmIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZGFmZGNlZDBhZGJkNDc0MmI4NDY3NmJlMWY2YjBhOTYuYmluZFBvcHVwKHBvcHVwXzE3MjI1YjFmYmNhYTQyMzliYmYxODM4OWZjY2M3NDk1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzAxNTEwNDFkM2NjYTQ2YzBiNDQwMDFiZDAwYzJmMGRiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzM5NDE2Mzk5OTk5OTk2LC03OS41ODg0MzY5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2IyZDE5NWRjYzM5ZTQ5MmZiNTllNjFjMTVkNmJlNGYwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2M5NDhmNjljYTFmYjQzMDA5MzNlYWRiNjkyZjlhOGU5ID0gJCgnPGRpdiBpZD0iaHRtbF9jOTQ4ZjY5Y2ExZmI0MzAwOTMzZWFkYjY5MmY5YThlOSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iMmQxOTVkY2MzOWU0OTJmYjU5ZTYxYzE1ZDZiZTRmMC5zZXRDb250ZW50KGh0bWxfYzk0OGY2OWNhMWZiNDMwMDkzM2VhZGI2OTJmOWE4ZTkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDE1MTA0MWQzY2NhNDZjMGI0NDAwMWJkMDBjMmYwZGIuYmluZFBvcHVwKHBvcHVwX2IyZDE5NWRjYzM5ZTQ5MmZiNTllNjFjMTVkNmJlNGYwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E4MDVjOWFmOWRiZDRhODVhZGRmYzA5MWQ3YjQzNjJiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzk5NTI1MjAwMDAwMDA1LC03OS4zMTgzODg3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzNkZTcxM2U1YzU5ZjQ0ODE4ZDUwMDc2NjgxNGU5NTIwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzNiOGE5YzRkZmJkZDQ0YzQ5NjMxZDFmZWM1MTJiM2ZmID0gJCgnPGRpdiBpZD0iaHRtbF8zYjhhOWM0ZGZiZGQ0NGM0OTYzMWQxZmVjNTEyYjNmZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzNkZTcxM2U1YzU5ZjQ0ODE4ZDUwMDc2NjgxNGU5NTIwLnNldENvbnRlbnQoaHRtbF8zYjhhOWM0ZGZiZGQ0NGM0OTYzMWQxZmVjNTEyYjNmZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hODA1YzlhZjlkYmQ0YTg1YWRkZmMwOTFkN2I0MzYyYi5iaW5kUG9wdXAocG9wdXBfM2RlNzEzZTVjNTlmNDQ4MThkNTAwNzY2ODE0ZTk1MjApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjBiNzQwMzYxYTBiNGI5Mzg4NTA5NTk4MWVmMGU2ODkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Nzk1NjI2LC03OS4zNzc1Mjk0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81ZDJkMDFjODJjYjU0MjI1OWUyMGQ5MjU0N2M3NmNmOCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83YTcyNmM4MWQzZTY0M2ZmYjU3MmEwNzZmODkwODM4NiA9ICQoJzxkaXYgaWQ9Imh0bWxfN2E3MjZjODFkM2U2NDNmZmI1NzJhMDc2Zjg5MDgzODYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzVkMmQwMWM4MmNiNTQyMjU5ZTIwZDkyNTQ3Yzc2Y2Y4LnNldENvbnRlbnQoaHRtbF83YTcyNmM4MWQzZTY0M2ZmYjU3MmEwNzZmODkwODM4Nik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82MGI3NDAzNjFhMGI0YjkzODg1MDk1OTgxZWYwZTY4OS5iaW5kUG9wdXAocG9wdXBfNWQyZDAxYzgyY2I1NDIyNTllMjBkOTI1NDdjNzZjZjgpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWZiZGFlZDY2YzU5NGJmMTgzNzE0MGI1Zjg1MDdhNmUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDY0MzUyLC03OS4zNzQ4NDU5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mNWFiMDYxMDZmYzQ0YThkYWRjMjE0NzZlODY4MmExMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kYzY5MjE0NzYxYzY0NmFlODQxMzZhNzVjOTcwYTJlNSA9ICQoJzxkaXYgaWQ9Imh0bWxfZGM2OTIxNDc2MWM2NDZhZTg0MTM2YTc1Yzk3MGEyZTUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Y1YWIwNjEwNmZjNDRhOGRhZGMyMTQ3NmU4NjgyYTEwLnNldENvbnRlbnQoaHRtbF9kYzY5MjE0NzYxYzY0NmFlODQxMzZhNzVjOTcwYTJlNSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85ZmJkYWVkNjZjNTk0YmYxODM3MTQwYjVmODUwN2E2ZS5iaW5kUG9wdXAocG9wdXBfZjVhYjA2MTA2ZmM0NGE4ZGFkYzIxNDc2ZTg2ODJhMTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfY2UxOWZkMmI2ODM5NDUwYzk4MzhhMWUxMWUwNGQ4NDUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42MDI0MTM3MDAwMDAwMSwtNzkuNTQzNDg0MDk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTBhNDljZjZjMjI0NGE5ZmJlMTE1Y2Q5MjZkYjMzOTUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODBlZWMwYTFkMDg2NDY1NTk3ZTA0OWVjZWExMjg0NzkgPSAkKCc8ZGl2IGlkPSJodG1sXzgwZWVjMGExZDA4NjQ2NTU5N2UwNDllY2VhMTI4NDc5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FdG9iaWNva2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2EwYTQ5Y2Y2YzIyNDRhOWZiZTExNWNkOTI2ZGIzMzk1LnNldENvbnRlbnQoaHRtbF84MGVlYzBhMWQwODY0NjU1OTdlMDQ5ZWNlYTEyODQ3OSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jZTE5ZmQyYjY4Mzk0NTBjOTgzOGExZTExZTA0ZDg0NS5iaW5kUG9wdXAocG9wdXBfYTBhNDljZjZjMjI0NGE5ZmJlMTE1Y2Q5MjZkYjMzOTUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWNhOGFjOGI0NGFjNDVkYmJjZjhhYzU0OWZlMzAwZTkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MDY3NDgyOTk5OTk5OTQsLTc5LjU5NDA1NDRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZThlMDU0ZmIwZjI4NDBkZWE5ZDUyOTA4OWZkYTRlMTAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZDNiNWMzYWFkMzNjNDJmOWJjMTYxMGFmMDcxZjNmMGMgPSAkKCc8ZGl2IGlkPSJodG1sX2QzYjVjM2FhZDMzYzQyZjliYzE2MTBhZjA3MWYzZjBjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FdG9iaWNva2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2U4ZTA1NGZiMGYyODQwZGVhOWQ1MjkwODlmZGE0ZTEwLnNldENvbnRlbnQoaHRtbF9kM2I1YzNhYWQzM2M0MmY5YmMxNjEwYWYwNzFmM2YwYyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xY2E4YWM4YjQ0YWM0NWRiYmNmOGFjNTQ5ZmUzMDBlOS5iaW5kUG9wdXAocG9wdXBfZThlMDU0ZmIwZjI4NDBkZWE5ZDUyOTA4OWZkYTRlMTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTk5YTUwOTE3NjA2NDA0Mjk2YTkxMDUzYmRlNjE1ODcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My44MzYxMjQ3MDAwMDAwMDYsLTc5LjIwNTYzNjA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzg4MmZlZjZlZjFiMzQ3MjViYzlkZjFiNmY1Y2E4ZTRjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzU3OGI2MGQyMmQzMDRkODQ5NDk3Yjg3OTg2YWM1NTZkID0gJCgnPGRpdiBpZD0iaHRtbF81NzhiNjBkMjJkMzA0ZDg0OTQ5N2I4Nzk4NmFjNTU2ZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzg4MmZlZjZlZjFiMzQ3MjViYzlkZjFiNmY1Y2E4ZTRjLnNldENvbnRlbnQoaHRtbF81NzhiNjBkMjJkMzA0ZDg0OTQ5N2I4Nzk4NmFjNTU2ZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85OTlhNTA5MTc2MDY0MDQyOTZhOTEwNTNiZGU2MTU4Ny5iaW5kUG9wdXAocG9wdXBfODgyZmVmNmVmMWIzNDcyNWJjOWRmMWI2ZjVjYThlNGMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNDYxMDk4YjI1YzcyNGFmZmIwNGU3ZWY5YTdhMjNmMDMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njc5NjcsLTc5LjM2NzY3NTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjhkYTBhMThlMmQ3NGI4MzljYTQ1NjliM2E1MTFmN2EgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMDhhNmRlMjBjMTQxNDMxMzlhZjEwYmQ4YTJhNzZlZGUgPSAkKCc8ZGl2IGlkPSJodG1sXzA4YTZkZTIwYzE0MTQzMTM5YWYxMGJkOGEyYTc2ZWRlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iOGRhMGExOGUyZDc0YjgzOWNhNDU2OWIzYTUxMWY3YS5zZXRDb250ZW50KGh0bWxfMDhhNmRlMjBjMTQxNDMxMzlhZjEwYmQ4YTJhNzZlZGUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDYxMDk4YjI1YzcyNGFmZmIwNGU3ZWY5YTdhMjNmMDMuYmluZFBvcHVwKHBvcHVwX2I4ZGEwYTE4ZTJkNzRiODM5Y2E0NTY5YjNhNTExZjdhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzY1ZGIxNzA2MzU0OTQxNTg4YTQ3MjdjYTNlY2I0YTBhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ4NDI5MiwtNzkuMzgyMjgwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hNzU4NTFhMTQ0MzY0MTUzOTAxODdkYTJiZGE5MjJkMSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yNzYzMmI4ODU1Njk0YzRlYjNmYjhmOGM0MGNiMGRhZCA9ICQoJzxkaXYgaWQ9Imh0bWxfMjc2MzJiODg1NTY5NGM0ZWIzZmI4ZjhjNDBjYjBkYWQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2E3NTg1MWExNDQzNjQxNTM5MDE4N2RhMmJkYTkyMmQxLnNldENvbnRlbnQoaHRtbF8yNzYzMmI4ODU1Njk0YzRlYjNmYjhmOGM0MGNiMGRhZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82NWRiMTcwNjM1NDk0MTU4OGE0NzI3Y2EzZWNiNGEwYS5iaW5kUG9wdXAocG9wdXBfYTc1ODUxYTE0NDM2NDE1MzkwMTg3ZGEyYmRhOTIyZDEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjQxOTRjZDRhNjM5NDMwMjk5MDM0YTE1OWEwZjYzYjYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTM2NTM2MDAwMDAwMDUsLTc5LjUwNjk0MzZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMmMzMjBkNzQ3MjI2NGY1NmI2MGFkNjNkNjdmNDg4YzYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfY2VhNzZkYzVhNzVlNGFlMDlkZGYzNDEzMGJmNzk2NGYgPSAkKCc8ZGl2IGlkPSJodG1sX2NlYTc2ZGM1YTc1ZTRhZTA5ZGRmMzQxMzBiZjc5NjRmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FdG9iaWNva2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzJjMzIwZDc0NzIyNjRmNTZiNjBhZDYzZDY3ZjQ4OGM2LnNldENvbnRlbnQoaHRtbF9jZWE3NmRjNWE3NWU0YWUwOWRkZjM0MTMwYmY3OTY0Zik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yNDE5NGNkNGE2Mzk0MzAyOTkwMzRhMTU5YTBmNjNiNi5iaW5kUG9wdXAocG9wdXBfMmMzMjBkNzQ3MjI2NGY1NmI2MGFkNjNkNjdmNDg4YzYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYzM3ZmUxZDNmYjg2NDMwNmI1ZDkyMWI3ZTlkYjgwMTggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjU4NTk5LC03OS4zODMxNTk5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hYzBjZmFjMjQzMDg0MzZiYjM4OTZkNmEyODVmOGNmNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zMzIyOGJkNDhlNmE0ZGM5YjhkZTRkM2Q1ZTFlOTE5YSA9ICQoJzxkaXYgaWQ9Imh0bWxfMzMyMjhiZDQ4ZTZhNGRjOWI4ZGU0ZDNkNWUxZTkxOWEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2FjMGNmYWMyNDMwODQzNmJiMzg5NmQ2YTI4NWY4Y2Y0LnNldENvbnRlbnQoaHRtbF8zMzIyOGJkNDhlNmE0ZGM5YjhkZTRkM2Q1ZTFlOTE5YSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jMzdmZTFkM2ZiODY0MzA2YjVkOTIxYjdlOWRiODAxOC5iaW5kUG9wdXAocG9wdXBfYWMwY2ZhYzI0MzA4NDM2YmIzODk2ZDZhMjg1ZjhjZjQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTRiNDE4NjA4ZDZhNDA3NWFhMTI5Y2RlZWZjZmMyMjggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjI3NDM5LC03OS4zMjE1NThdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmVkZDQ2ZDNmNGIzNDVmNmI1YWVhOTI0NGU2NTIyZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTVjNzhjMWQ1ZTNjNGNjY2I1YThiNDE4Zjg3Y2Y4NjIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYjk5OGRmMGZhYjg3NDI4Yjg4NGZiYjUwYjNhYThlOTYgPSAkKCc8ZGl2IGlkPSJodG1sX2I5OThkZjBmYWI4NzQyOGI4ODRmYmI1MGIzYWE4ZTk2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2E1Yzc4YzFkNWUzYzRjY2NiNWE4YjQxOGY4N2NmODYyLnNldENvbnRlbnQoaHRtbF9iOTk4ZGYwZmFiODc0MjhiODg0ZmJiNTBiM2FhOGU5Nik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85NGI0MTg2MDhkNmE0MDc1YWExMjljZGVlZmNmYzIyOC5iaW5kUG9wdXAocG9wdXBfYTVjNzhjMWQ1ZTNjNGNjY2I1YThiNDE4Zjg3Y2Y4NjIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjdmNzM3NTI2ZTRkNDlhM2FlNzkzNTI4NmZjYzFjOGQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42MzYyNTc5LC03OS40OTg1MDkwOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mZWRkNDZkM2Y0YjM0NWY2YjVhZWE5MjQ0ZTY1MjJkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iYzFhNGFmMTdhNzk0NDgxYjYyMmU2ZjEyMWI3ZWUyMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mNjM2MThkNjc1NjQ0MmU0YjQ2MGVhZTViZWEyMTI3ZSA9ICQoJzxkaXYgaWQ9Imh0bWxfZjYzNjE4ZDY3NTY0NDJlNGI0NjBlYWU1YmVhMjEyN2UiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkV0b2JpY29rZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYmMxYTRhZjE3YTc5NDQ4MWI2MjJlNmYxMjFiN2VlMjMuc2V0Q29udGVudChodG1sX2Y2MzYxOGQ2NzU2NDQyZTRiNDYwZWFlNWJlYTIxMjdlKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzI3ZjczNzUyNmU0ZDQ5YTNhZTc5MzUyODZmY2MxYzhkLmJpbmRQb3B1cChwb3B1cF9iYzFhNGFmMTdhNzk0NDgxYjYyMmU2ZjEyMWI3ZWUyMyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jNzAwOWYwOTJiYzE0MGQ2OWMzMDAyMjExODA5MjRiZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYyODg0MDgsLTc5LjUyMDk5OTQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZlZGQ0NmQzZjRiMzQ1ZjZiNWFlYTkyNDRlNjUyMmQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Y0MTI1NDc5OTE2MjRiNTI5MTNiOWI2OGU2NDAxM2VhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2ZkZTdjMGFlYzk3YjRmNzlhNGRhZjA3MGZiYjM5ZWZhID0gJCgnPGRpdiBpZD0iaHRtbF9mZGU3YzBhZWM5N2I0Zjc5YTRkYWYwNzBmYmIzOWVmYSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mNDEyNTQ3OTkxNjI0YjUyOTEzYjliNjhlNjQwMTNlYS5zZXRDb250ZW50KGh0bWxfZmRlN2MwYWVjOTdiNGY3OWE0ZGFmMDcwZmJiMzllZmEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYzcwMDlmMDkyYmMxNDBkNjljMzAwMjIxMTgwOTI0YmQuYmluZFBvcHVwKHBvcHVwX2Y0MTI1NDc5OTE2MjRiNTI5MTNiOWI2OGU2NDAxM2VhKTsKCiAgICAgICAgICAgIAogICAgICAgIAo8L3NjcmlwdD4= onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
df3.shape
```




    (103, 5)




```python

```
