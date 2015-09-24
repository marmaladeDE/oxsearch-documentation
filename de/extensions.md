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
    
OXSEARCH bringt mehrere eigene Modifikatoren mit:

 * `product_OXPRICE` und `product_OXVARMINPRICE` berechnen dn Bruttopreis in die entsprechenden Preisfelder ein, sofern
   der Shop im Nettomodus arbeitet.
 * `product_SUGGEST` baut ein Feld für eine Autovervollständigung auf.
 * `product_OXLONGDESC` säubert die Langbeschreibung von HTML und kürzt sie, sofern sie zu lang für Elasticsearch wird.
 * `product_OXSOLDAMOUNT`, `product_MARM_OXSEARCH_REQCOUNT` und `product_MARM_OXSEARCH_BASKETCOUNT` überschreiben die
   alten Trackingfelder mit Daten aus dem Tracking-Adapter.
 * `product_TRACKING` holt alle getrackten Datenfelder für den Artikel aus dem Tracking-Adapter.
 
Für eigene Modifikationen erweitert man die Klasse in einem eigenen Modul und implementiert die Modifikationsmethoden
mit demselben Namensschema.

__Hinweis: Modifikatoren werden nur auf existierende Felder angewendet. Das gilt auch für Felder, die nur im Rahmen von
OXSEARCH oder einer OXSEARCH-Erweiterung verwendet werden! Für diesen Fall ist es aber bereits ausreichend, das Feld in der
Datenbank als `TINYINT(1) NULL` anzulegen.__

### Eigene Tracking-Werte ###

Neben Detailseitenaufrufe, legen des Artikels in den Warenkorb und Käufen können beliebig weitere Werte getrackt werden,
um diese in eigenen Elasticsearchanfragen oder Analysen zu benutzen. Für Artikel reicht es, dazu die Methode
`oxArticle::getTracker` aufzurufen:

    $id = 'some_id';
    $oArticle = oxNew('oxarticle');
    if ($oArticle->load($id)) {
        $oArticle->getTracker('some_key')->increase();
    }
    
Für andere Objekte kann man direkt auf das Tracking-System zugreifen:

    $dic = oxRegistry::get('yamm_dic');
    $dic['marm_oxsearch']['tracking']->getTracker('some_type', 'some_id', 'some_key')->inc();
    
In beiden Fällen erhält man ein Tracker-Objekt, welches den zu trackenden mittels dieser vier Methoden manipulieren kann:

 * `increase($by = 1)` erhöht den Wert um den Inhalt von `$by`.
 * `decrease($by = 1)` verringert den Wert um den Inhalt von `$by`.
 * `set($value)` setzt den Wert auf den Inhalt von `$value`.
 * `get()` holt den aktuellen Wert.
 
__Hinweis: Die Tracker-Klasse kann nicht erweitert werden!__

### Eigene Tracking-Adapter ###

Standardmäßig werden getrackte Daten in einer zusätzlichen Tabelle innerhalb der Datenbank abgelegt. Um dieses Verhalten
zu verändern (z.B. um Redis benutzen). Dazu sind zwei Aktionen notwendig.
Zuerst muss die abstrakte Klasse `AbstractOxSearchTracking` abgeleitet und implementiert werden:

    class SpecificTracking extends AbstractOxSearchTracking {
        public function increase($type, $id, $key, $by = 1) {} // increase a tracking value
        public function decrease($type, $id, $key, $by = 1) {} // descrease a tracking value
        public function set($type, $id, $key, $value) {}       // set a tracking value
        public function get($type, $id, $key = null) {}        // get a tracking value
        public function isInitialized() {}                     // check if the adapter is ready to use
        public function initialize() {}                        // Do setup, e.g. of tables
        public function listTrackedObjects($limit = false, $from = 0) {} // List tracked objects (not keys!) as list of array('type' => 'x', 'id' => 'y')
        public function countTrackedObjects() {}               // get number of tracked objects (not keys)
    }
    
Danach muss das Tracking über die `config.inc.php` konfiguriert werden:

     $this->aOxSearchTracking = array(
         'adapter' => 'SpecificTracking',
         'setting1' => 'some Setting',
         'setting2' => 'some Other Setting'
         ...
     );

Die Konfigurationoptionen sind innerhalb des Adapters über `$this->settings` verfügbar.
