## OXSEARCH-Setup ##

For OXSEARCH to run smoothly with OXID, some crucial settings have to be applied first. The setup is devided into four Tabs:

- In OXSEARCH-Setup you can make general settings, search options and product filter.
- In boosting and searchable links, you can configure Boosting and Synonyms.
- In elasticSearch configuration, you enter the access data vor the elasticSearch server and start the indexing process for your shop.
- In seakrequest without result, you find a list of key words without a result.

![OXSEARCH setup](img/oxsearch_setup.png)

### ElasticSearch connection information  and indexing ###

Before you can use OXSEARCH, it is mandatory to enter the information for elasticSearch.

1. Select the tab Elasticsearch configuration > connection details.
2. Enter the host in the formate of [USER:PASSWORd@]ES-DOMAIN_OR_IP.
3. Specify the port which processes search requests. In most cases, it is 9200.
4. Save your settings before you continue with the next step.
5. Reopen the elasticSearch configuration settings.
6. Define a distinct name for the active and inactive index.
__Note: the names vor both indexes can be the same. It is important, however, for them to be in lower case because of an Invalid JSON error message that accurs otherwise. Also, both the active and passive index must be defined as otherwise, the active index will not be written into.__
On the active index, search requests in the shop frontend are performed. The inactive index is only relevant for the indexing process itself. After both indexes are defined and the active index is filled with contents, please make sure to swap both indexes with the corresponding button.
__Note: To avoid tempering with the shop from the outside, we strongly recommend that elasticSearch requires at least a basic authorization!__

![Connection data and indexing](img/oxsearch_elastic_search_config.png)

### Data import ###

There are three ways to initiate a data import:

- by clicking on "rebuild index now" in the elasticSearch configuration
- assigning this task to a cronjob which executes the script file <ShopRoot>/modules/marm/oxsearch/importer/doImport.sh.
- with the help of the update and delete API
Using the "rebuild index now" button in the back end, all changes take effect immediately. Depending on the data in the shop, however, the indexing process can take some time. Also, you cannot leave the page during the process.
Like the rebuilding in the elasticSearch configuration window, the cronjob is a ful data import. Therefore, it also can take some time to finish. In contrast to using the rebuild button, it's not necessary to log into your shop and initiate the process manually.
For a more specific data import, we recommend the usage of the update and delete API.

### Data export ###

You are able to export your elasticSearch index to a remote server. It is useful if you have some troubles with your elasticSearch configuration and you want to send a copy of an index to your agency. In the input field you have to write the full path of the index. For example: `http://elastic.example.com:9200/index_name`. If there is an authorization required to access the index, please write it in the following format: `http://user:password@elastic.example.com:9200/index_name`.
__Note: An index has to be created before the export.__

#### Update- & Delete-API ####

In certain situations a ful import is not the best solution. If For example, if you want to update your index with every occuring change in your stock, a ful import can resolute in unnecessary high traffic. To adjust the indexing process to your individual needs and tie the indexing to certain conditions, for example when an article in stock reaches a value of 10, the marmOxsearchImport class comes with a few methods for updating and deleting articles:

- `updateArticle($articleId, $language = 0, $index = 'active')` updates a specific article.
- `updateArticles($articleIds, $language = 0, $index = 'active')` is the equivalent to an article list.
- `deleteArticle($articleId, $index = 'active')` deletes an article.
- `deleteArticles($articleIds, $index = 'active')` deletes several articles.
The optional Parameter $language determins the language using the language ID of OXID, $index specifies weather you work on the active or inactive index.
Use case:
		$sOxid = oxRegistry::getConfig()->getRequestParameter('oxid');
		oxRegistry::get('marmOxsearchImport')->updateArticle($sOxid);
__Note:
Please use the list methods with care as they have no limits. A huge amount of articles can result in memory or runtime issues.__

### General configuration ###

The general configuration dialogue is divided into three sections:

- general settings for crucial settings
- Search options to define search criteria
- product filter

#### General configuration ####

![OXSEARCH-Setup](img/oxsearch_setup.png)

- Activation of OXSEARCH
- List view: This setting applies either OXID standards or OXSEARCH filters to the category lists. Only checking this box will apply your configured filters.
- Search: Here you decide if the search is based on the OXID settings or if you apply OXSEARCH's product filters. The OXSEARCH product filters aply only when the checkbox is checked.
__Note: This option requires a new indexing!__
- Autosuggestion offers suggestion while entering a search request
- Add page number to the URL of article: Beginning with page 2, the page number is appended to the URL of a more than one pages long result list.
- Active promotions overwrite category sorting defined in oxid: This setting affects whether the in OXID defined sorting is overwritten by promotions. Promotions influence the positioning of articles. In OXID, you can maintain promotions in "customer info > promotions.
- Parent categories automatically contain all products of subcategories: If your shop contains a category computer and accessories subdivided into computer and accessories, the check will display all articles of all subcategories whereas with the unchecked box, the customer will have to chose one subcategory first.
__Note: This option requires a new indexing!__
- Automatically rebuild index after the saving of each article: If your index is regularly rebuilt by a cronjob, this option is unnecessary.
__Note: Articles deleted from the shop will also be deleted in Elasticsearch when this option is activated. When it's deactivated, a new indexing is required to do so!__
- Disable OXLocator for detail pages of search results: The OXLocator adds navigation links to product pages of a search result list to facilitate navigation (prior/previous article, etc.).
___Note: OXLocator has negative effects on the performance of your shop system.___
- Divider for attributes with multiplevalues: For example, if an article consists of more than one material, the attribute material consists of two values. The divider enables both values to be saved.

