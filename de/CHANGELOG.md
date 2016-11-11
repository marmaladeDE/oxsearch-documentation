OXSEARCH CHANGELOG
==================

version 4.0.15
--------------
* FIXED Promotions without defined filters crash search
* CHANGED Stricter version dependency in composer.json
* CHANGED URL to manufacturer page in autosuggest is now being fetched from oxManufacturer object

version 4.0.14
--------------
* FIXED Filter URLs where broken when using sorting and not using SEO URLs
* FIXED Inherited products didn't use proper URLs
* ADDED Blocks in template for synonym and searchable link configuration
* CHANGED Improved deep category query for importer

version 4.0.13
--------------
* FIXED Generated parameter names for filters retained dots
* ADDED Extension point for limiting the bucket count of aggregations
* ADDED Shop ID parameter for Update and Delete API
* CHANGED Category filter visibility gets saved as empty if not configured

version 4.0.12
--------------
* FIXED Update API wrote to wrong language index
* FIXED Delete API was not deleting article from all languages
* FIXED Some SQL queries used double quotes for strings
* CHANGED Added a lot of class comments

version 4.0.11
--------------
* FIXED "Reindex on save" caused trouble with various ERP/PIM update scripts
* FIXED Enabling SEO urls for filter broke sorting of product list
* ADDED Simple Logging for indexing step of importer

version 4.0.10
--------------
* FIXED Warnings on search page caused by misspelling of method

version 4.0.9
-------------
* FIXED CSS for slider still contained image references
* FIXED All settings were lost on deactivation
* ADDED Extension points for slider filters
* ADDED Support for custom filters
* ADDED Result caching for oxArticleList

version 4.0.8
-------------
* FIXED Inherited categories not showing deep category products
* FIXED Attribute titles sometimes not shown in filter configuration
* FIXED Stock status flag introduced in 4.0.7 causes warning

version 4.0.7
-------------
* FIXED Active field breaks import on ES 2.1
* FIXED Cannot use oxArticleList::loadCategoryArticles on search page
* CHANGED No stock sorting wasn't respecting stock status

version 4.0.6
-------------
* FIXED Broken category sorting
* FIXED No stock sorting brakes sorting in search
* FIXED Autosuggest does not load with only additionalSearchresults
* CHANGED Switched from truncate to oxtruncate in templates
* CHANGED marm_oxsearch_query_builder now gets instantiated in the dic using oxNew

version 4.0.5
-------------
* FIXED Language change lags one request behind
* FIXED OXSEARCHKEYS only worked when maintained lowercase
* ADDED Support for future OXHIDDEN flag
* CHANGED Language switching now resets filters

version 4.0.4
-------------
* FIXED Language change lags one request behind (only when SEO Filter URLs are disabled)
* FIXED Autosuggest broken on Flow
* CHANGED Search by article number no longer adds a redirected parameter

version 4.0.3
-------------
* FIXED Search by article number shouldn't find inactive products
* FIXED Synonyms weren't added on index (re)creation
* FIXED Bad encoding of OXID in in db tracking adapter
* CHANGED Synonyms are now added to all language indexes
* CHANGED Autosuggestion shows single product if the search matches it's product number
* CHANGED Import from CLI now uses another endpoint (doImport.php)
* CHANGED Index names are now automatically changed to lowercase

version 4.0.2
-------------
* FIXED Dynamic boosting broke search relevancy
* ADDED Manufacturer thumbnails in autosuggestion

version 4.0.1
-------------
* ADDED Check for decompound plugin
* CHANGED Subshop aware tracking is now available

version 4.0.0
-------------
* ::: BREAKING CHANGES! :::
* MIGRATED to Elasticsearch 2.x. For Elasticsearc 1.x please use OXSEARCH 3.x version.
* CHANGED scripts from mvel to groovy language.

version 3.8.0
-------------
* FIXED Dynamic boosting counting of detail page’s views
* FIXED Wrong deselection of list filters when selecting multi-select filters
* ADDED Dynamic boosting improvement:
    - boosting after sales
    - boosting after profit margins
    - boosting after ratings
    - option to devaluate boosting values
    - out of stock products are displayed last
* ADDED Option to export elasticsearch index to a remote server
* ADDED Filter dependency configuration
* ADDED Functional testing implementation using Behat
* ADDED Help & Debugging tab in admin back-end
* ADDED Attribute autosuggestion for filters in landing pages, promotions and dynamic categories
* ADDED Redirection to product page when search matches exact product number
* CHANGED Moved Elasticsearch debugging to the tab "Help & Debugging"
* CHANGED Unified look for filters in landing pages, promotions and dynamic categories
* CHANGED Improved UI in admin back-end for deleting not used product filters
* CHANGED Improved UI in admin back-end how assessed search fields are described

