Introduction
------------
Scraphp (say Scraph, last p is silent) is a web crawling program. It is basically built to be a standalone executable which can crawl websites and store extract useful content out of it. I created this script for a challenge posted by Indix on Jan 2012, where in I was asked to crawl AGMarket (http://agmarknet.nic.in/) to get the prices of all the products, and store their prices. I also had to version the prices such that it should persist across dates. 

Scraph was inspired from a similar project called Scrappy, written in Python. This is not an attempt to port it, but just wanted to see how much similar properties can I build from it in less than a day. 

One of the major features I would like to call it is, When you crawl the page you can extract entites out of it based on XPath. So basically when we crawl a page I create a bean whose properties are set of values got by applying the given XPath on the page. Each XPath is completely independent of the other. Currently Scraph supports creating only 1 type of object per page. 

Hack into the source code, its well commented and easy to modify as per requirement. All the details of the crawling page, XPath queries are all provided in the configuration.php or you can supply your own config file, see the Usage below. 

Directory Layout
-----------------

`./data/`		-	Contains the log and default scrapper.db SQLite databases

`./lib/`		-	Contains the librarys that were used in the application

`./logs/`		-	Contains the logs that are generated by the application. 

`./model/`		-	Contains the models (Beans) and interfaces used in the application


Usage
-----

Default configuration implemenation crawls the AGMarket (initial question) and adds the content to the datastore

`$ ./scraphp`

I have created a sample config for crawling Flipkart also which can be executed as

`$ ./scraphp --configuration=config.flipkart.sample.php`

This crawls all the products with their name, type (major classifications like books, computers, etc.) and price. 

Configuration Options
--------------------

Configuration layout is well documented in the samples provided.

General Method to use
---------------------

1.	Copy any of the sample existing configuration and update the values of _bean and XPath
2.	Also create a class which implements the Scrapable interface and implement the method save();
3.	All the `props` that were defined in the configuration are available as properties in the object. <br />
	If the prop item is not found, it will not be available, always use isset($this->item) to check 
	if the item was crawled from the page.
4.	For datastore we are using RedBeanPHP (http://www.redbeanphp.com/manual/), for its simplicity in 
	design. A 15 min read of the documentation can get you started in RedBean right away :-)
5.	Once the configuration and the corresponding beans are created, you can just invoke the spider as 
	the normal	executable. 
	
Special Mention
---------------

*	When we crawl any page we use Tidy to clean the document and then load its DOM. So normal Google Chrome's or Firebug's XPath might not work on the way. Use YQL to test the XPath you are using (YQL also tidies the document). Sample YQL query might be - http://goo.gl/3NDlm

Depencies
---------

Scrapper depends on 

* php5-tidy, 
* php5-mysql, 
* php5-sqlite, 
* php5-cli 

Webserver configured with PHP5 as CGI or natively as a module (like Apache) to use the Web interface for querying the loaded data. 
PHP5 packages.