#### Search options ####

In the search options, you can define search filters which influence your search results.

- Assessed search fields: This parameter defines the quantifier for searches. If you assessed the short description with 3 and the article name with 10, the article name would be priorized over the short description.
Note that if you want your customers to find the product by it's exact product number, add a search attribute _OXARTNUM_ (with multiplier 10) and _OXARTNUM.unanalyzed_ (with multiplier 1000).
- Search in attribute values: Attributes are propperties of articles like colour or size. The enabled checkbox would enable a user to search your shop for all blue t-shirts.
- Search in variants: Variants of an article inherit from a parent article. The parent article contains basic information and the variants different colours or sizes. In the shop frontend, the customer clicks on one article and can select the preferred variant within the article details.
- Additional fuzzy search: The fuzzy search corrects minor mistakes like typing errors if at least 30 % of the search term is spelled correctly.
- Wildcard search: enables and disables the usage of wild cards in search requests. Wildcards can be helpful if the article name consists of more than two compound words. In those cases, not using wild card search might not display all appropriate hits in the result list.
___Note that this option has negative effects on the performance of your shop!___
- Don't redirect to article for single item results: Checking this box will display a search result list for a single hit instead of redirecting to the article.
- Show suggestions: Auto suggestion completes the entered search terms.
- OXSEARCH-Keys: Here you define which search fields are displayed (article name, short description, long description, etc.).
- Show found categories: Checking this option will result in the listing of categories next to the search results in the frontend..
- Show manufacturer results displays all manufacturers for the requested article
- Show found links enables the customer to search for information in your shop other than articles like payment, or terms of service.
- Cutoff frequency: This option assigns a minor relevance to unimportant words like "the" or "of" and packs them into a subquerry to display exact search results.
- Price range for search results: Narrows the results to the desired price range. This can be of great help when the search term is quite general (kiteboard).
- Querry operator: In this sellect box you decide wether all or at least one criteria.

#### Product filter ### #

In this section, you configure product filters.

- Use SEO-URLS for filtered pages: If you uncheck this field, all selected filters will be appended as parameters to the URL of the page. Depending on the length of the URL, this can cause problems up to the point where you can't open the desired page.
___Note: If SEO-URLS are checked, the underscore cannot be defined as dividers in third party modules!___
- Display count of docs for filter value: If you set this option, the user can see the number of results for his request.
- Don't move selected filter values to front: This option specifies if the original filter sorting remains when filters are selected in a multiselect box. If this option remains unchecked, you will see the selected filters at the top of the select box.
- Activate category filter: If this option is checked, the result list displays the categories that contain results for the particular search.
__Note: This filter only works correctly if _Parent categories automatically contain all products of subcategories_ is activated as well!__
- Divider for dynamic categories: Dynamic categories are populated by elasticSearch. The exemplary category "gift items under 100 EUR" could be populated with all articles for less then 100 EUR. To differenciate dynamic categories from static ones, the divider must be different from the divider configured in the search options.
- Attributes: You can define attributes for articles, colour and size being the obvious choices for clothing. OXSEARCH can then filter those. If you maintain a multilingual shop, you can add an identifier which refers to the language file of your choice for the translation of filter names. The identifier has to be defined in the database in advance and the language files have to be maintained in your OXSEARCH directory in
	/application/views/azure/[Sprachverzeichnis]
- Article: Here you can set filters on article details such as price and weight.
- Script: This option enables you to write scripts which check certain values to define specials.

### Synonyms and searchable links ###

This tab is divided into several sections and covers:

- Dynamic Boosting
- Dynamic Boosting devaluation
- Landing Pages
- Synonyms
- Antonyms

#### Dynamic Boosting ####

The dynamic boosting manipulates the position of articles in category views as well as search results. This is done by dynamically changing values such as sales values or visits to article pages. You can define the factors for the weight of statistics.
![Dynamic Boosting](img/oxsearch_dynamic_boosting.png)

