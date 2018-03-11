# Web-Crawler

A micro-framework to crawl the web pages. You can literally define what sites you 
want to crawl through and even configure the type of data you want to crawl and gather.
The scripts can cache as well as dave the data into database. 

Currently Supported databases:

- Elasticsearch
- MongoDB

## Install

```bash

pip install git+https://github.com/invanatech/web-crawler#egg=webcrawler

```

## Usage example with MongoDB as database

```python

from webcrawler.parser import crawler
import json

example_config = json.load(open('../example.json'))

common_settings = {
    'COMPRESSION_ENABLED': False,
    'HTTPCACHE_ENABLED': True,
    'INVANA_CRAWLER_COLLECTION': "weblinks",
    'INVANA_CRAWLER_EXTRACTION_COLLECTION': "weblinks_extracted_data",
    'LOG_LEVEL': 'INFO'
}

mongodb_settings = {
    'PIPELINE_MONGODB_DATABASE': "crawler_data",
    'ITEM_PIPELINES': {'webcrawler.pipelines.mongodb.MongoDBPipeline': 1},

    'HTTPCACHE_STORAGE': "webcrawler.httpcache.mongodb.MongoDBCacheStorage",
    'HTTPCACHE_MONGODB_DATABASE': "crawler_data",
    "HTTPCACHE_MONGODB_PORT": 27017,
}

common_settings.update(mongodb_settings)

if __name__ == '__main__':
    crawler(config=example_config, settings=common_settings)

```

## Usage example with Elasticsearch as database

```python

from webcrawler.parser import crawler
import json

example_config = json.load(open('../example.json'))
common_settings = {
    'COMPRESSION_ENABLED': False,
    'HTTPCACHE_ENABLED': True,
    'INVANA_CRAWLER_COLLECTION': "weblinks",
    'INVANA_CRAWLER_EXTRACTION_COLLECTION': "weblinks_extracted_data",
    'LOG_LEVEL': 'INFO'
}

es_settings = {
    'ITEM_PIPELINES': {'webcrawler.pipelines.elasticsearch.ElasticsearchPipeline': 1},

    'HTTPCACHE_STORAGE': "webcrawler.httpcache.elasticsearch.ESCacheStorage",
}

common_settings.update(es_settings)
print(common_settings)

if __name__ == '__main__':
    crawler(config=example_config, settings=common_settings)

```


### Using MongoDB as http cache storage


```python


settings = {
    'HTTPCACHE_ENABLED': True,
    'HTTPCACHE_STORAGE': "webcrawler.httpcache.mongodb.MongoDBCacheStorage",
    'HTTPCACHE_MONGODB_DATABASE': "crawlers",
    "HTTPCACHE_MONGODB_PORT": 27017,
    'COMPRESSION_ENABLED': False,
}


```

### Using MongoDB to save extracted data

```python


settings = {
    'HTTPCACHE_STORAGE': "webcrawler.pipelines.mongodb.MongoDBPipeline",
}

```


### Using Elasticsearch as http cache storage

```python


settings = {
    'HTTPCACHE_ENABLED': True,
    'HTTPCACHE_STORAGE': "webcrawler.httpcache.elasticsearch.ESCacheStorage",
    'COMPRESSION_ENABLED': False,
}

```

### Using Elasticsearch to save extracted data

```python


settings = {
    'HTTPCACHE_STORAGE': "webcrawler.pipelines.elasticsearch.ElasticsearchPipeline",
}

```

**Note:** By default, both ES and MongoDB uses `crawlers_data` as database and `weblinks` collection/doctype
to save the webpage data and `weblinks_extracted_data` as collection/doctype to save the extracted data.

All the entries in ES as url as id.


### Saving data to json file
```python


from webcrawler.utils import example_config
from webcrawler.parser import crawler
 

settings = {
    'FEED_URI': 'result.json',
    'ITEM_PIPELINES': {'__main__.MongoDBPipeline': 1},

}

if __name__ == '__main__':
    crawler(config=example_config, settings=settings)


```




## References

- https://stackoverflow.com/a/14453186/3448851


