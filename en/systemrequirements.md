## System requirements ##

For OXID and OXSEARCH to run smoothly together, make sure the following components are installed:  

- PHP 5.3+  
- OXID 4.8+ or higher  
- Php-curl: This is a requirement for OXID and therefore should already be installed..  
- Elasticsearch 1.1 bis 1.4 (Elasticsearch 1.5+ is not supported yet) 
- optional Elasticsearch-Plugins for better Unicode search  
 - elasticsearch-analysis-icu  
 - elasticsearch-analysis-combo  

__Notes__  

- Elasticsearch 1.2 requires an explicite activation of dynamic scripting. To achieve that, please add the line die  
	script.disable_dynamic: false	
to the file elasticsearch.yml.  
- In ElasticSearch 1.4, the scripting language mvel is no longer existant.  
To maintain ful functionality of OXSEARCH, it is therefore necessary to install the plugin elasticsearch-lang-mvel.  
- After installing the plugin, mvel must be defined as the standard language in elasticsearch.yml.  
	script.default_lang: "mvel"	

