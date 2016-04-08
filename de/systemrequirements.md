# Systemanforderungen #

Um eine Einbindung von OXSEARCH in OXID zu gewährleisten, müssen folgende Komponenten installiert sein:

* PHP 5.3+
* OXID 4.8+ oder höher
* PHP-curl: dies ist eine der Voraussetzungen für OXID und sollte bereits vorhanden sein.
* OXSEARCH 3.x
    * Elasticsearch 1.1 - 1.7
    * optionale Elasticsearch-Plugins für bessere Umlaut und Unicode Suche  
        * elasticsearch-analysis-icu  
        * elasticsearch-analysis-combo  
* OXSEARCH 4.x
    * Elasticsearch 2.x
    * Optional ElasticSearch-Plugins für bessere Suchergebnisse zusammengesetzter Wörter
        - elasticsearch-analysis-decompound (https://github.com/jprante/elasticsearch-analysis-decompound)

## Hinweise: ##
* OXSEARCH 3.x
    * Ab Elasticsearch 1.2 muss dynamisches Scripting explizit aktiviert werden. Dazu muss in die Datei elasticsearch.yml die Zeile `script.disable_dynamic: false` eingefügt werden. 
    * Mit Elasticsearch 1.4 wurde die von OXSEARCH verwendete Scriptsprache mvel abgeschafft. Zum Erhalt des vollen Funktionsumfangs ist es deshalb notwendig, diese in Form des Plugins elasticsearch-lang-mvel nachzuinstallieren. 
    * Nach der Installation muss mvel als Standardsprache in der elasticsearch.yml eingetragen werden. `script.default_lang: "mvel"`
* OXSEARCH 4.x
    * dynamisches Scripting muss explizit aktiviert werden. Dazu muss in die Datei elasticsearch.yml die Zeile `script.inline: on` eingefügt werden.
    * In ElasticSearch 2.x ist die Scriptsprache mvel nicht länger verfügbar.
Um über den vollen Funktionsumfang von OXSEARCH zu verfügen, sollten Sie für Ihre Scripte die Scriptsprache groovy verwenden.
