OXSEARCH CHANGELOG
==================

version 3.6
-----------
FIXED Shops using mysqli broke on empty searches
FIXED Shops using underscores as divider in SEO URLs broke
FIXED Existence of index was checked even when is wasn't configured
FIXED Missing argument caused warnings in promotion & landing page mapping views
FIXED Promotions not applied to categories caused empty search results
FIXED Could not find time limited products with empty starting date
FIXED The index status was always using the active backend language, even when it wasn't active for frontend use
FIXED Fallback mode caused a notice until it happened once
ADDED Configurable request timeouts
CHANGED Synonyms are now processed more to avoid uppercase characters and empty lines, as they cause unexpected behaviour
CHANGED First step in massive refactoring to comply with PSR-1 and PSR-2
CHANGED Removal of some dead code
CHANGED Scripts needed for autosuggestion are now loaded in a template include, to make it easier to override them

version 3.5.1
-------------
FIXED Filterparams were not added to search detail locator links
FIXED Details pages were not accessible while seo-mode is activate

version 3.5
-----------
FIXED Variants where indexed as full articles on save
FIXED Multishop aggregation for update api was missing
FIXED Display type was not stored for new field filters
FIXED Autosuggestion was broken on SSL pages
FIXED Categories came up empty when handled by OXSEARCH and no filters active
FIXED OXIDs dynamic content cache broke OXSEARCH
FIXED Import didn't use languages of subshops
FIXED Category filter showed hidden or inactive categories
ADDED CLI importer can now swap indexes
CHANGED Settings backup is now subshop-aware
CHANGED Only remove products from index that where successfully deleted
CHANGED Categories can now show globally disabled filters
CHANGED Full import now deletes inactive index
CHANGED Prices are now always mapped to double in Elasticsearch
CHANGED Inherited products are now sorted behind products directly assigned to the current category
CHANGED Default values for import size are now lower

version 3.4.4
-------------
FIXED Search parameter was lost when SEO urls for filters where activated
FIXED Using `oxarticle.save` in a CLI script caused the importer usage message to be shown
CHANGED Single article updates can now be caused via API calls. See `readme.md`

version 3.4.3
-------------
FIXED A hack enabling usage with latin1 shops was removed
FIXED The association between products, categories and subshops was not stored nor queried properly
FIXED Locator header on details pages was showing bogus info
ADDED Products can optionally re-indexed on save
ADDED Locator can now be disabled
ADDED Silent mode for importer
CHANGED Some performance improvements
CHANGED Modifiers are now properly extensible

version 3.4.2
-------------
FIXED category filter not working for manufacturerlist

version 3.4.1
-------------
FIXED OXID >=4.9/5.2 displays translation errors in filter templates
FIXED filter exclude for product fields
FIXED syntax error in category_mapping
FIXED oxactivefrom/oxactiveto for variants does not work correct
ADDED category filter option
ADDED term suggestions to autosuggest
ADDED backup/restore for OXSEARCH configuration
CHANGED performance of multiselect filters improved
CHANGED rss feed now sorted by relevance

version 3.4
-----------
ADDED dynamic category building
ADDED new filter options for color and rating
ADDED elasticsearch import from oxid backend
ADDED optionally split attribute values at an defined sign to separate filter options
ADDED optionally display subcategory items in parent category
ADDED fallback mode if elasticsearch index is not available
ADDED Selection if search terms are joined with AND/OR
ADDED allow wildcard searches
CHANGED performance of auto-suggest improved
CHANGED dynamic boosting improved

version 3.2
-----------
ADDED optionally assign filters for each category

version 3.1
-----------
REMOVED elasticsearch mysql river support and dependency
ADDED importer script
ADDED support for new OXID EE multi shop handling

version 3.0
-----------
refactoring with integration of unit tests, auto-loader and dic

