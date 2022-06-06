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
