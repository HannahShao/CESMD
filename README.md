# CESMD

The Center for Engineering Strong Motion Data (CESMD) is a cooperative center established by the US Geological Survey (USGS) and the California Geological Survey (CGS) to integrate earthquake strong-motion data from the CGS California Strong Motion Instrumentation Program, the USGS National Strong Motion Project, and the Advanced National Seismic System (ANSS). The CESMD provides raw and processed strong-motion data for earthquake engineering applications.

## My Binder: https://mybinder.org/v2/gh/HannahShao/CESMD.git/HEAD
----
### CESMD web services: https://www.strongmotioncenter.org/wserv/
![image](https://user-images.githubusercontent.com/74167887/171951585-d8d5909d-619a-4cb2-a8ed-85a9c4789af4.png)

#### Python Demo

We will start with the necessary imports
```Python
import urllib
import json
import urllib.request as urllib2
import pandas as pd
import csv
import sys
import math
import numpy as np

%matplotlib inline
import matplotlib.pyplot as plt
from mpl_toolkits.basemap import Basemap
from mpl_toolkits.basemap import Basemap
import matplotlib.pyplot as plt
from matplotlib.patches import Polygon

import colorama
from colorama import Fore

import folium
from folium import Choropleth, Circle, Marker
from folium.plugins import HeatMap, MarkerCluster
```

#### Patameters and Description
![image](https://user-images.githubusercontent.com/74167887/172212209-3ee0cf25-6c8d-453f-aa09-6475a8ee07e8.png)


Query countries and states list
``` Python
country_list = ["US","British Virgin Islands","Canada","Chile",
        "Costa Rica","Cuba","Ecuador", "Guatemala","Haiti","Indonesia","Italy",
        "Japan","Kermadec Islands","Mexico","Nepal","New Zealand","Papua New Guinea",
        "Puerto Rico","Solomon Islands","Spain", "Taiwan", "Tonga","Turkey","Vanuatu"]
state_list = ["AK","AR","CA","DE","HI","ID","IL","KS","MT","NV","OK","OR","SC","VA","WA"]
```

##### There are some python functions to make the webservice URL for query

### Start Query

Start adding parameters to the basic URL.
By running the python function, it will ask user to enter the parameters. 
```Python
# Sample Query URL 
url = 'https://www.strongmotioncenter.org/wserv/events/query?&orderby=time&format=csv&nodata=404' # initial url with csv format
event_url = query_earthquake(url)
```
![image](https://user-images.githubusercontent.com/74167887/172213349-b4399787-b038-4771-bfa5-cf518498e834.png)

Now we will read in the example data
```Python
data = open_url(event_url)
print(data.describe())
data.head()
```
![image](https://user-images.githubusercontent.com/74167887/172213523-78f5a3ac-a543-470c-83d5-90cad4933284.png)
![image](https://user-images.githubusercontent.com/74167887/172215295-1b802fe1-f901-457b-b915-56e3e265dd87.png)
