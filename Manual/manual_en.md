# Installation #

## System requirements ##

For OXID and OXSEARCH to run smoothly together, make sure the following components are installed:  
- PHP 5.3+  
- OXID 4.8+ or higher  
- Php-curl: This is a requirement for OXID and therefore should already be installed..  
- 	- Elasticsearch 1.1+ oder 1.4  
- optional Elasticsearch-Plugins for better Unicode search  
- elasticsearch-analysis-icu  
- elasticsearch-analysis-combo  

Notes:  
- Elasticsearch 1.2 requires an explicite activation of dynamic scripting. To achieve that, please add the line die  
	script.disable_dynamic: false	
to the file elasticsearch.yml.  
- In ElasticSearch 1.4, the scripting language mvel is no longer existant.  
To maintain ful functionality of OXSEARCH, it is therefore necessary to install the plugin elasticsearch-lang-mvel.  
- After installing the plugin, mvel must be defined as the standard language in elasticsearch.yml.  
	script.default_lang: "mvel"	

## Installation ##

1. Download the zip file of OXSEARCH.  
2. Install its contents to the path <ShopRoot>/modules/marm/oxsearch.  
3. Please make sure that the file <ShopRoot>/modules/marm/vendormetadata.php exists. If that's not the file, please create an empty file by that name.  
4. Now you can activate the extension in the OXID backend in Extensions > Modules > marmalade:: OXSEARCH.  
![Aktivierung von OXSEARCH](img/oxid_oxsearch_aktivieren.png)  
Clicking extensions on the left hand side, a new entry OXSEARCH-Setup now appears. Here, you can comfortably manage OXSEARCH.

# OXSEARCH-Setup #

For OXSEARCH to run smoothly with OXID, some crucial settings have to be applied first. The setup is devided into four Tabs:  

- In OXSEARCH-Setup you can make general settings, search options and product filter.  
- In boosting and seakable links, you can configure Boosting and Synonyms.  
- In elasticSearch configuration, you enter the access data vor the elasticSearch server and start the indexing process for your shop.  
- In seakrequest without result, you find a list of key words without a result.
![Moduleinstellungen für OXID](img/oxsearch_setup.png)  

## ElasticSearch connection information and indexing ##

Before you can use OXSEARCH, it is mandatory to enter the information for elasticSearch.  

1. Select the tab Elasticsearch configuration > connection details.  
2. Enter the host in the formate of [USER:PASSWORd@]ES-DOMAIN_OR_IP.  
3. Specify the port which processes search requests. In most cases, it is 9200.  
4. Save your settings before you continue with the next step.  
5. Reopen the elasticSearch configuration settings.  
6. Define a distinct name for the active and inactive index.  
Note: the names vor both indexes can be the same. It is important, however, for them to be in lower case because of an Invalid JSON error message that accurs otherwise.__  
On the active index, search requests in the shop frontend are performed. The inactive index is only relevant for the indexing process itself. Clicking the swap button will swap both indexes.  
![Verbindungsdaten und Indexerstellung](img/oxsearch_elastic_search_config.png)
__Note: To avoid tempering with the shop from the outside, we strongly recommend that elasticSearch requires at least a basic authorization!___

## Data import ##

You can initiate the data import by clicking on rebuild index now in the elasticSearch configuration. In your live shop, we recommend to assign this task to a cronjob. Just configure the cronjob to execute the file <ShopRoot>/modules/marm/oxsearch/importer/doImport.sh.

### Update- & Delete-API ###

The marmOxsearchImport class comes with a few methods for updating and deleting articles:  
- `updateArticle($articleId, $language = 0, $index = 'active')` updates a specific article.  
- `updateArticles($articleIds, $language = 0, $index = 'active')` is the equivalent to an article list.  
- `deleteArticle($articleId, $index = 'active')` deletes an article.  
- `deleteArticles($articleIds, $index = 'active')` deletes several articles.  
The optional Parameter $language determins the language using the language ID of OXID, $index specifies weather you work on the active or inactive index.  
Use case::
	$sOxid = oxRegistry::getConfig()->getRequestParameter('oxid');
oxRegistry::get('marmOxsearchImport')->updateArticle($sOxid);	  
__Note:  
Please use the list methods with care as they have no limits. A huge amount of articles can result in memory or runtime issues.__  

## General configuration ##

The general configuration dialogue is divided into three sections:  
- general settings for crucial settings  
- Search options to define search criteria  
- product filter  

### General configuration ###

![OXSEARCH-Setup](img/oxsearch_setup.png)
- Activation of OXSEARCH  
- List view: Here you decide if the search is based on the OXID settings or if you apply OXSEARCH's product filters. The OXSEARCH product filters aply only when the checkbox is checked.  
__Note: This option requires a new indexing!__  
- Autosuggestion offers suggestion while entering a search request  
- Add page number to the URL of article: Beginning with page 2, the page number is appended to the URL of a more than one pages long result list.  
- Active promotions overwrite category sorting defined in oxid: This setting affects whether the in OXID defined sorting is overwritten by promotions. Promotions influence the positioning of articles.  
- Parent categories automatically contain all products of subcategories: If your shop contains a category computer and accessories subdivided into computer and accessories, the check will display all articles of all subcategories whereas with the unchecked box, the customer will have to chose one subcategory first.  
__Note: This option requires a new indexing!__  
- Automatically rebuild index after the saving of each article: If your index is regularly rebuilt by a cronjob, this option is unnecessary.  
- Disable OXLocator for detail pages of search results: The OXLocator adds navigation links to product pages of a search result list to facilitate navigation (prior/previous article, etc.).  
___Note: OXLocator has negative effects on the performance of your shop system.___
- Divider for attributes with multiplevalues: For example, if an article consists of more than one material, the attribute material consists of two values. The divider enables both values to be saved.  