version 3.7.2
-------------
* FIXED Autocompletion/suggest terms contained leading and trailing spaces

version 3.7.1
-------------
* FIXED Manual category sorting did not affect category order in EE
* FIXED Boost debugging displayed incorrect values due to tracking changes
* FIXED Reindexing products on save didn't apply modifiers correctly

version 3.7.0
-------------
* ::: BREAKING CHANGES! :::
* REMOVED Details controllers
* CHANGED New tracking system, default is now to put tracked counters into an extra table
* CHANGED Details page requests are now counted via ajax or pixel
* ADDED Tons of new config.inc.php options, see documentation
* ::: ALSO :::
* FIXED Category modifier for multishop caused inheritance in the wrong direction
* CHANGED Modifiers don't require stub columns anymore
* CHANGED Aggregation buckets without text or zero count are now getting removed

version 3.6
-----------
* FIXED Shops using mysqli broke on empty searches
* FIXED Shops using underscores as divider in SEO URLs broke
* FIXED Existence of index was checked even when is wasn't configured
* FIXED Missing argument caused warnings in promotion & landing page mapping views
* FIXED Promotions not applied to categories caused empty search results
* FIXED Could not find time limited products with empty starting date
* FIXED The index status was always using the active backend language, even when it wasn't active for frontend use
* FIXED Fallback mode caused a notice until it happened once
* ADDED Configurable request timeouts
* CHANGED Synonyms are now processed more to avoid uppercase characters and empty lines, as they cause unexpected behaviour
* CHANGED First step in massive refactoring to comply with PSR-1 and PSR-2
* CHANGED Removal of some dead code
* CHANGED Scripts needed for autosuggestion are now loaded in a template include, to make it easier to override them

version 3.5.1
-------------
* FIXED Filterparams were not added to search detail locator links
* FIXED Details pages were not accessible while seo-mode is activate

version 3.5
-----------
* FIXED Variants where indexed as full articles on save
* FIXED Multishop aggregation for update api was missing
* FIXED Display type was not stored for new field filters
* FIXED Autosuggestion was broken on SSL pages
* FIXED Categories came up empty when handled by OXSEARCH and no filters active
* FIXED OXIDs dynamic content cache broke OXSEARCH
* FIXED Import didn't use languages of subshops
* FIXED Category filter showed hidden or inactive categories
* ADDED CLI importer can now swap indexes
* CHANGED Settings backup is now subshop-aware
* CHANGED Only remove products from index that where successfully deleted
* CHANGED Categories can now show globally disabled filters
* CHANGED Full import now deletes inactive index
* CHANGED Prices are now always mapped to double in Elasticsearch
* CHANGED Inherited products are now sorted behind products directly assigned to the current category
* CHANGED Default values for import size are now lower

version 3.4.4
-------------
* FIXED Search parameter was lost when SEO urls for filters where activated
* FIXED Using `oxarticle.save` in a CLI script caused the importer usage message to be shown
CHANGED Single article updates can now be caused via API calls. See `readme.md`

version 3.4.3
-------------
* FIXED A hack enabling usage with latin1 shops was removed
* FIXED The association between products, categories and subshops was not stored nor queried properly
* FIXED Locator header on details pages was showing bogus info
* ADDED Products can optionally re-indexed on save
* ADDED Locator can now be disabled
* ADDED Silent mode for importer
* CHANGED Some performance improvements
* CHANGED Modifiers are now properly extensible

version 3.4.2
-------------
* FIXED category filter not working for manufacturerlist

version 3.4.1
-------------
* FIXED OXID >=4.9/5.2 displays translation errors in filter templates
* FIXED filter exclude for product fields
* FIXED syntax error in category_mapping
* FIXED oxactivefrom/oxactiveto for variants does not work correct
* ADDED category filter option
* ADDED term suggestions to autosuggest
* ADDED backup/restore for OXSEARCH configuration
* CHANGED performance of multiselect filters improved
* CHANGED rss feed now sorted by relevance

version 3.4
-----------
* ADDED dynamic category building
* ADDED new filter options for color and rating
* ADDED elasticsearch import from oxid backend
* ADDED optionally split attribute values at an defined sign to separate filter options
* ADDED optionally display subcategory items in parent category
* ADDED fallback mode if elasticsearch index is not available
* ADDED Selection if search terms are joined with AND/OR
* ADDED allow wildcard searches
* CHANGED performance of auto-suggest improved
* CHANGED dynamic boosting improved

version 3.2
-----------
* ADDED optionally assign filters for each category

version 3.1
-----------
* REMOVED elasticsearch mysql river support and dependency
* ADDED importer script
* ADDED support for new OXID EE multi shop handling

version 3.0
-----------
* refactoring with integration of unit tests, auto-loader and dic

