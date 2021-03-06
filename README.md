# DynamicSpider
## Description

A dynamic spider by scrapy. 

## Usage

1. Create a mongodb database named ``spider`` ,create a collection named ``rules`` .
2. Create collections named ``info`` , ``history_info`` .
3. Get scrapy-splash support: see [scrapy-splash](https://github.com/scrapy-plugins/scrapy-splash) .



The rule format are just like this:  

*(sure, you do not need to known much about the format. I will make an editor later.)*

```json
{
  "_id": 8, 
  "name": "创世中文网", 
  "start_url": [
    "http://chuangshi.qq.com/bk/zp1gx3/"
  ],
  "next_selector": ".nextBtn"
  "next_page": "//a[@class=\"nextBtn\"]/@href", 
  "max_page": 50,
  "body_list": "//div[@class=\"leftlist\"]/table/tr[not(@class)]", 
  "meta": {
    "source": 8
  },
  "item_name": [
    "title",
    "author",
    "url",
    "source",
    "bookid",
    "id"
  ],
  "xpath": {
    "title_xpath": {
      "selector": "./td[3]/a[1]/text()",
      "role": "normal"
    },
    "author_xpath": {
      "selector": "./td[5]/a/text()",
      "role": "normal"
    },
    "url_xpath": {
      "selector": "./td[3]/a[1]/@href",
      "role": "url"
    }
  },
  "re": {
    "bookid": {
      "reg": "\\d+",
      "str_name": "url"
    }
  },
  "join": {
    "id": {
      "prefix": "cs",
      "join_str": "_",
      "end_name": "bookid"
    }
  }
}
```

run ``python run.py``.

## Updated

1. **#2017-04-08#:** support basic dynamic crawl for list and next page.
2. **#2017-04-10#:** update rule format and note; add pipelines; add re, join in parse. Now you can get data from Mongodb!
3. **#2017-04-13#:** update rule; add javascript support; add max next page support; add extract roles.

## Rule Format

| No   | Rule Field     | Descript                                 |
| :--- | :------------- | :--------------------------------------- |
| 1    | _id            | rule id                                  |
| 2    | name           | rule name                                |
| 3    | start_url      | the same as scrapy.Spider                |
| 4    | next_page      | next page xpath                          |
| 5    | body_list      | data(type: list) xpath                   |
| 6    | meta           | basic info type in directly              |
| 7    | item_name      | item names in scrapy.Item, #source# in #meta#, #bookid# in #re#, #id# in #join# |
| 8    | xpath          | xpaths to parse, xpath prefix must in #item_name# |
| 9    | re             | regulation to extract message from scrapy.Item, #reg# for compile, #str_name# for string in scrapy.Item |
| 10   | join           | join string, #prefix# for #id# string prefix, #join_str# for joint mark, #end_name# for string at the end of #id#, #id# value must in scrapy.Item |
| 11   | next_selector  | jquery selector. use by javascript spider to go to the next page. |
| 12   | max_page       | max crawl pages.                         |
| 13   | xpath:selector | --                                       |
| 14   | xpath:role     | normal, url,...                          |
