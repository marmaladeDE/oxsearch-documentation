# Systemanforderungen #

Um eine Einbindung von OXSEARCH in OXID zu gewährleisten, müssen folgende Komponenten installiert sein:

* PHP 5.3+  
* OXID 4.8+ oder höher  
* PHP-curl: dies ist eine der Voraussetzungen für OXID und sollte bereits vorhanden sein.  
* Elasticsearch 1.1 bis 1.4 (Elasticsearch 1.5+ wird derzeit noch nicht unterstützt) 
* optionale Elasticsearch-Plugins für bessere Umlaut und Unicode Suche  
    * elasticsearch-analysis-icu  
    * elasticsearch-analysis-combo  

## Hinweise: ## 
* Ab Elasticsearch 1.2 muss dynamisches Scripting explizit aktiviert werden. Dazu muss in die Datei elasticsearch.yml die Zeile `script.disable_dynamic: false` eingefügt werden. 
* Mit Elasticsearch 1.4 wurde die von OXSEARCH verwendete Scriptsprache mvel abgeschafft. Zum Erhalt des vollen Funktionsumfangs ist es deshalb notwendig, diese in Form des Plugins elasticsearch-lang-mvel nachzuinstallieren. 
* Nach der Installation muss mvel als Standardsprache in der elasticsearch.yml eingetragen werden. `script.default_lang: "mvel"`