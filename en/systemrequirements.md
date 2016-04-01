## System requirements ##

For OXID and OXSEARCH to run smoothly together, make sure the following components are installed:

- PHP 5.3+
- OXID 4.8+ or higher
- Php-curl: This is a requirement for OXID and therefore should already be installed..
- Elasticsearch 2.x
- optional Elasticsearch-Plugins for better search of compound words
  - elasticsearch-analysis-decompound (https://github.com/jprante/elasticsearch-analysis-decompound)

__Notes__

- Elasticsearch 2.x requires an explicite activation of dynamic scripting. To achieve that, please add the line
    `script.inline: on`
to the file elasticsearch.yml.
- In ElasticSearch 2.x, the scripting language mvel is no longer existant.
To maintain ful functionality of OXSEARCH, you have to write your scripts in groovy scripting language.

