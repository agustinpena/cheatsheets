# Notes on Scrapy

To start a scrapy project, type in terminal:
```sh
> scrapy startproject <project_name>
```
If our project name is *ebook_scraper*, a folder with that name will be created. Inside there will be a folder with the same name, and inside that folder there's the *Spiders* folder. CD to *Spiders*.

### *Creating our first spider*

In the *Spiders* folder, create a python file, say, *ebook.py*. This is our first spider. It has to inherint all properties from the *Spider* scrapy class:
```py
from scrapy import Spider

class EbookSpider(Spider):
    # spider name:
    name = 'ebook'
    # urls to scrape:
    start_urls = ['https://ebooks.toscrape.com']

    # parsing method:
    def parse(self, response):
        print('*** RESPONSE ***')
        print(response)
```
This simple spider will parse the web site and print the result only. There is no specific selection of data. To scrape the web site with our spider, cd to the project main directory and type:
```sh
> scrapy crawl ebook
```
where *ebook* is our spider's name, as defined in its variable *name*.


### *Scrapy CSS Selectors*
Now our spider will select data from the web site by using CSS selectors. Look at the *parse* method inside the spider:
```py
from scrapy import Spider

class EbookSpider(Spider):
    name = 'ebook'
    start_urls = ['https://ebooks.toscrape.com']

    def parse(self, response):
        print('*** PARSE ****')
        data = response.css('h3 a::text').get()
        print(data)
```
*h3 a* - retrieves only the content of an 'a' tag inside a 'h3' tag.

*::text* - indicates only the text inside the selectors is to be retrieved.

get() - retrieves the indicated data.

A more complicated spider:
```py
from scrapy import Spider

class EbookSpider(Spider):
    name = 'ebook'
    start_urls = ['https://ebooks.toscrape.com']

    def parse(self, response):
        print('*** PARSE ***')

        # gets data contained inside
        # the 'article' selector:
        ebooks = response.css('article')

        # gets info for each ebook:
        for ebook in ebooks:
            title = ebook.css('a::text').get()
            price = ebook.css('p.price_color::text').get()

            yield {
                'title': title,
                'price': price
            }
```
To output the result to a json file, type:
```sh
> scrapy crawl ebook -o ebook.json
```
This will create an *ebook.json* file with the data we need.
