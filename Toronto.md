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
    Solving environment: - 
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
<th>Postal Code
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
      <th>Postal Code</th>
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
df2 = df1.groupby(['Postal Code','Borough'], sort=False).agg(', '.join)
df2.reset_index(inplace=True)
df2.rename(columns={'Postal Code':'Postcode'},inplace=True)

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

for lat,lng,borough in zip(df4['Latitude'],df4['Longitude'],df4['Borough']):
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




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCcsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDMuNjUxMDcsLTc5LjM0NzAxNV0sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMCwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfNTIwYzgzYmEyZWU3NGQ4ZGI1YWU5YjRjZTJhZTgyYWMgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2QwMjA5ZDFjZWU1MTQwYWE5OWMxYTEwZWY3MzRiMzQ4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU0MjU5OSwtNzkuMzYwNjM1OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80NzFiZDZhYmQxOGE0ODNhYjAzOTMwNTc2Y2JlZWIxMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hMDlmNWYxM2FhOTU0MDlkYjhmZjI1MmE4MTE3ODE3MSA9ICQoJzxkaXYgaWQ9Imh0bWxfYTA5ZjVmMTNhYTk1NDA5ZGI4ZmYyNTJhODExNzgxNzEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQ3MWJkNmFiZDE4YTQ4M2FiMDM5MzA1NzZjYmVlYjEzLnNldENvbnRlbnQoaHRtbF9hMDlmNWYxM2FhOTU0MDlkYjhmZjI1MmE4MTE3ODE3MSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kMDIwOWQxY2VlNTE0MGFhOTljMWExMGVmNzM0YjM0OC5iaW5kUG9wdXAocG9wdXBfNDcxYmQ2YWJkMThhNDgzYWIwMzkzMDU3NmNiZWViMTMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMTFmOGY3MzMyODBhNDExMTg4MTcxYjBiYzNiYjUwMzIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjIzMDE1LC03OS4zODk0OTM4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzQ2MDNkZmRlOWM2MjRmNjdiZTY5NzMyMDhjOTI2YjM3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzhhZjY1ZThiZDEwZDQ5NzNhY2ZlODg4NDM0MDc5M2Q0ID0gJCgnPGRpdiBpZD0iaHRtbF84YWY2NWU4YmQxMGQ0OTczYWNmZTg4ODQzNDA3OTNkNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNDYwM2RmZGU5YzYyNGY2N2JlNjk3MzIwOGM5MjZiMzcuc2V0Q29udGVudChodG1sXzhhZjY1ZThiZDEwZDQ5NzNhY2ZlODg4NDM0MDc5M2Q0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzExZjhmNzMzMjgwYTQxMTE4ODE3MWIwYmMzYmI1MDMyLmJpbmRQb3B1cChwb3B1cF80NjAzZGZkZTljNjI0ZjY3YmU2OTczMjA4YzkyNmIzNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wOWNhOTU0MzZkYTU0YmNiOTFmZGJjYzJlZGU1YWU5OCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1NzE2MTgsLTc5LjM3ODkzNzA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2RkYTgyYTRjNTU4YTRjMDZhY2NiNjkxNTFlMGVjZjRkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzU5MmVlZDY3YWY2YzRmM2NhOTRjMmJiNTc3YjU4ZGY1ID0gJCgnPGRpdiBpZD0iaHRtbF81OTJlZWQ2N2FmNmM0ZjNjYTk0YzJiYjU3N2I1OGRmNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZGRhODJhNGM1NThhNGMwNmFjY2I2OTE1MWUwZWNmNGQuc2V0Q29udGVudChodG1sXzU5MmVlZDY3YWY2YzRmM2NhOTRjMmJiNTc3YjU4ZGY1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzA5Y2E5NTQzNmRhNTRiY2I5MWZkYmNjMmVkZTVhZTk4LmJpbmRQb3B1cChwb3B1cF9kZGE4MmE0YzU1OGE0YzA2YWNjYjY5MTUxZTBlY2Y0ZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zZTdhZjIxMGRlZDQ0YmZjOTc1OTY5YzQxODE0YWUxOSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MTQ5MzksLTc5LjM3NTQxNzldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNmJmMTc3NTA3YzMxNGFlMzhhMmZiMjczNDY4NmZiOGEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZGM1ZGZlNzg3NjZjNDgzNDk2NDRlMmRkNjIxZjc0ZTcgPSAkKCc8ZGl2IGlkPSJodG1sX2RjNWRmZTc4NzY2YzQ4MzQ5NjQ0ZTJkZDYyMWY3NGU3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82YmYxNzc1MDdjMzE0YWUzOGEyZmIyNzM0Njg2ZmI4YS5zZXRDb250ZW50KGh0bWxfZGM1ZGZlNzg3NjZjNDgzNDk2NDRlMmRkNjIxZjc0ZTcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfM2U3YWYyMTBkZWQ0NGJmYzk3NTk2OWM0MTgxNGFlMTkuYmluZFBvcHVwKHBvcHVwXzZiZjE3NzUwN2MzMTRhZTM4YTJmYjI3MzQ2ODZmYjhhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q5MDg2MDMzMDI5MjQyZTQ4YjBmMzkzZTc0NDU4YzJkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjc2MzU3Mzk5OTk5OTksLTc5LjI5MzAzMTJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYzY0YzNiZWUzMjI5NGM2ZWE5NWEzOTlhYjcwMmI2NDQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTk4ODI1NjJiMWExNGZiZGI2OTlmNTQ3OTViZWViNzUgPSAkKCc8ZGl2IGlkPSJodG1sX2E5ODgyNTYyYjFhMTRmYmRiNjk5ZjU0Nzk1YmVlYjc1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2M2NGMzYmVlMzIyOTRjNmVhOTVhMzk5YWI3MDJiNjQ0LnNldENvbnRlbnQoaHRtbF9hOTg4MjU2MmIxYTE0ZmJkYjY5OWY1NDc5NWJlZWI3NSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kOTA4NjAzMzAyOTI0MmU0OGIwZjM5M2U3NDQ1OGMyZC5iaW5kUG9wdXAocG9wdXBfYzY0YzNiZWUzMjI5NGM2ZWE5NWEzOTlhYjcwMmI2NDQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDMzN2M1MzA4ZDg3NGZlODk1Y2U1NDJlODA4NzQ2ZWYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDQ3NzA3OTk5OTk5OTYsLTc5LjM3MzMwNjRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODQ5NWQ4MzcyMmFhNGY3NmE3Zjg4NTQ4ZDc0NjYyMjkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNDBhODZkNGM0ZTRiNDc1ZTljNmQ1ZmRlMGM5NzQ1YjUgPSAkKCc8ZGl2IGlkPSJodG1sXzQwYTg2ZDRjNGU0YjQ3NWU5YzZkNWZkZTBjOTc0NWI1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF84NDk1ZDgzNzIyYWE0Zjc2YTdmODg1NDhkNzQ2NjIyOS5zZXRDb250ZW50KGh0bWxfNDBhODZkNGM0ZTRiNDc1ZTljNmQ1ZmRlMGM5NzQ1YjUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDMzN2M1MzA4ZDg3NGZlODk1Y2U1NDJlODA4NzQ2ZWYuYmluZFBvcHVwKHBvcHVwXzg0OTVkODM3MjJhYTRmNzZhN2Y4ODU0OGQ3NDY2MjI5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzE3OTc2MTViZDI2NTRjNDJiZjNjMjY5ZTBmMzEzOGU2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU3OTUyNCwtNzkuMzg3MzgyNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81NjY0YzVmMjgyMjY0ZDIwODI5ZTg4MDFiODA3ZGI2ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yNTM0ZjBmNWRhMWY0NjgyODMwYTQxM2FjNjg1NzQ3YiA9ICQoJzxkaXYgaWQ9Imh0bWxfMjUzNGYwZjVkYTFmNDY4MjgzMGE0MTNhYzY4NTc0N2IiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzU2NjRjNWYyODIyNjRkMjA4MjllODgwMWI4MDdkYjZmLnNldENvbnRlbnQoaHRtbF8yNTM0ZjBmNWRhMWY0NjgyODMwYTQxM2FjNjg1NzQ3Yik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xNzk3NjE1YmQyNjU0YzQyYmYzYzI2OWUwZjMxMzhlNi5iaW5kUG9wdXAocG9wdXBfNTY2NGM1ZjI4MjI2NGQyMDgyOWU4ODAxYjgwN2RiNmYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMmYyZjllYjU1N2M2NDA5ZTlmODM0NzhiMWRiMjJiOGEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njk1NDIsLTc5LjQyMjU2MzddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzRlYzJiYWI2YTVmNGExY2I3YzgzY2QyZmM4ZTQ1ODEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjA1MTU1Y2Q1OGNkNDYxZjgzOGNkYmIyNjkyODE1MDEgPSAkKCc8ZGl2IGlkPSJodG1sX2YwNTE1NWNkNThjZDQ2MWY4MzhjZGJiMjY5MjgxNTAxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83NGVjMmJhYjZhNWY0YTFjYjdjODNjZDJmYzhlNDU4MS5zZXRDb250ZW50KGh0bWxfZjA1MTU1Y2Q1OGNkNDYxZjgzOGNkYmIyNjkyODE1MDEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMmYyZjllYjU1N2M2NDA5ZTlmODM0NzhiMWRiMjJiOGEuYmluZFBvcHVwKHBvcHVwXzc0ZWMyYmFiNmE1ZjRhMWNiN2M4M2NkMmZjOGU0NTgxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q5NDRiZWNlNmZmYzQzNTdiMjBlYjlhMDdiYTgzOWYyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUwNTcxMjAwMDAwMDEsLTc5LjM4NDU2NzVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYzdmNmUwNDMzMDVhNGQ4ZmI5ZDE0NzhmZTIyMWZhMTAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNjI3ZjYyZDQ5NDRmNDk4OThhMDQ3MDYwYjMwYzhmNDAgPSAkKCc8ZGl2IGlkPSJodG1sXzYyN2Y2MmQ0OTQ0ZjQ5ODk4YTA0NzA2MGIzMGM4ZjQwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jN2Y2ZTA0MzMwNWE0ZDhmYjlkMTQ3OGZlMjIxZmExMC5zZXRDb250ZW50KGh0bWxfNjI3ZjYyZDQ5NDRmNDk4OThhMDQ3MDYwYjMwYzhmNDApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDk0NGJlY2U2ZmZjNDM1N2IyMGViOWEwN2JhODM5ZjIuYmluZFBvcHVwKHBvcHVwX2M3ZjZlMDQzMzA1YTRkOGZiOWQxNDc4ZmUyMjFmYTEwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ1NjAyOTM0MmFiYzQwNDRhOGY3ZjJhZWM1OTUyOTQ4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY5MDA1MTAwMDAwMDEsLTc5LjQ0MjI1OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYzg3N2FmY2MxN2VhNDE3Nzk0OWI3NWYzYzU3MGFhZTAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYjg5ZWFmYWVmOWUwNGZjZGJlNWYyZDcxY2M2N2JmZDggPSAkKCc8ZGl2IGlkPSJodG1sX2I4OWVhZmFlZjllMDRmY2RiZTVmMmQ3MWNjNjdiZmQ4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XZXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2M4NzdhZmNjMTdlYTQxNzc5NDliNzVmM2M1NzBhYWUwLnNldENvbnRlbnQoaHRtbF9iODllYWZhZWY5ZTA0ZmNkYmU1ZjJkNzFjYzY3YmZkOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80NTYwMjkzNDJhYmM0MDQ0YThmN2YyYWVjNTk1Mjk0OC5iaW5kUG9wdXAocG9wdXBfYzg3N2FmY2MxN2VhNDE3Nzk0OWI3NWYzYzU3MGFhZTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjU3MDMyNGI0ZWNiNDBjZGEwZDI5ZDlmNGU0YmJkMTUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDA4MTU3LC03OS4zODE3NTIyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xNzVhMGRiYmM2OGM0OTZmYjZlMWE0ZDViMjU4YmViMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85Mjk0YzgxMDg0M2E0ZGMzYmFmNTBiOWY5OWE1MThhNyA9ICQoJzxkaXYgaWQ9Imh0bWxfOTI5NGM4MTA4NDNhNGRjM2JhZjUwYjlmOTlhNTE4YTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzE3NWEwZGJiYzY4YzQ5NmZiNmUxYTRkNWIyNThiZWIyLnNldENvbnRlbnQoaHRtbF85Mjk0YzgxMDg0M2E0ZGMzYmFmNTBiOWY5OWE1MThhNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82NTcwMzI0YjRlY2I0MGNkYTBkMjlkOWY0ZTRiYmQxNS5iaW5kUG9wdXAocG9wdXBfMTc1YTBkYmJjNjhjNDk2ZmI2ZTFhNGQ1YjI1OGJlYjIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWJlM2JjNTUyYTRiNGUyOGI1NDY0YWNiMWMyMjI5MTYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDc5MjY3MDAwMDAwMDYsLTc5LjQxOTc0OTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTM5OGM5MWZjZWQwNDNiMTk2OTg2ZjM0Y2JmOWUwYzYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzZlYTVmOTBhYjYxNDc4MWEyN2VkNGU0OWI3YjFmNDEgPSAkKCc8ZGl2IGlkPSJodG1sXzc2ZWE1ZjkwYWI2MTQ3ODFhMjdlZDRlNDliN2IxZjQxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XZXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2UzOThjOTFmY2VkMDQzYjE5Njk4NmYzNGNiZjllMGM2LnNldENvbnRlbnQoaHRtbF83NmVhNWY5MGFiNjE0NzgxYTI3ZWQ0ZTQ5YjdiMWY0MSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xYmUzYmM1NTJhNGI0ZTI4YjU0NjRhY2IxYzIyMjkxNi5iaW5kUG9wdXAocG9wdXBfZTM5OGM5MWZjZWQwNDNiMTk2OTg2ZjM0Y2JmOWUwYzYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWRlNTMyNzNkNWI0NDQwNGI3NzhhOTY1NTMzNjc2YTUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Nzk1NTcxLC03OS4zNTIxODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTAzMWZlMDY5NTRhNDFlN2IzYmNkYzdhN2UxNmQzNWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMDVlZGM5ODViMGU1NDFjNGFjMGFmZTJmNjNiMjU4ZGUgPSAkKCc8ZGl2IGlkPSJodG1sXzA1ZWRjOTg1YjBlNTQxYzRhYzBhZmUyZjYzYjI1OGRlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzUwMzFmZTA2OTU0YTQxZTdiM2JjZGM3YTdlMTZkMzVmLnNldENvbnRlbnQoaHRtbF8wNWVkYzk4NWIwZTU0MWM0YWMwYWZlMmY2M2IyNThkZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hZGU1MzI3M2Q1YjQ0NDA0Yjc3OGE5NjU1MzM2NzZhNS5iaW5kUG9wdXAocG9wdXBfNTAzMWZlMDY5NTRhNDFlN2IzYmNkYzdhN2UxNmQzNWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNWM3NWI5N2MzNTA3NGI2Y2JhMmJlMWMzOTVlMTgwMmUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDcxNzY4LC03OS4zODE1NzY0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mNWFhYjkyNjUxNDY0YTQ3OWJlYWEwMzYwMmIzZDNhNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mYjBhMDgwMWY2MzI0OTUwOGJhNDdhMzliYmQzNGYxYiA9ICQoJzxkaXYgaWQ9Imh0bWxfZmIwYTA4MDFmNjMyNDk1MDhiYTQ3YTM5YmJkMzRmMWIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Y1YWFiOTI2NTE0NjRhNDc5YmVhYTAzNjAyYjNkM2E1LnNldENvbnRlbnQoaHRtbF9mYjBhMDgwMWY2MzI0OTUwOGJhNDdhMzliYmQzNGYxYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81Yzc1Yjk3YzM1MDc0YjZjYmEyYmUxYzM5NWUxODAyZS5iaW5kUG9wdXAocG9wdXBfZjVhYWI5MjY1MTQ2NGE0NzliZWFhMDM2MDJiM2QzYTUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYzQzMzMxYmMzZDgxNDhhZGEyOGU0MjcwZDkwMzdjMzYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42MzY4NDcyLC03OS40MjgxOTE0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81YjRhYWViYzZmOWE0YWIxYWExMzY5MzZjYTczOWY1MCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kMjQ3MDNhMjA1MmQ0ZTZmYjA4OGEzMTBjMmM4Y2I1OSA9ICQoJzxkaXYgaWQ9Imh0bWxfZDI0NzAzYTIwNTJkNGU2ZmIwODhhMzEwYzJjOGNiNTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNWI0YWFlYmM2ZjlhNGFiMWFhMTM2OTM2Y2E3MzlmNTAuc2V0Q29udGVudChodG1sX2QyNDcwM2EyMDUyZDRlNmZiMDg4YTMxMGMyYzhjYjU5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2M0MzMzMWJjM2Q4MTQ4YWRhMjhlNDI3MGQ5MDM3YzM2LmJpbmRQb3B1cChwb3B1cF81YjRhYWViYzZmOWE0YWIxYWExMzY5MzZjYTczOWY1MCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80Nzk1NWE1NGRjZWM0MTJmOGYzMzhkMjA0OTgxMGNmMiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2ODk5ODUsLTc5LjMxNTU3MTU5OTk5OTk4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzk3NmVkYmJkMmY1ZTQxMWRhNDM1MWYyNGZiMWM2MWM5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2ViNzY4MzFhZGFlMzQzOTdhZDg4NmZmYzRiOGY1NWZlID0gJCgnPGRpdiBpZD0iaHRtbF9lYjc2ODMxYWRhZTM0Mzk3YWQ4ODZmZmM0YjhmNTVmZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RWFzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85NzZlZGJiZDJmNWU0MTFkYTQzNTFmMjRmYjFjNjFjOS5zZXRDb250ZW50KGh0bWxfZWI3NjgzMWFkYWUzNDM5N2FkODg2ZmZjNGI4ZjU1ZmUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDc5NTVhNTRkY2VjNDEyZjhmMzM4ZDIwNDk4MTBjZjIuYmluZFBvcHVwKHBvcHVwXzk3NmVkYmJkMmY1ZTQxMWRhNDM1MWYyNGZiMWM2MWM5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk5Zjc0NmYwNTgyNzQyNTBhMzgxZjNhM2EzNjQ0ODFkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ4MTk4NSwtNzkuMzc5ODE2OTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMzI5MDNmOGRlODI1NDAzYTgyZDMzNmU5MWVlZTQ3ZWIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNTU5YzdkNDE1MjZmNDVhMTgzZmFiNzczODdmYTc1OWMgPSAkKCc8ZGl2IGlkPSJodG1sXzU1OWM3ZDQxNTI2ZjQ1YTE4M2ZhYjc3Mzg3ZmE3NTljIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8zMjkwM2Y4ZGU4MjU0MDNhODJkMzM2ZTkxZWVlNDdlYi5zZXRDb250ZW50KGh0bWxfNTU5YzdkNDE1MjZmNDVhMTgzZmFiNzczODdmYTc1OWMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOTlmNzQ2ZjA1ODI3NDI1MGEzODFmM2EzYTM2NDQ4MWQuYmluZFBvcHVwKHBvcHVwXzMyOTAzZjhkZTgyNTQwM2E4MmQzMzZlOTFlZWU0N2ViKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzVlOThmMzQ2ZTYyZTQ0OWE5YzExMDZmMzliMjU4YTMwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU5NTI1NSwtNzkuMzQwOTIzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2YzOWJjOWZiYzkxNTRmMzg5OTEwMjRiMjNkNTVkZjdmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZiZTdkZmE0YmUyODRhOWNiMjZkMTlkZGI2NTljYTFkID0gJCgnPGRpdiBpZD0iaHRtbF82YmU3ZGZhNGJlMjg0YTljYjI2ZDE5ZGRiNjU5Y2ExZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RWFzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mMzliYzlmYmM5MTU0ZjM4OTkxMDI0YjIzZDU1ZGY3Zi5zZXRDb250ZW50KGh0bWxfNmJlN2RmYTRiZTI4NGE5Y2IyNmQxOWRkYjY1OWNhMWQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNWU5OGYzNDZlNjJlNDQ5YTljMTEwNmYzOWIyNThhMzAuYmluZFBvcHVwKHBvcHVwX2YzOWJjOWZiYzkxNTRmMzg5OTEwMjRiMjNkNTVkZjdmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Y3NGQ5MmNlYzI0ZDQ4OWRhYzIzOGFmM2RlN2UzMDRjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzI4MDIwNSwtNzkuMzg4NzkwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wOGFjZDhlOGExYjU0YmRkOTU3MjlhNjdlNzYyZWQ5YyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84M2EwMmU4YzdlMjg0ZWIzODNkZDEyMjQ2NjkzZjdlMyA9ICQoJzxkaXYgaWQ9Imh0bWxfODNhMDJlOGM3ZTI4NGViMzgzZGQxMjI0NjY5M2Y3ZTMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDhhY2Q4ZThhMWI1NGJkZDk1NzI5YTY3ZTc2MmVkOWMuc2V0Q29udGVudChodG1sXzgzYTAyZThjN2UyODRlYjM4M2RkMTIyNDY2OTNmN2UzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Y3NGQ5MmNlYzI0ZDQ4OWRhYzIzOGFmM2RlN2UzMDRjLmJpbmRQb3B1cChwb3B1cF8wOGFjZDhlOGExYjU0YmRkOTU3MjlhNjdlNzYyZWQ5Yyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wNzM1YjVmNzRlY2I0ZTg2OWY0ZGY0ZWIzZjM2ZjBhOSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxMTY5NDgsLTc5LjQxNjkzNTU5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FiODhjZjI1N2ZjYjQ1NDdiZTY0MzcxZmYyMGU5MTM2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzJmYTIyMjhjNjJkNjQxMDA4ZWZkNGMyNmVmZGQxNWVlID0gJCgnPGRpdiBpZD0iaHRtbF8yZmEyMjI4YzYyZDY0MTAwOGVmZDRjMjZlZmRkMTVlZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hYjg4Y2YyNTdmY2I0NTQ3YmU2NDM3MWZmMjBlOTEzNi5zZXRDb250ZW50KGh0bWxfMmZhMjIyOGM2MmQ2NDEwMDhlZmQ0YzI2ZWZkZDE1ZWUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDczNWI1Zjc0ZWNiNGU4NjlmNGRmNGViM2YzNmYwYTkuYmluZFBvcHVwKHBvcHVwX2FiODhjZjI1N2ZjYjQ1NDdiZTY0MzcxZmYyMGU5MTM2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2FhMzQ2YWRmNTk5NjQ4MjY5ZjRkZjczNTkyYWVkZGY3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzEyNzUxMSwtNzkuMzkwMTk3NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mZGJhNDhmODhiNWI0MjMzYTkyOTZlMTc5MGE2NzJjOCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hODIwZWQxNmZjZjc0MjhkOGQ5YThjN2Y0ZjYzZDM4YiA9ICQoJzxkaXYgaWQ9Imh0bWxfYTgyMGVkMTZmY2Y3NDI4ZDhkOWE4YzdmNGY2M2QzOGIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmRiYTQ4Zjg4YjViNDIzM2E5Mjk2ZTE3OTBhNjcyYzguc2V0Q29udGVudChodG1sX2E4MjBlZDE2ZmNmNzQyOGQ4ZDlhOGM3ZjRmNjNkMzhiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FhMzQ2YWRmNTk5NjQ4MjY5ZjRkZjczNTkyYWVkZGY3LmJpbmRQb3B1cChwb3B1cF9mZGJhNDhmODhiNWI0MjMzYTkyOTZlMTc5MGE2NzJjOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zZGQyYjFhNTRlMzI0NWRhOTFiMTVkNTBjMzliYjgwYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY5Njk0NzYsLTc5LjQxMTMwNzIwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzc1ZmJiYWY0NzYzMDQ2Y2E4M2VjY2VjMjIzNGQyZjYwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZkNTUxMzY1YjUyNDRhYjVhYmE0YWM4ZDVhZDE0ODJmID0gJCgnPGRpdiBpZD0iaHRtbF82ZDU1MTM2NWI1MjQ0YWI1YWJhNGFjOGQ1YWQxNDgyZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83NWZiYmFmNDc2MzA0NmNhODNlY2NlYzIyMzRkMmY2MC5zZXRDb250ZW50KGh0bWxfNmQ1NTEzNjViNTI0NGFiNWFiYTRhYzhkNWFkMTQ4MmYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfM2RkMmIxYTU0ZTMyNDVkYTkxYjE1ZDUwYzM5YmI4MGMuYmluZFBvcHVwKHBvcHVwXzc1ZmJiYWY0NzYzMDQ2Y2E4M2VjY2VjMjIzNGQyZjYwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJjYzQ3NDI1OTgwODRlMDFiMjA1MWZlM2YwMDc4NTJlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjYxNjA4MywtNzkuNDY0NzYzMjk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfY2UyNmJlNGVjYzk5NDA0OWIwZTkxMjg2MzM4YzQ5YzQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMmY2ZjE0YmU0ODM2NDRjY2JiODg3MjhmODg4YTJhZjEgPSAkKCc8ZGl2IGlkPSJodG1sXzJmNmYxNGJlNDgzNjQ0Y2NiYjg4NzI4Zjg4OGEyYWYxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XZXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NlMjZiZTRlY2M5OTQwNDliMGU5MTI4NjMzOGM0OWM0LnNldENvbnRlbnQoaHRtbF8yZjZmMTRiZTQ4MzY0NGNjYmI4ODcyOGY4ODhhMmFmMSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yY2M0NzQyNTk4MDg0ZTAxYjIwNTFmZTNmMDA3ODUyZS5iaW5kUG9wdXAocG9wdXBfY2UyNmJlNGVjYzk5NDA0OWIwZTkxMjg2MzM4YzQ5YzQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNDgzYjI2YmYzOTE0NDQzNTg4YmU5NTM2MzFiYjUwYzIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTUzODM0LC03OS40MDU2Nzg0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jZTJlNzQ2MTkxMjc0MjFkYWFjOTJkYjZlMTlhYmY5OCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80YjgzNWNjYjkzNjc0OTU5ODFhZWFiOWI2ODRiZjlkOCA9ICQoJzxkaXYgaWQ9Imh0bWxfNGI4MzVjY2I5MzY3NDk1OTgxYWVhYjliNjg0YmY5ZDgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2UyZTc0NjE5MTI3NDIxZGFhYzkyZGI2ZTE5YWJmOTguc2V0Q29udGVudChodG1sXzRiODM1Y2NiOTM2NzQ5NTk4MWFlYWI5YjY4NGJmOWQ4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzQ4M2IyNmJmMzkxNDQ0MzU4OGJlOTUzNjMxYmI1MGMyLmJpbmRQb3B1cChwb3B1cF9jZTJlNzQ2MTkxMjc0MjFkYWFjOTJkYjZlMTlhYmY5OCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hMTVjNDM3NDFhM2U0MTZmOTZiOTkyZmFjMzc1MmNlMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY3MjcwOTcsLTc5LjQwNTY3ODQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2E2ZjEzMjRjY2RkOTQ4YTc5ZmY2OTA2MzgxYjZkNzkxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2U0MzZiYmZiZGQ0NjRjNzc4OWY2ZDI4YzFlN2RiODU3ID0gJCgnPGRpdiBpZD0iaHRtbF9lNDM2YmJmYmRkNDY0Yzc3ODlmNmQyOGMxZTdkYjg1NyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hNmYxMzI0Y2NkZDk0OGE3OWZmNjkwNjM4MWI2ZDc5MS5zZXRDb250ZW50KGh0bWxfZTQzNmJiZmJkZDQ2NGM3Nzg5ZjZkMjhjMWU3ZGI4NTcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYTE1YzQzNzQxYTNlNDE2Zjk2Yjk5MmZhYzM3NTJjZTEuYmluZFBvcHVwKHBvcHVwX2E2ZjEzMjRjY2RkOTQ4YTc5ZmY2OTA2MzgxYjZkNzkxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJhZWM1ZGRlNDAyMzQwNDU4NzNhNDU4MTEzMjFmZmFlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ4OTU5NywtNzkuNDU2MzI1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzY2YjA5YWI2YjkxMzQ3MzM4MzM3OGY1MGMwY2FjYTI1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzk1MWFhYWIyMjVkNzQ3YmZiNTFkYjc4ZjMzOGQ0NzQ3ID0gJCgnPGRpdiBpZD0iaHRtbF85NTFhYWFiMjI1ZDc0N2JmYjUxZGI3OGYzMzhkNDc0NyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2VzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82NmIwOWFiNmI5MTM0NzMzODMzNzhmNTBjMGNhY2EyNS5zZXRDb250ZW50KGh0bWxfOTUxYWFhYjIyNWQ3NDdiZmI1MWRiNzhmMzM4ZDQ3NDcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMmFlYzVkZGU0MDIzNDA0NTg3M2E0NTgxMTMyMWZmYWUuYmluZFBvcHVwKHBvcHVwXzY2YjA5YWI2YjkxMzQ3MzM4MzM3OGY1MGMwY2FjYTI1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2NhMjBlZmIzY2Y2ODRhZTFiNTAzYTk1ZDc2NWRkNTkyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA0MzI0NCwtNzkuMzg4NzkwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lOTk3YTI2NzAxZjc0NzJlOWVkOGNjMTIyNzdlOTY3MSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zYWE4MWE1ZmI3ZTk0Y2M0OTc3MjU5YWViYTNjYTFhZiA9ICQoJzxkaXYgaWQ9Imh0bWxfM2FhODFhNWZiN2U5NGNjNDk3NzI1OWFlYmEzY2ExYWYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTk5N2EyNjcwMWY3NDcyZTllZDhjYzEyMjc3ZTk2NzEuc2V0Q29udGVudChodG1sXzNhYTgxYTVmYjdlOTRjYzQ5NzcyNTlhZWJhM2NhMWFmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2NhMjBlZmIzY2Y2ODRhZTFiNTAzYTk1ZDc2NWRkNTkyLmJpbmRQb3B1cChwb3B1cF9lOTk3YTI2NzAxZjc0NzJlOWVkOGNjMTIyNzdlOTY3MSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xMGYxMzMwNzRjNzc0YTM1YmVhYmQ1Y2NkZDNlNDJlZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2MjY5NTYsLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMzFjNDJhYmNlOTM2NDg0Mjg5MWI0ZWZlNTk0NWMxNzAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZThiYWVkNzI2MjI1NDQ2ZThhNzA0Mzg2MTVkNWNhNDcgPSAkKCc8ZGl2IGlkPSJodG1sX2U4YmFlZDcyNjIyNTQ0NmU4YTcwNDM4NjE1ZDVjYTQ3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8zMWM0MmFiY2U5MzY0ODQyODkxYjRlZmU1OTQ1YzE3MC5zZXRDb250ZW50KGh0bWxfZThiYWVkNzI2MjI1NDQ2ZThhNzA0Mzg2MTVkNWNhNDcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMTBmMTMzMDc0Yzc3NGEzNWJlYWJkNWNjZGQzZTQyZWUuYmluZFBvcHVwKHBvcHVwXzMxYzQyYWJjZTkzNjQ4NDI4OTFiNGVmZTU5NDVjMTcwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk5ODg1ZjY1NjRjMzQ1ODA5YTYzN2MzYmU4NjY0MzZiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUxNTcwNiwtNzkuNDg0NDQ5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83NDE1MTUzNzllMTk0NmJmYWIwODdmYWVhYWZkMmM1ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83NzA3ZDM5N2Q0MDg0MDYzYjUxMWRhMWZlZGFkNmExZSA9ICQoJzxkaXYgaWQ9Imh0bWxfNzcwN2QzOTdkNDA4NDA2M2I1MTFkYTFmZWRhZDZhMWUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNzQxNTE1Mzc5ZTE5NDZiZmFiMDg3ZmFlYWFmZDJjNWQuc2V0Q29udGVudChodG1sXzc3MDdkMzk3ZDQwODQwNjNiNTExZGExZmVkYWQ2YTFlKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzk5ODg1ZjY1NjRjMzQ1ODA5YTYzN2MzYmU4NjY0MzZiLmJpbmRQb3B1cChwb3B1cF83NDE1MTUzNzllMTk0NmJmYWIwODdmYWVhYWZkMmM1ZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zMjY3MjE1NDA3NDY0OWMyOGZjZGMxZjE2ZDYxZWRkYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4OTU3NDMsLTc5LjM4MzE1OTkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2U4ODZkZjQxOGViNzRhNDE5MDdlNDIyMDEyZGNiNjY2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzgzMmYwY2FlMzIzYzQwZjViYjcxNWFkODMxMGI0NzAzID0gJCgnPGRpdiBpZD0iaHRtbF84MzJmMGNhZTMyM2M0MGY1YmI3MTVhZDgzMTBiNDcwMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lODg2ZGY0MThlYjc0YTQxOTA3ZTQyMjAxMmRjYjY2Ni5zZXRDb250ZW50KGh0bWxfODMyZjBjYWUzMjNjNDBmNWJiNzE1YWQ4MzEwYjQ3MDMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzI2NzIxNTQwNzQ2NDljMjhmY2RjMWYxNmQ2MWVkZGMuYmluZFBvcHVwKHBvcHVwX2U4ODZkZjQxOGViNzRhNDE5MDdlNDIyMDEyZGNiNjY2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2UyOWJkZWNiMDE3OTQxNTBhMWQxZjBjNDA2MGM4MjQ0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUzMjA1NywtNzkuNDAwMDQ5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iNTA3YTEzZjY2OTM0NDRjODFmNzdhMTNmYjQ3N2UwNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83NTFmNmJjOTI2M2M0OGNmOTk2ZDgyN2NiMzMyZmJlNyA9ICQoJzxkaXYgaWQ9Imh0bWxfNzUxZjZiYzkyNjNjNDhjZjk5NmQ4MjdjYjMzMmZiZTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2I1MDdhMTNmNjY5MzQ0NGM4MWY3N2ExM2ZiNDc3ZTA3LnNldENvbnRlbnQoaHRtbF83NTFmNmJjOTI2M2M0OGNmOTk2ZDgyN2NiMzMyZmJlNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lMjliZGVjYjAxNzk0MTUwYTFkMWYwYzQwNjBjODI0NC5iaW5kUG9wdXAocG9wdXBfYjUwN2ExM2Y2NjkzNDQ0YzgxZjc3YTEzZmI0NzdlMDcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWI2NmQ5YzA2YjYzNDBiYmI0YmMxYjc4ZjJhNGFkMTcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42ODY0MTIyOTk5OTk5OSwtNzkuNDAwMDQ5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9hYmNiYjMwNjgzZDU0MzM5YjE1ZGI5OWJiYzRhYTcxZCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jY2ZiNmFlZDAzNTA0ZGY0ODE1YjFjM2U4ZWE3ZTdlNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yODkyZGQ1YTJiOTc0ZDFmOTIzNTY5ZmRkN2EwMmZjMyA9ICQoJzxkaXYgaWQ9Imh0bWxfMjg5MmRkNWEyYjk3NGQxZjkyMzU2OWZkZDdhMDJmYzMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2NmYjZhZWQwMzUwNGRmNDgxNWIxYzNlOGVhN2U3ZTQuc2V0Q29udGVudChodG1sXzI4OTJkZDVhMmI5NzRkMWY5MjM1NjlmZGQ3YTAyZmMzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzliNjZkOWMwNmI2MzQwYmJiNGJjMWI3OGYyYTRhZDE3LmJpbmRQb3B1cChwb3B1cF9jY2ZiNmFlZDAzNTA0ZGY0ODE1YjFjM2U4ZWE3ZTdlNCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82MmJmNTA5OGQwYzQ0NTRhOTUwNDc3NTgwNWU4NDI1NSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYyODk0NjcsLTc5LjM5NDQxOTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMWQzNGM1MWIyMTdkNGYxYTlhNGRhNTMyOTI0MzEwY2IgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMmI5ZjA1Yjg4Y2NjNGFhNjg2YzdjZDFmNmM5YWI0ZDUgPSAkKCc8ZGl2IGlkPSJodG1sXzJiOWYwNWI4OGNjYzRhYTY4NmM3Y2QxZjZjOWFiNGQ1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xZDM0YzUxYjIxN2Q0ZjFhOWE0ZGE1MzI5MjQzMTBjYi5zZXRDb250ZW50KGh0bWxfMmI5ZjA1Yjg4Y2NjNGFhNjg2YzdjZDFmNmM5YWI0ZDUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNjJiZjUwOThkMGM0NDU0YTk1MDQ3NzU4MDVlODQyNTUuYmluZFBvcHVwKHBvcHVwXzFkMzRjNTFiMjE3ZDRmMWE5YTRkYTUzMjkyNDMxMGNiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q5YjAyNWY3NzQ3ZTRiNjU5N2U5Y2VhYjIzNTcwZWNkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjc5NTYyNiwtNzkuMzc3NTI5NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOTFlMjJjMDYxMzM4NDIwMWI0NTdhNzBmOTI5ZTdhMWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNmZkNzE5ZWFjYzYwNGRhZDllMzcwOWM0ZmJkMDQ1ZDEgPSAkKCc8ZGl2IGlkPSJodG1sXzZmZDcxOWVhY2M2MDRkYWQ5ZTM3MDljNGZiZDA0NWQxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85MWUyMmMwNjEzMzg0MjAxYjQ1N2E3MGY5MjllN2ExZi5zZXRDb250ZW50KGh0bWxfNmZkNzE5ZWFjYzYwNGRhZDllMzcwOWM0ZmJkMDQ1ZDEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDliMDI1Zjc3NDdlNGI2NTk3ZTljZWFiMjM1NzBlY2QuYmluZFBvcHVwKHBvcHVwXzkxZTIyYzA2MTMzODQyMDFiNDU3YTcwZjkyOWU3YTFmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzcwNjEyY2U5YjkzODRhMTRiZTVhZjMwODg3Y2Q1NjI4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ2NDM1MiwtNzkuMzc0ODQ1OTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNjUwYTNjOTU0YjA0NGQ3NGJhNTg5YmU3OTU2M2I3OTYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjQ2OTM3OTQ3YmViNGQ1ZGEzZjNlZTQ0NjdjMWMyMGUgPSAkKCc8ZGl2IGlkPSJodG1sX2Y0NjkzNzk0N2JlYjRkNWRhM2YzZWU0NDY3YzFjMjBlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82NTBhM2M5NTRiMDQ0ZDc0YmE1ODliZTc5NTYzYjc5Ni5zZXRDb250ZW50KGh0bWxfZjQ2OTM3OTQ3YmViNGQ1ZGEzZjNlZTQ0NjdjMWMyMGUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNzA2MTJjZTliOTM4NGExNGJlNWFmMzA4ODdjZDU2MjguYmluZFBvcHVwKHBvcHVwXzY1MGEzYzk1NGIwNDRkNzRiYTU4OWJlNzk1NjNiNzk2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzRhNDY3MTg5ZjlhODQ1NDliMWI2MTI4OWIzOTcyMGM2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY3OTY3LC03OS4zNjc2NzUzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2UzODdkNzVhZDJjMjRlMTk4MjVkMGQ4ZGZkZjhjN2RhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQ2YmYzYjQ4YWZlMzRmNzQ4ZWVlNTI0MjM5OWZlMzgwID0gJCgnPGRpdiBpZD0iaHRtbF80NmJmM2I0OGFmZTM0Zjc0OGVlZTUyNDIzOTlmZTM4MCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTM4N2Q3NWFkMmMyNGUxOTgyNWQwZDhkZmRmOGM3ZGEuc2V0Q29udGVudChodG1sXzQ2YmYzYjQ4YWZlMzRmNzQ4ZWVlNTI0MjM5OWZlMzgwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRhNDY3MTg5ZjlhODQ1NDliMWI2MTI4OWIzOTcyMGM2LmJpbmRQb3B1cChwb3B1cF9lMzg3ZDc1YWQyYzI0ZTE5ODI1ZDBkOGRmZGY4YzdkYSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81MjU5NjlmYWQ0Mjc0N2YxYjc3NTFkMTM1YTIyM2U0NSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0ODQyOTIsLTc5LjM4MjI4MDJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNGYyZjc5NDJkZDFlNDdlNmI2NjMxZDZjZDVlNDA0MDYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMzRhNjUwZTAwMGZkNDE2OGIxMDk0YjExYjQwNjA5NzEgPSAkKCc8ZGl2IGlkPSJodG1sXzM0YTY1MGUwMDBmZDQxNjhiMTA5NGIxMWI0MDYwOTcxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80ZjJmNzk0MmRkMWU0N2U2YjY2MzFkNmNkNWU0MDQwNi5zZXRDb250ZW50KGh0bWxfMzRhNjUwZTAwMGZkNDE2OGIxMDk0YjExYjQwNjA5NzEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNTI1OTY5ZmFkNDI3NDdmMWI3NzUxZDEzNWEyMjNlNDUuYmluZFBvcHVwKHBvcHVwXzRmMmY3OTQyZGQxZTQ3ZTZiNjYzMWQ2Y2Q1ZTQwNDA2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzMyNGI3MjljZjIxMDQwMWE5Nzg0NmQ2ODM0ZmQ0OGU0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY1ODU5OSwtNzkuMzgzMTU5OTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYWJjYmIzMDY4M2Q1NDMzOWIxNWRiOTliYmM0YWE3MWQpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMTFiODg2NjcwM2I5NDY4YmJkYzJkZDg3NDUwNTBkYjMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNWE3OTI4MWMzZDE1NDgxOTk0YTM2NWFkY2QyYzgzMmQgPSAkKCc8ZGl2IGlkPSJodG1sXzVhNzkyODFjM2QxNTQ4MTk5NGEzNjVhZGNkMmM4MzJkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xMWI4ODY2NzAzYjk0NjhiYmRjMmRkODc0NTA1MGRiMy5zZXRDb250ZW50KGh0bWxfNWE3OTI4MWMzZDE1NDgxOTk0YTM2NWFkY2QyYzgzMmQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzI0YjcyOWNmMjEwNDAxYTk3ODQ2ZDY4MzRmZDQ4ZTQuYmluZFBvcHVwKHBvcHVwXzExYjg4NjY3MDNiOTQ2OGJiZGMyZGQ4NzQ1MDUwZGIzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2IwN2NhOWNiYjZhZjRkNzZiMWZmYmZlMjMzMzFlMGNjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjYyNzQzOSwtNzkuMzIxNTU4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2FiY2JiMzA2ODNkNTQzMzliMTVkYjk5YmJjNGFhNzFkKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FiOGY2NTk2NzkwZjQ0ZDRiNWFhZjdlMDZmMDFjMjZmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2VmOGU3NzllYTYwMzQyYTQ5M2VmYmQ4ZDBhMjJkMGVlID0gJCgnPGRpdiBpZD0iaHRtbF9lZjhlNzc5ZWE2MDM0MmE0OTNlZmJkOGQwYTIyZDBlZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RWFzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hYjhmNjU5Njc5MGY0NGQ0YjVhYWY3ZTA2ZjAxYzI2Zi5zZXRDb250ZW50KGh0bWxfZWY4ZTc3OWVhNjAzNDJhNDkzZWZiZDhkMGEyMmQwZWUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYjA3Y2E5Y2JiNmFmNGQ3NmIxZmZiZmUyMzMzMWUwY2MuYmluZFBvcHVwKHBvcHVwX2FiOGY2NTk2NzkwZjQ0ZDRiNWFhZjdlMDZmMDFjMjZmKTsKCiAgICAgICAgICAgIAogICAgICAgIAo8L3NjcmlwdD4= onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python

```
