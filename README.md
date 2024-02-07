![Web Scrapping](https://www.antevenio.com/wp-content/uploads/2019/03/web.jpeg)

# Company data extraction project.
![Static Badge](https://img.shields.io/badge/Python-yellow?logo=python) | ![Static Badge](https://img.shields.io/badge/Pandas-blue?logo=pandas) | ![Static Badge](https://img.shields.io/badge/Scrapy-blue?logo=scrapy)


##### This project is intended only for educational and demonstrative purposes, it is not intended to be used for commercial purposes.

In this project we will observe how using the Scrapy tool we can extract a large amount of information from a web to obtain relevant data from it, such as; Company name, company id, phone number, city and state where it is located, obtaining this data can help commercial departments to optimize the result of a search since data extraction is much faster than what an average person would take to copy and paste the data into an Excel sheet.

For this project we will mainly use two Python libraries: Pandas, which will allow us to model and clean the data obtained and Scrapy, which will allow us to access the web and extract the data that we require.


## What will we need?

First of all, we must make sure that we have the libraries that we need, for this it is necessary to install _Pandas_ and _Scrapy_

```
pip install pandas
pip install scrapy
```

Once we have our libraries installed, we must load the Scrapy packages that we will use, for this extraction we will use:

```
from scrapy.downloadermiddlewares.useragent import UserAgentMiddleware
from scrapy.item import Field, Item
from scrapy.spiders import Spider, CrawlSpider, Rule
from scrapy.selector import Selector
from scrapy.loader import ItemLoader
from scrapy.loader.processors import MapCompose
from scrapy.crawler import CrawlerProcess
from scrapy.linkextractors import LinkExtractor
from scrapy import signals
```

we must also perform the correct import of Pandas:

```
import pandas as pd
```

Also, it is important that we import the time and random libraries, because when we use our user-agent we will need them:

```
import time
import random
```

## How does it work?

Our project relies on three main classes, which are:

```
class CultivosDeFlores(Item):
```

This class will be in charge of defining the fields that we will query, here we will define what we are interested in searching/obtaining from our query.

```
class CultivosFlores(CrawlSpider)
```

In this class we will define our user agents that will allow us to have an "identity" when making our queries, we will pass as if the requirements were being done by a human being and not, our script.

The user-agents that I use are:

```
'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36',
'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:31.0) Gecko/20100101 Firefox/31.0',
'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.125 Safari/537.36',
```

within our class **_CultivosFlores_** we define a function _parse_empresa_ that allows us, through the **xpath** of the id of each element, to locate its position and thus be able to extract only the information that we need.

Our other class:

```
class RotateUserAgentMiddleware(UserAgentMiddleware):
```

It allows us to switch between our user-agents randomly so that our traffic looks more real.

Finally, we generate a **_crawler process_** in order to obtain our data in a document with **.csv** extension that we later manipulate in Pandas.

```
process = CrawlerProcess({
    'FEEDS': {
        'Cultivo_De_Flores.csv': {'format': 'csv'},
    },
})
```

It is worth noting that the format can be modified to the user's liking if they prefer it in another format, such as **json** it would only be to replace _csv_ by _json_ `'Cultivo_De_Flores.csv':{'format': 'csv'}` = `'Cultivo_De_Flores.json':{'format': 'json'}`

## How long does the extraction take?

Depending on the amount of information that we want to extract, in this extraction I managed to recover around 5600 records, that is why we will rely on Pandas to manipulate the information and keep only the amount of data that we will actually use.