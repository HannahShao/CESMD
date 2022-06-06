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
'''

### **Parameters and description**

| Parameters ||Description |  ||
| :- | -: || -: | -: | 
|Circle||Return events within a given distance (in kilometers) of a location|
|Coordinates||Return stations within a range of latitudes or longitudes|
|Country/State||Return events in a given country or U.S. state|
|Date/Time||Search by date and time (in UTC) in yyyy-mm-dd hh:mm:ss format|
|Earthquake Name||May contain the nearby city to the epicenter. A search for a nearby city returns events whose earthquake name contains the name of the nearby city. |
|Epicentral Dist. (km)||Specify an epicentral distance range (min to max inclusive) to narrow down search criteria|
|Event IDs||Search event(s) by ID(s), e.g. CI37904927,CI38043999. Most other options are unavailable when an event ID is specified. |
|Fault Type||Return events with specific fault type. Multiple fault types may be selected for the search. Leaving them unselected will search for all possible types. |
|Group By||Specify the grouping of the metadata. Default is grouping by stations. Grouping by stations will group records under the same station into a single data structure. |
|Include Inactive Stations||Check to include inactive stations in search criteria, uncheck otherwise. |
|Login Email||Available if dataset is selected as the return type. Registered e-mail must be provided in order to download data files. |
|Metadata Format||Specify output format. Default is JSON. Available if metadata is selected as the return type. |
|Magnitude||Self-explanatory. Returns events within a range of magnitudes. Default returns all magnitude ranges|
|Network||Two letter Network ID of station (multiple network IDs can be listed, e.g. CE,AA) |
|Order Records By||Specifies the sort order of the record output. Default is by epicentral distance (descending). |
|PGA (g)||Specify a PGA range (min to max inclusive) to narrow down search criteria|
|Processed, Raw, Plot||Available if dataset is selected as the return type. Checking processed and raw will package both processed and raw data in the returned (.ZIP) file. |
|Return Type||Return type of the output. Dataset will return a (.ZIP) file packaged with the requested data. Metadata will return either (.JSON) or (.XML), depending on user selection. |
|Station Code(s)||Search station(s) by code(s), e.g. CE24319,AAAK16. Most other options are unavailable when a station code is specified. |
|Station Name||Name of station, accepts wildcards|
|Station Type||Default is Any, alternative options are Array, Ground, Building, Bridge, Dam, Tunnel, Wharf, Other|
