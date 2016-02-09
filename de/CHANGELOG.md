OXSEARCH CHANGELOG
==================

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
* ADDED Search results comparison with other shops/environments
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

