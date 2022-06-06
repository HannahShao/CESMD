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

##### Query function to make the webservice URL
```Python
"""LOCATION QUERY FUNCTION"""
# By Country
def earthquake_country(url):
    print('Country List:\n',country_list)
    country= input('What is the country?')  
    if country != '':
        url = url+'&country='+country
        print('State List:\n',state_list)
        state= input('what is the state?') 
        if state != '':
            url = url+'&state='+state
        else: 
            print('No input state.')
    else: 
        print('No input country.')    
    print(url)
    return url




# By Circle
def earthquake_country_circle(url):
    latitue = input('Latitude at circle center')  
    longitude = input('Longitude at circle center')
    radius = input ('Radius(km) from circle center')
    if  latitue and longitude and radius: 
        url = url+f'&lat={latitue}&lon={longitude}&rad={radius}'
        print(url)
    else:
        print(Fore.RED + 'You may missing at least one parameter, the url will not contain the circle query')
        print(url)
    return url




# By Coordinates
def earthquake_Coordinates(url):
    minlat=input('Min Latitude') 
    maxlat=input('Max Latitude') 
    minlon=input('Min Longitude')
    maxlon=input('Max Longitude')
    
    if  minlat and maxlat and minlon and maxlon: 
        url = url +f'&minlat={minlat}&maxlat={maxlat}&minlon={minlon}&maxlon={maxlon}'
        print(url)
    else:
        print(Fore.RED + 'You may missing at least one parameter, the url will not contain the coordinates query')
        print(url)
    return url
    
# Start with create the query url

def earthquake_parameter(url):
    # Start enter the input and check the format to make url
    print("For the following question, please follow the instructions or skip by pushing the 'return' button. Thanks!\n")
    #
    minmag= input('Min magnitude   (a number between 0~10):')
    maxmag= input('Max magnitude   (a number between 0~10, greater than min magnitude):')
    startdate= input('Enter a start date (ex.2021-08-25, 2021-08-14T11:57:43.715Z):')    
    enddate=input('Enter a end date (ex.2021-08-25, 2021-08-14T11:57:43.715Z, later than start date):')
    faulttype = input('Enter a fault type (ex.NM,RS,SS. Default is any):')

    
    # Eroor Handeling
    if minmag != '' and float(minmag):
        url = url+'&minmag='+minmag
    else: 
        print(Fore.RED +'No min magnitude or input is not a number.')
        

    if maxmag != '' and float(maxmag):
        if minmag == '' or float(maxmag) >= float(minmag):
            url = url+'&maxmag='+maxmag
        else:
            print(Fore.RED +'Max magnitude is not a number.')
    else: 
        print(Fore.RED +'No max magnitude.')

    if startdate != '':
        url = url+'&startdate='+startdate
    else: 
        print(Fore.RED +'No input start date.')

    if enddate != '':
        url = url+'&enddate='+startdate
    else: 
        print(Fore.RED +'No input end date.')    
    
    if faulttype != '':
        url = url+'&faulttype='+faulttype
    else:
        print(Fore.RED +'fault type is any')
        

    #orderby=time
    #format=csv
    print(url)
    return url






# ask users input and create the complete URL by adding pieces 
def query_earthquake(url):
    event_id = input('Do you have an id number? (Yes/No)')
    # event id is the primary 
    if  event_id.lower() == 'yes':
        id_ = input('Event ID:')
        url = url +f'&eventid={id_}'
    # parameters and location to the url 
    else:
        query = input('\nDo you want to query by parameters, location, or both? (parameters/location/both)')
        if query.lower() == 'parameters':
            url = earthquake_parameter(url)
        elif query.lower() == 'location':
            e,s='',''
            loc_type= input('Do you want to query by country, circle or coordination?')
            if loc_type.lower().lower() == 'country':
                url = earthquake_country(url)
            elif loc_type.lower() == 'circle':
                url =  earthquake_country_circle(url)
            elif loc_type.lower() == 'coordination':
                url = earthquake_Coordinates(url)
            else:
                print(Fore.RED+'No input for location')
        elif query.lower() == 'both':
            url = earthquake_parameter(url)
            loc_type= input('Do you want to query by country, circle or coordination?')
            if loc_type.lower() == 'country':
                url = earthquake_country(url)
            elif loc_type.lower() == 'circle':
                url =  earthquake_country_circle(url)
            elif loc_type.lower() == 'coordination':
                url = earthquake_Coordinates(url)
            else:
                print('No input for location')
        else:
            print(Fore.RED +('Unknow input, you may need try again if the url is not working'))
    if url == 'https://www.strongmotioncenter.org/wserv/events/query?&orderby=time&format=csv&nodata=404':
        print(Fore.RED +'You must enter at least one parameter! Try Again.')
    
    return url 
```
