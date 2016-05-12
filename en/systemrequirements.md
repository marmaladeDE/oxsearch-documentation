## System requirements ##

For OXID and OXSEARCH to run smoothly together, make sure the following components are installed:

- PHP 5.4+
- OXID 4.8+ or higher
- Php-curl: This is a requirement for OXID and therefore should already be installed..
- OXSEARCH 3.x
    - Elasticsearch 1.1 - 1.7
    - optional Elasticsearch plugins for better unicode search  
        - elasticsearch-analysis-icu  
        - elasticsearch-analysis-combo
- OXSEARCH 4.x
    - Elasticsearch 2.x
    - optional Elasticsearch-Plugins for better search of compound words
        - elasticsearch-analysis-decompound (https://github.com/jprante/elasticsearch-analysis-decompound)

__Notes__

- OXSEARCH 3.x
    - Elasticsearch 1.2 requires an explicite activation of dynamic scripting. To achieve that, please add the line  
         `script.disable_dynamic: false`
         to the file elasticsearch.yml.  
    - In ElasticSearch 1.4, the scripting language mvel is no longer existant.  
      To maintain ful functionality of OXSEARCH, it is therefore necessary to install the plugin elasticsearch-lang-mvel.  
    - After installing the plugin, mvel must be defined as the standard language in elasticsearch.yml.  
        `script.default_lang: "mvel"`
- OXSEARCH 4.x
    - Elasticsearch 2.x requires an explicite activation of dynamic scripting. To achieve that, please add the line
        `script.inline: on`
        to the file elasticsearch.yml.
    - In ElasticSearch 2.x, the scripting language mvel is no longer existant.
        To maintain ful functionality of OXSEARCH, you have to write your scripts in groovy scripting language.

