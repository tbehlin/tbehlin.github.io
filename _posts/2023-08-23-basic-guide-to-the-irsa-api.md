---
layout: post
title: Basic Guide to the IRSA API
categories:
- Blog Posts
tags:
- python
- pandas
- astronomy
date: 2023-08-23 16:39 -0400
---
    As part of my work with YSOLab, I needed to find some way of streamlining the previously manual process of acquiring data for specific objects, given a Right Ascension and Declination. I spent some time researching, and came across the process described in this blog post, which I have refined somewhat over the past 6 months.

    This blog post focuses on providing a basic primer on how to use the IRSA API to automatically download telescope data for a given position from within python, as well as how to avoid some small issues that bedeviled my implementation for some time.

## What is IRSA?

IRSA is the name of the NASA/IPAC Infrared Science Archive, which, as the name suggests, is a repository for infrared data from a variety of sources, including the WISE space telescope, which I will focus on in this article.

    Navigating to a specific telescope, you can see that each one has a number of different catalogs of data. Knowing which catalog you are specifically interested in will be an important detail later on in the process.



## What is an API?

    The acronym API stands for Application Programming Interface. API's are primarily employed to allow multiple disconnected applications to communicate with each other, and share information automatically between different programs. In our case, the IRSA API will allow us to automatically download data from a specific telescope and catalog, given a few identifying parameters. The ability to leverage these APIs is an important skill to have, as it can help to eliminate a large amount of manual work that would slow down your processes, and introduce human error.



## Working with the IRSA API

#### Setup

    In order to integrate the IRSA API with a python script, you will need to install a specific package called [PyVO](https://pyvo.readthedocs.io/en/latest/). This package is what allows your computer to access the IRSA database in the first place, but is widely used by other databases, and is an important tool to have lying around.



#### Finding the Catalog Identifier

    This is when knowing what catalog you will be working with becomes important. In order to use the pyvo commands to download the data, you will need to specify to the IRSA database what specific catalog you want to retrieve data from. For IRSA, the available catalogs can be found from [this page](https://irsa.ipac.caltech.edu/onlinehelp/catalogs/#api), in the adjustable keywords section. The identifier we will be using for this example is neowiser_p1bs_psd, which corresponds to the data from the NEOWISE-R Single Exposure (L1b) Source Table.



#### Search Style

    Now that we can identify the specific table, we will need to know what kind of search we will be performing. There are multiple different kinds of search available within the PyVO package, however we will be focusing on the simplest form, the Simple Cone Search (SCS). This search type takes in a Right Ascension, Declination, and Radius, and performs a search to find any measurements taken within the resulting circle on the sky.

    Someone with more database experience might instead opt for the Table Access Protocol (TAP) type search, which allows for more sql-ish queries.



#### Implementation in Python

    Shown below is a simple implementation of the Simple Cone Search using the PyVO package. This snippet shows both the retrieval and storage of data for a specific position and radius.

```python
import numpy as np #used for handling recieved data
import pandas as pd #used for handling recieved data

import pyvo #the necessary package to perform the search

from astropy.table import Table #recieved data is in this format

#This is the API URL for the NEOWISE-R Single Exposure (L1b) Source Table
#  this is constructed using the catalog identifier from earlier
#  the portion following 'table=' is the identifier for the table
url = ("https://irsa.ipac.caltech.edu/SCS?table=neowiser_p1bs_psd")

#the necessary parameters (in degrees)
object_ra  = 84.9854
object_dec = -7.431
search_radius = (10) / 3600.0 #10 arcseconds in degrees

#This command performs the resulting search
#  This can take around a minute or longer
#  Depends upon the quality of your internet connection
#  Returns an astropy Table object
astrotable = pyvo.conesearch(url, 
                             pos=(object_ra, object_dec), 
                             radius = search_radius)

#Converts the Table to a more usable pandas DataFrame.
df = astrotable.to_table().to_pandas()
```

## Further Recommendations

There are a few small additional things I would recommend you keep in mind when you are implementing these methods.

#### Prevent Repeated Downloading

    Firstly, I would recommend saving the data you retrieve to your computer automatically (a .csv file or .pkl file would work, but format doesn't really matter here). This can help save time by reducing the need to repeatedly query the same data from the API multiple times. In general, any operation that involves downloading data through the API will necessarily take longer than loading a saved file from your disk. 

    For example, in my current implementation, I save the downloaded dataframe as a .pkl file, with the name containing the ra, dec, and search radius used within the search. Before I perform a search, I first check for the existence of a file that contains the same data I would recieve from this search. This simple process saves a significant amount of time over multiple runs or larger datasets. 

    One caveat is that you do need to check your catalog's site every once in a while for new releases, as any new data introduced to the catalog will not be downloaded unless you delete the existing data and allow it to be re-downloaded.



#### Watch for Rate-Limiting

    One detail to watch out for is the rate-limiting included within the API. From some rough experimentation, the API will prevent you from submitting one search more than once every 30-45 seconds. If you attempt to do so, the second search will return an error.

    In general, even without this rate-limiting, it is best practice to limit your query rate to an external database when you are making multiple searches in rapid succession. This is to avoid putting undue strain upon that database, which might struggle to fulfill a flurry of large requests.

Implementing this rate-limiting is best done using a combination of a timing package such as datetime, which would be used to avoid sending requests too quickly, and by implementing a try-except block around the PyVO command in tandem with sleep commands, to catch rate-limited requests and pause for a moment before retrying.