### Search options ###

In the search options, you can define search filters which influence your search results.  
- Assessed search fields: This parameter defines the quantifier for searches. If you assessed the short description with 3 and the article name with 10, the article name would be priorized over the short description.  
- Search in attribute values: Attributes are propperties of articles like colour, size, etc. The enabled checkbox would enable a user to search your shop for all blue t-shirts.  
- Search in variants  
- Additional fuzzy search: The fuzzy search corrects minor mistakes like typing errors.  
- Wildcard search: enables and disables the usage of wild cards in search requests.  
___Note that this option has negative effects on the performance of your shop!___  
- Don't rediret to article for single item results: Checking this box will display a search result list for a single hit instead of redirecting to the article.  
- Show suggestions: Similar search results are displayed in the result list.  
- OXSEARCH-Keys: Here you define which search fields are displayed (article name, short description, long description, etc.).  
- Show found categories: A check will result in displaying kategories with the found results.  
- Show manufacturer results displays all manufacturers for the requested article  
- Show found links enables the customer to search for information in your shop other than articles like payment, or terms of service.  
- Cutoff-Häufigkeit: This option excludes defined irrelevant search terms like the, of, ...  
- Price range for search results  
- Querry operator: In this sellect box you decide wether all or at least one criteria.  

### Product filter ### 

In this section, you configure product filters.  
- Use SEO-uRLS for filtered pages: SEO-URLS: We recommend using SEO-URLs as search engines include them into the search results. Besides, they can be remembered more easily by customers.  
- Display count of docs for filter value: If you set this option, the user can see the number of results for his request.  
- Don't move selected filter values to front: This option specifies if the filter sorting still remains when the result list has more than one page.  
- Activate category filter enables customers to select the categories which he wants to search in.  
- Divider for dynamic categories filters: Dynamic categories are populated by elasticSearch and consist of combinations of already existing categories. To differenciate dynamic categories from static ones, the divider must be different from the divider configured in the search options. An example for a dynamic category would be to display all articles in stock by a specified manufacturer.  
- Attributes: You can define attributes for articles, colour and size being the obvious choices for clothing. If you define attributes for clothing, a shirt in multiple colours and sizes will be saved as one article with the colours and sizes as dropdown menues to select. Without defined attributes, every colour and size variation will have to be saved as a separate article.  
- Script: This option enables you to write scripts which check certain values to define specials.  

## Synonyms and searchable links ##

This tab is divided into three sections and covers:  
- dynamic Boosting  
- Seakable Links  
- Synonyms  

### Dynamic Boosting ###

The dynamic boosting manipulates the position of articles in category views as well as search results. This is done by dynamically changing values such as sales values or visits to article pages. You can define the factors for the weight of statistics.  
![Dynamisches Boosting](img/oxsearch_dynamisches_boosting.png)  
- Enable dynamisc Boosting: You have to check dynamic boosting in order to benefit from the below settings.  
- Use for categories: If you enable this option, the category sorting doesn't aplly anymore.  
- Boostfactor Value of sales  
- Boostfactor Added to card  
- Boostfactor Number of clicks  
- Boostfactor Promotions  
- Relevance exponent: defines the factor with which the quantifiers are potentiated.  
- Enable debug mode: When the administrator logs into the shop frontend, he can see the result of the boosting settings.  

### Seakable links ###

This section defines which links asside from shop articles are displayed in the search results. Again, the most common examples are payment or terms of service..   
![Suchbare Links](img/suchbare_links.png)  
The following information is required for each link:  
- Title  
- external  
- page  

### Synonyms ###

This section serves for maintaining synonyms.  
![Synonyme](img/oxsearch_synonyme.png)  
If "candy bar" appears in the statistic for searkrequest without results, you can define "chocolate bar" as a synonym.  
___Please note that the index requires a new rebuilt!___  

### Searchrequest without results ###

This section serves a statistical purpose. We recommend to extend your synonyms based on the evaluation of this page.  
![Suchanfragen ohne Ergebnis](img/oxsearch_suchanfragen_ohne_ergebnis.png) 
Bei der Auswertung bietet es sich an, Synonyme zu pflegen, die mit den ergebnislosen Suchanfragen verwandt sind.  

# Notes # 
## LandingPages and Promotions ## 

OXID's  actions interface can be used to set up landingpages and promotions and be supplied with dynamic filters.  

## Categories ## 

Categories can be populated dynamically with articles. In the tab __dynamic article selection__ in the OXID settings under administer products > categories, you can assign values to the filters defined in OXSEARCH. It is not even necessary for the filter to be active, it just needs a unique parameter name. To decide whichfilters aplly for which category, select the visible filters tab.