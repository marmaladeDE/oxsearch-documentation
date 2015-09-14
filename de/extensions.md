# OXSEARCH erweitern #

OXSEARCH kann durch weitere OXID-Module und -Themes erweitert werden. Niemals sollte OXSEARCH-Dateien direkt editiert
werden!

### Postprocessing mittels Modifier ###

Manchmal ist es notwendig das Felder in Elasticsearch andere Werte enthalten als in der Datenbank, oder dass neue Felder
eingeführt werden müssen, welche nur innerhalb der Suche von Bedeutung sind. Für dieses Postprocessing ist die Klasse
`marmOxsearchModifiers` zuständig. Die Methoden dieser Klasse werden zwischen der ersten Aggregation und
der Übertragung in den Index auf die einzelnen Datensätze angewendet, wobei die Zuordnung durch ein vorgegebenes
Namensschema erreicht wird:

    public function [datentyp]_[FELD]($value, $set) {}
    
 * _datentyp_ ist der Typ des Datensatz. OXSEARCH unterstützt folgende Typen:
    * _product_ ist ein (Eltern-)Artikel.
    * _category_ beschreibt eine Produktkategorie.
    * _variant_ ist eine Produktvariante.
   _datentyp_ muss kleingeschrieben werden.
 * _FELD_ ist der Name des Datensatzfeldes, welches modifiziert werden soll. Es muss immer groß geschrieben sein.
 * Die Parameter:
    * `$value` ist der aktuelle Wert des Feldes.
    * `$set` ist eine Kopie des Datensatzes.
    
OXSEARCH bringt drei eigene Modifikatoren mit:

 * `product_OXPRICE` und `product_OXVARMINPRICE` berechnen dn Bruttopreis in die entsprechenden Preisfelder ein, sofern
   der Shop im Nettomodus arbeitet.
 * `product_SUGGEST` baut ein Feld für eine Autoverfollständigung auf.
 
Für eigene Modifikationen erweitert man die Klasse in einem eigenen Modul und implementiert die Modifikationsmethoden
mit demselben Namensschema.

__Hinweis: Modifikatoren werden nur auf existierende Felder angewendet. Das gilt auch für Felder, die nur im Rahmen von
OXSEARCH oder einer OXSEARCH-Erweiterung verwendet werden! Für diesen Fall ist es aber bereits ausreichend, das Feld in der
Datenbank als `TINYINT(1) NULL` anzulegen.__