- Enable dynamic Boosting: You have to check dynamic boosting in order to benefit from the below settings.
- Use for categories: If you enable this option, the category sorting doesn't aplly anymore.
- Boostfactor "sold amount"
- Boostfactor "added to card"
- Boostfactor "view count details"
- Boostfactor "promotions"
- Boostfactor "revenue"
- Boostfactor "rating"
- Boostfactor "profit margin"
- Relevance exponent: defines the factor with which the quantifiers are potentiated. The following formular is applied:
boostfactor value multiplied with relevance exponentiated with relevance exponent

___Note: Boostfactor "profit margin" can be set in product's administration___

The values for one article added.
___Note: We recommend the following boost values:___

- Added to card: 10
- boostfactor sold amount: 100
- Boostfactor view count details: 1  __
- Enable debug mode: When the administrator logs into the shop frontend, he can see the result of the boosting settings.

#### Dynamic Boosting devaluation ####

By starting the devaluation all dynamic boosting's tracking fields (times added to basket, ratings, views, sold, revenue) will be decreased by entered percentage.  
Devaluation process can also be performed by a cron job. Please take a look at "crons/marm_oxsearch_boosting_devaluation.php"

#### Landing Pages ####

Assuming you activated the corresponding option in the search options, this section defines which links asside from shop articles are displayed in the search results. Again, the most common examples are payment or terms of service which also appear in the auto suggestion.
![searchable links](img/oxsearch_searchable_links.png)
The following information is required for each link:

- Title
- external
- page
___Note: In this section, the translated version of every page has to be added explicitely. If you offer a German version of your terms of service, you have to include the link to AGB to this list!
Every searchable link needs at least one keyword lest it is ignored without an error message!___

#### Synonyms ####

This section serves for maintaining synonyms.
![Synonyms](img/oxsearch_synonyms.png)
If "candy bar" appears in the statistic for searkrequest without results, you can define "chocolate bar" as a synonym.
___Please note that the index requires a new rebuilt! Also, synonyms can only be in lowercase. If you try to use uppercase synonyms, OXSEARCH will convert them into lowercase.___

#### Antonyms ####

This section serves for maintaining antonyms.
![Antonyms](img/oxsearch_antonyms.png)
In order to exclude results containing word "kiteboarding" while searching for "kite" such a rule should be added:
"kite => kiteboarding". This excludes all the products which have word "kiteboarding" in their title, short description or manufacturer's title.
Following these tips will ensure that newly added rules are correct:

- Rule is separated into right and left side by "=>"
- Every new rule must start at the new line
- Multiple expressions on the right side of the rule must be separated by comma

___Note: Left side is handled as a single expression___

### Searchrequest without results ###

This section serves a statistical purpose. We recommend to extend your synonyms based on the evaluation of this page.
![search requests without result](img/oxsearch_searchrequests_without_result.png)

## Notes # #
### LandingPages and Promotions ###

OXID's actions interface can be used to set up landingpages and promotions which can be supplied with dynamic filters. Use cases are weekly or monthly specials or most popular articles.

### Categories ###

Categories can be populated dynamically with articles. In the tab __dynamic article selection__ in the OXID settings under administer products > categories, you can assign values to the filters defined in OXSEARCH. It is not even necessary for the filter to be active, it just needs a unique parameter name. To decide whichfilters aplly for which category, select the visible filters tab. Exemplary dynamic categories are weekly or monthly specials or gift items.

### multilingual shops ###

If your shop is multilingual, we recommend that you define a language identifier so articles and pages are displayed in the respective language. Every language has its own index and has to be maintained separately. Maintaining articles therefore requires the selection of the language you want to edit via a dropdown list.

### Multi-tenancy capability ###

OXSEARCH is multi-tenancy capable. Please note that you have to install and configure OXSEARCH individually for every subshop. To guarantee ful functionality for boosting settings, make sure that every subshop has its own index.

### Usage of jQuery ###

OXID comes with preinstalled copies of [jQuery](http://jquery.com) and [jQueryUI](http://jqueryui.com).
If you want to prevent their implementation because, for example, you want to use newer versions of both or totally different libraries, just create the file `widget/header/autosuggestion.tpl` with the following content:

		    [{if $oViewConf->isActivated('autosuggest')}]
		        <script type="text/javascript">
		            var source = '[{$oViewConf->getAutosuggestionLink()}]';
		        </script>
		        [{oxstyle include=$oViewConf->getModuleUrl('marm/oxsearch','out/src/css/autosuggest.css')}]
		        [{oxscript include=$oViewConf->getModuleUrl('marm/oxsearch','out/src/js/autosuggest.js')}]
		    [{/if}]

Thus you can use OXSEARCH's autosuggestion script without having to include several copies of jQuery.

### Support of OXID standard functions ###

As OXSEARCH is based on OXID, it supports most of the OXID standard functions. However, there are a few functions not supported:

- user roles and rights in the frontend
- Varnish
However, unsupported OXID features can be adapted for specific projects.
