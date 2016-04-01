# Systemanforderungen #

Um eine Einbindung von OXSEARCH in OXID zu gewährleisten, müssen folgende Komponenten installiert sein:

* PHP 5.3+
* OXID 4.8+ oder höher
* PHP-curl: dies ist eine der Voraussetzungen für OXID und sollte bereits vorhanden sein.
* Elasticsearch 2.x
* Optional ElasticSearch-Plugins für bessere Suchergebnisse zusammengesetzter Wörter
  - elasticsearch-analysis-decompound (https://github.com/jprante/elasticsearch-analysis-decompound)

## Hinweise: ##
* Ab Elasticsearch 2.x muss dynamisches Scripting explizit aktiviert werden. Dazu muss in die Datei elasticsearch.yml die Zeile `script.inline: on` eingefügt werden.
* In ElasticSearch 2.x ist die Scriptsprache mvel nicht länger verfügbar.
Um über den vollen Funktionsumfang von OXSEARCH zu verfügen, sollten Sie für Ihre Scripte die Scriptsprache groovy verwenden.