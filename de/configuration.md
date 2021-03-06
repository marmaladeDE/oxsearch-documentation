# OXSEARCH-Setup #

Um OXSEARCH erfolgreich in OXID einzubinden, müssen nun noch einige wesentliche Einstellungen im OXSEARCH-Setup vorgenommen werden. Das Setup besteht aus
fünf Reitern mit folgenden Einstellungsmöglichkeiten:

- Im ersten Reiter OXSEARCH-Setup finden Sie Grund-, Such- & Filtereinstellungen.
- Im zweiten Reiter können Sie Boosting & Synonyme konfigurieren.
- Im dritten Reiter werden die Zugangsdaten zu Elasticsearch hinterlegt & die Indizierung gestartet.
- Im vierten Reiter werden die Suchbegriffe fehlgeschlagener Suchen aufgelistet.
- In den Bereichen Hilfe und Fehlersuche finden Sie Informationen, hilfreiche Dokumentationslinks und einige Werkzeuge zur Fehlersuche.

![Moduleinstellungen für OXID](img/oxsearch_setup.png)

## Elasticsearch-Verbindungsdaten und Indizierung ##

Bevor Sie OXSEARCH nutzen können ist es erforderlich, die Verbindungsdaten für Elasticsearch zu hinterlegen und die Indizierung zu starten.

1. Wechseln Sie in den Reiter "Elasticsearch Konfiguration > Verbindungsdaten".
2. Tragen Sie den Host in der Form [USER:PASSWORT@]ES-DOMAIN_ODER_IP ein.
3. Legen Sie den Port fest, auf dem Elasticsearch auf Anfragen horcht, meistens ist das 9200.
4. Speichern Sie zunächst die eingetragenen Verbindungsdaten, bevor Sie mit dem nächsten Schritt fortfahren.
5. Öffnen Sie erneut die Verbindungseinstellungen zu Elasticsearch.
6. Definieren Sie einen eindeutigen Indexnamen für den aktiven und den passiven Index.
__Hinweis: der Indexname kann für beide Indizes gleich sein, muss aber unbedingt klein werden, da sonst eine Fehlermeldung "Invalid JSON" auftritt. Es ist wichtig, dass beide Indizes definiert werden, da sonst der aktive Index nicht beschrieben wird.__
Auf dem aktiven Index findet im Shop-Frontend die Suche statt, während der passive Index allein zur Indexerstellung benötigt wird. Nach der Indexerstellung und der Befüllung tauschen Sie die Indizes mit Betätigung des entsprechenden Schalters.
__Hinweis: Elasticsearch sollte mindestens mit einer "basic authorization" konfiguriert werden, um Manipulationen des Shop-Bestands von außen zu vermeiden!__

![Verbindungsdaten und Indexerstellung](img/oxsearch_elastic_search_config.png)

## Datenimport ##

Der Datenimport kann auf drei verschiedene Arten durchgeführt werden.

- Im Backend durch drücken der Schaltfläche "Index jetzt neu aufbauen" im Reiter Elasticsearch-Konfiguration
- mittels eines Cronjobs, der die Skriptdatei `<ShopRoot>/modules/marm/oxsearch/importer/doImport.sh` ausführt
- mit Hilfe der Update- & Delete-API

Wenn Sie die Schaltfläche im Backend benutzen, wirken sich vorgenommene Änderungen sofort aus. Je nach Datenmenge, kann der Neuaufbau einige Zeit in Anspruch nehmen und die Konfigurationsseite muss während des Indexaufbaus geöffnet bleiben, ansonsten bricht der Indexaufbau ab.
Das Update durch den Cronjob geschieht im Hintergrund und bedarf keiner aktiven Ausführung, genau wie die Schaltfläche "Index jetzt neu aufbauen" im Backend handelt es sich hier um einen Vollimport, der in Abhängigkeit zur Datenmenge einige Zeit in Anspruch nehmen kann.
Die Update- und Delete-API bietet sich an, wenn gezielte Aktualisierungen vorgenommen werden sollen.


### Data export ###

Der ElasticSearch-Index kann auf einen Remoteserver exportiert werden. Dies ist hilfreich, wenn Probleme innerhalb der ElasticSearch-Konfiguration auftreten und eine Kopie des Indexes an eine Agentur ausgegeben werden soll. Tragen Sie in das Eingabefeld den gesamten Indexpfad ein, z.B. `http://elastic.example.com:9200/index_name`. Wird eine Kennwort-Abfrage eingeblendet, um auf den Index zuzugreifen, geben Sie bitte den folgenden Pfad ein:  `http://user:password@elastic.example.com:9200/index_name`.
__Hinweis: Ein Index muss vor dem Export angelegt werden.__

#### Update- & Delete-API ####

Es gibt Situationen, in denen der Vollimport nicht die optimale Lösung darstellt. Wenn Sie Ihren Shop beispielsweise mit jeder Bestandsveränderung im Lager neu indizieren, belastet das Ihren Shop bei großem Datenaufkommen erheblich. Um die Indizierung an Ihre Bedürfnisse anpassen zu können und die Indizierung bestimmter oder aller Artikel beispielsweise an die Erreichung eines Lagerbestands von 10 knüpfen zu können, bietet die Klasse marmOxsearchImport einige Methoden zur Aktualisierung und Löschung von Artikeln:

- `updateArticle($articleId, $language = 0, $index = 'active')` aktualisiert einen einzelnen Artikel.
- `updateArticles($articleIds, $language = 0, $index = 'active')` ist das äquivalent für eine Liste von Artikeln.
- `deleteArticle($articleId, $index = 'active')` löscht einen Artikel.
- `deleteArticles($articleIds, $index = 'active')` löscht mehrere Artikel.

Der optionale Parameter $language wählt die Sprache anhand der Sprachnummern von OXID, während $index die Wahl zwischen aktivem (active) und inaktiven (inactive) Index erlaubt.

Verwendungsbeispiel:
		$sOxid = oxRegistry::getConfig()-		>getRequestParameter('oxid');
		oxRegistry::get('marmOxsearchImport')->updateArticle($sOxid);`

__Hinweis: Die Listenmethoden sind mit Bedacht zu verwenden, da sie keine Begrenzung enthalten und große Artikelzahlen zu Speicher- oder Laufzeitüberschreitungen führen können.__

### Reiter OXSEARCH-Setup ###

Die Grundeinstellungen von OXSEARCH sind in folgende Abschnitte unterteilt:

- Grundeinstellungen für wesentliche Konfigurationsmöglichkeiten von OXSEARCH
- Sucheinstellungen zur Festlegung von Suchkriterien
- Produktfilter

#### Grundeinstellungen ####

![OXSEARCH-Setup](img/oxsearch_setup.png)

- Aktivierung des Moduls
- Listenansicht: legt fest, ob für Kategorielisten der OXID- oder OXSEARCH-Standard verwendet wird. Erst bei gesetztem Haken profitieren Sie von vorgenommenen Filtereinstellungen.
- Suche: Hier entscheiden Sie, ob für die Suche die Einstellungen von OXID verwendet oder ob die Filtereinstellungen von OXSEARCH genutzt werden. Nur bei Aktivierung des Kontrollkästchens kommen in OXSEARCH vorgenommene Filtereinstellungen zur Geltung.
__Hinweis: Diese Option erfordert eine erneute Indizierung!__
- Autosuggestion unterbreitet während der Sucheingabe Vorschläge zu Suchbegriffen
- Seitennummer an Artikel-URL anhängen: Bei mehrseitigen Listen wird ab Seite 2 die Seiten-Nummer in der URL hinterlegt.
- Aktuelle Promotion überschreibt Kategoriesortierung: Diese Einstellung nimmt Einfluss darauf, ob die Sortierung innerhalb einer Kategorie von der aktuellen Promotion ersetzt wird. Mit der Promotion von Artikeln können Sie Einfluss darauf nehmen, ob ein bestimmter Artikel oder eine Gruppe von Artikel ganz vorne oder weiter hinten in der Liste angezeigt wird. Promotionen können Sie in OXID unter Kundeninformation > Aktionslisten oder direkt am Artikel unter Artikel verwalten > Artikel > Reiter Erweitert (Boost-Wert) pflegen.
- Elternkategorien enthalten alle Produkte ihrer Kinder: Wenn Ihr Shop beispielsweise eine Kategorie "Computer und Zubehör" enthält, die aus den Unterkategorien "Computer" und "Zubehör" besteht, können Sie hier beeinflussen, ob bei der Auswahl der Kategorie alle zugeordneten Artikel der Unterkategorien angezeigt werden oder ob der Kunde sich erst für eine der genannten Unterkategorien entscheiden muss.
__Hinweis: Diese Option erfordert eine neue Indizierung!__
- Artikel beim Speichern erneut indizieren: Wenn die Indizierung der Artikel durch einen ChronJob automatisch erfolgt, ist diese Option optional.
__Hinweis: Wenn diese Option aktiviert ist, werden Artikel, welche im Shop gelöscht wurden, auch aus Elasticsearch entfernt. Andernfalls ist für die Löschung eine erneute Indizierung notwendig!__
- oxLocator für Suchergebnis-Detailseiten abschalten: Der oxLocator fügt den Detailseiten der Artikel Navigationslinks hinzu, mit deren Hilfe man zwischen den Artikeln hin- und herspringen kann (u. a. vorheriger Artikel, nächster Artikel, etc.).
___Hinweis: Der aktivierte oxLocator wirkt sich negativ auf die Performance aus.___
- Trennzeichen für Attribute mit mehreren Wertausprägungen: Wenn ein Artikel beispielsweise aus mehreren Materialien besteht, besteht das Attribut "Material" aus zwei Wertausprägungen. Das Trennzeichen stellt sicher, dass beide Materialien für den betreffenden Artikel gespeichert werden.

#### Sucheinstellungen ####

In den Sucheinstellungen können Sie Suchfilter festlegen. Alle diese Ergebnisse wirken sich auf die Suchergebnisse aus.

- Gewichtete Suchfilter: Hier werden die Wertigkeiten für Suchfelder angegeben. Ein klassischer Anwendungsfall wäre, dass die Artikeltitel mit einer Wertigkeit von 10 höher gewichtet wird als der Suchbegriff in der Kurzbeschreibung, die beispielhaft mit 3 angegeben wird.
Note that if you want your customers to find the product by it's exact product number, add a search attribute _OXARTNUM_ (with multiplier 10) and _OXARTNUM.unanalyzed_ (with multiplier 1000).[de]
- Attribute Durchsuchen: Attribute sind Eigenschaften, die ein Artikel enthalten kann (Verschiedene Größen, Farben, etc.). Ist diese Option aktiviert, könnte der Endkunde im Shop-Frontend beispielsweise Suchanfragen mit der Attributsausprägungen `blau` auslösen.
- Varianten durchsuchen: Die Variante eines Artikels hat immer einen Elternartikel, von dem die Variante erbt. Im Fall von Bekleidung in verschiedenen Größen würde das bedeuten, dass es einen Artikel für ein bestimmtes Kleidungsstück gibt und die Varianten (verschiedene Farben und Größen) die jeweiligen Kindartikel sind. Im Frontend wird nur ein Artikel angezeigt und innerhalb des Artikels kann der Kunde mittels einer Dropdown-Liste die präferierte Variante wählen.
- Zusätzliche Fuzzy-Suche: Die Fuzzy-Suche korrigiert Tippfehler wie Buchstabendreher. Mindestens 30 % des Wortes müssen dabei richtig geschrieben sein.
- Wildcard-Suche: Hier können Sie entscheiden, ob für die Suche auch Platzhalter genutzt werden können. Wildcards sind sinnvoll, wenn der Name eines Artikels aus zusammengesetzten Wörtern besteht. Je nachdem, aus wie Vielen Wörtern der Begriff zusammen gesetzt ist, werden betroffene Artikel nicht immer zuverlässig gefunden. "Kiteboard" im OXID Demoshop besteht nur aus zwei zusammen gesetzten Wörtern und würde auch ohne Wildcard-Suche gefunden werden, wenn es jetzt aber "Kiteboard-Hüllen" gibt, würden ohne Wildcard-Suche hauptsächlich alle Treffer mit "Kiteboard" und "Hülle" ausgegeben werden.
___Hinweis: Die Wildcard-Suche wirkt sich negativ auf die Performance aus!___
- Bei Einzelergebnissen nicht zum Artikel weiterleiten: Mit dieser Option beeinflussen Sie, ob im Falle eines einzigen Treffers auf die Suchergebnisseite oder direkt zum Artikel weitergeleitet wird.
- Zeige Suchvorschläge: Während der Sucheingabe wird mit Hilfe von Autosuggestion der Suchbegriff vervollständigt.
- OXSEARCH-Keys: Hier legen Sie fest, welche Felder (Artikelbezeichnung, Kurzbeschreibung, Langbeschreibung, etc.) in die Suche einbezogen werden sollen.
- Zeige gefundene Kategorien: Bei aktivierter Option sind in der Trefferliste im Frontend alle Kategorien sichtbar, in denen Treffer gefunden wurden.
- Zeige gefundene Hersteller listet gefundene Hersteller des gesuchten Artikels
- Zeige gefundene Links: Ist diese Option aktiviert, kann man im Shop beispielsweise Zahlungs- und Versandbedingungen oder Öffnungszeiten suchen.
- Cutoff-Häufigkeit: Hier können Sie einen absoluten oder relativen Wert festlegen, der häufig auftauchenden Wörtern wie "der", "die", "das" eine geringere Relevanz zuordnet und in einem Subquerry verarbeitet, damit die Trefferliste möglichst exakte Ergebnisse liefert.
- Preisbereich für Suchergebnisse (von ... bis ...): Hier können Sie die angezeigten Artikel auf den von Ihnen gewünschten Preisbereich beschränken. Dies ist sehr hilfreich, wenn Sie beispielsweise kostenlose Artikel, wie Dreingaben aus den Suchergebnissen ausschließen wollen.
- Abfrage-Verknüpfung: In dieser Select-Box wählen Sie aus, ob alle oder mindestens einer der abgefragten Kriterien zutreffen.

#### Produktfilter ####

In diesem Reiter können Sie Produktfilter konfigurieren.

- SEO-URLS für gefilterte Seiten verwenden: Wenn diese Option deaktiviert ist, werden alle Filter in Form von Parametern an die URL angehängt. Je nach gewählten Filtern kann die Länge der URL zu Problemen führen, so dass die gewünschte Seite nicht angezeigt werden kann.
___Hinweis: Wenn SEO-URLS aktiviert sind, kann der Unterstrich nicht als Trennzeichen in anderen Modulen verwendet werden!___
- Anzahl gefundener Artikel am Filterwert anzeigen: Ist diese Option aktiviert, wird im Frontend sichtbar, wie viele Artikel für den aktivierten Filter gefunden wurden.
- Selektierte Filter nicht nach vorne sortieren: in einer Multiselectbox werden ausgewählte Filter nicht nach vorne sortiert, sondern in der ursprünglichen Filtersortierung angezeigt. Je nach Anzahl der Elemente in der Selektbox bedeutet dies bei nicht gesetztem Haken, dass Sie scrollen müssen, um die selektierten Filter zu finden.
- Kategoriefilter aktivieren: In der Ergebnisliste werden die Kategorien angezeigt, die den Suchbegriff enthalten.
__Hinweis: Dieser Filter funktioniert nur korrekt, sofern _Elternkategorien enthalten alle Produkte ihrer Kinder_ aktiviert ist!__
- Trennzeichen für dynamische Kategorien: Dynamische Kategorien werden von Elasticsearch aus bestehenden Artikeln befüllt. Die Beispielkategorie "Geschenkartikel unter 100 EUR" könnte alle Artikel unter 100 EUR enthalten. Das Trennzeichen für dynamische Kategorien muss sich von dem für statische Kategorien unterscheiden.
- Attribute: Hier können Sie Attribute für Artikel wie Farbe und Größe bei Bekleidung definieren, auf die OXSEARCH filtern kann. Für den Fall, dass Sie einen mehrsprachigen Shop pflegen, können Sie hier einen in der Datenbank festzulegenden Identifier hinterlegen, der für die Übersetzung der Feldnamen auf die Sprachdatei der von Ihnen gewünschten Sprache verweist. Die Sprachdateien für mehrsprachige Shops sind im OXID-Verzeichnis unter
	/application/views/azure/[Sprachverzeichnis]
in lang.php zu pflegen.
- Artikel: Hier können Sie Filter auf Artikelinformationen wie Preis und Gewicht setzen.
- Skript: Hier können Sie Scripts erstellen, die mehrere Werte prüfen und im Shop beispielsweise als Sonderposten, Saisonartikel, etc. angezeigt werden.

### Reiter Synonyme und suchbare Links ###

Der nächste Punkt in den OXSEARCH-Einstellungen befasst sich mit Synonymen, Antonymen und suchbaren Links. Er ist in folgende Abschnitte unterteilt:

- Dynamisches Boosting
- Dynamisches Boosting Abwertung
- Suchbare Links
- Synonyme
- Antonyme

#### Dynamisches Boosting ####

Das dynamische Boosting ermöglicht es, die Position von Produkten in der Kategorieansicht oder bei Suchergebnissen zu beeinflussen. Dies geschieht anhand sich dynamisch ändernder Werte wie Verkaufszahlen oder Detailseitenaufrufe. Sie können hier die Faktoren festlegen, mit denen die einzelnen Statistiken gewichtet werden.
![Dynamisches Boosting](img/oxsearch_dynamisches_boosting.png)

- Dynamisches Boosting aktivieren: Wenn das dynamische Boosting deaktiviert ist, profitieren Sie nicht von den weiter unten festgelegten Wichtungskriterien.
- Auf Kategorien anwenden: Wenn Sie sich für diese Option entscheiden, wird die Kategoriesortierung verworfen.
- Boostfaktor Anzahl der Verkäufe
- Boostfaktor in den Warenkorb gelegt
- Boostfaktor Anzahl der Detailseitenaufrufe
- Boostfaktor Promotionen: Dies ist ein vorgeschlagener Defaultwert von OXID. Ihren Wünschen entsprechend können Sie dieses Feld beliebig konfigurieren, beispielsweise nach Anzahl der Verkäufe.
- Boostfaktor Einkommen
- Boostfaktor Bewertungen
- Boostfaktor Gewinnspanne
- Relevanzexponent: Mit ihm legen Sie fest, mit welchem Faktor die Wichtungen potenziert werden. Dies dient zur Abwertung von unscharfen Treffern im Suchergebniss.
Folgende Formel wird angewandt:
Boostfaktor Wert multipliziert mit Relevanz potenziert mit Relevanzexponent
___Hinweis: Der Boostfactor "profit margin" kann in der Produktadministration festgelegt werden.___
Die Werte eines Artikels werden dabei addiert.
__Hinweis: Wir empfehlen folgende Werte:
- In den Warenkorb gelegt: 10
- Anzahl Detailseitenaufrufe: 1
- Anzahl Verkäufe: 100  __
- Debug-Modus aktivieren: Wenn der Administrator im Frontend eingeloggt ist sehen Sie, wie sich Ihre Gewichtungskriterien auf eine Suche oder Kategorieliste auswirken.

#### Dynamische Reduzierung des Boostings####

Mit der Reduzierung des Boostings, verringern sich alle dynamischen Felder (Zeiten für Papierkörbe, Bewertungen, Ansichten, Umsätze) um den eingegebenen Prozentsatz.
Dieser Prozess kann ebenso mit einem Cronjob durchgeführt werden. Weitere Hinweise dazu finden Sie hier: "crons/marm_oxsearch_boosting_devaluation.php"

#### Suchbare Links ####

Hier legen Sie fest, welche Links in der Suche aufgeführt werden sollen, wenn diese in den Sucheinstellungen aktiviert sind. Beispiele wären Zahlungs- und Versandbedingungen oder Öffnungszeiten, die auch in der Autosuggestion auftauchen.
![Suchbare Links](img/suchbare_links.png)
Folgende Angaben werden für einen Link benötigt:

- Titel
- extern
- Seite
___Hinweis: für die suchbaren Links müssen mehrsprachige Versionen einer Seite explizit festgelegt werden, für die AGB-Seite müssten Sie beispielsweise auch die terms of service-Seite aufführen!
Jeder suchbare Link benötigt mindestens ein Schlüsselwort, sonst wird er ohne Fehlermeldung verworfen!___

#### Synonyme ####

Hier können Sie Synonyme pflegen.
![Synonyme](img/oxsearch_synonyme.png)
Wenn die Statistik der Suchanfragen ohne Ergebnisse zeigt, dass "Softdrinks" keine Treffer ergeben, können Sie "alkoholfreie Getränke" als Synonym festlegen, damit "Softdrinks" künftig zum gewünschten Ergebnis führt.
___Hinweis: Die Festlegung von Synonymen erfordert eine Aktualisierung des Index! Beachten Sie, dass Synonyme nur aus Kleinbuchstaben bestehen können. Für den Fall, dass sie dennoch groß geschrieben werden, konvertiert OXSEARCH diese automatisch.___

#### Antonyme ####

Dieser Bereich beschreibt die Behandlung von Antonymen.
![Antonyms](img/oxsearch_antonyms.png)
Um Ergebnisse wie "kiteboarding" zu vermeiden, während nach "kite" gesucht wird, sollte folgende Regel hinzugefügt werden:
"kite => kiteboarding". Diese schließt all diejenigen Produkte aus, die das Wort "kiteboarding" in ihrem Titel, der Kurzbeschreibung oder dem Herstellertitel aufgeführt haben.
Die folgenden Tipps helfen dabei, neu hinzugefügte Regeln korrekt auszuführen:

- Eine Regel sollte rechts und links folgendermaßen abgetrennt werden "=>"
- Jede neue Regel beginnt in einer neuen Zeile.
- Mehrteilige Begriffe auf der rechten Seite einer Regel werden durch Kommas getrennt.

___Hinweis: Die linke Seite wird wie ein einzelner Begriff behandelt. ___

### Suchanfragen ohne Ergebnis ###

Diese Seite zeigt Ihnen an, welche Suchanfragen ohne Ergebnis blieben.
![Suchanfragen ohne Ergebnis](img/oxsearch_suchanfragen_ohne_ergebnis.png)
Bei der Auswertung bietet es sich an, Synonyme zu pflegen, die mit den ergebnislosen Suchanfragen verwandt sind.

## Anmerkungen ##
### Landing-Pages und Promotionen ###

Landing-Pages und Promotionen lassen sich über das Aktionen-Interface von OXID anlegen und mit dynamisch anzuwendenden Filtern ausstatten. Anwendungsbeispiele wären Wochen- und Monatsangebote oder beliebteste Artikel.

### Kategorien ###

Kategorien können dynamisch mit Artikeln befüllt werden. Verwenden Sie hierzu in OXID unter Artikel verwalten > Kategorien > den Tab __"Dynamische Artikelwahl"__, unter welchem Sie die in der OXSEARCH-Konfiguration definierten Filter mit Werten belegen können. Die verwendbaren Filter müssen dafür nicht einmal aktiv sein, sondern lediglich mit einem eindeutigen Parameternamen versehen werden. Unter dem Tab __Sichtbare Filter__ können sie zusätzlich auswählen, welche Filter für die jeweilige Kategorie sichtbar sein sollen.
Beispiel für sinnvolle dynamische Kategorie wären Geschenkartikel oder Wochen/Monatsangebote.

### Mehrsprachigkeit ###

Für die Mehrsprachigkeit Ihres Shops empfiehlt es sich, eine Sprachvariable zu definieren. Wenn ein entsprechender Filter auf die Variable gesetzt ist, wird bei mehrsprachigen Varianten einer Seite oder eines Artikels die richtige angezeigt, im Frontend sieht der Nutzer in jedem Artikel per default die in seiner Sprache verfügbare Version. Jede Sprache verfügt dabei über ihren eigenen Index und muss separat gepflegt werden. Bei der Artikelpflege müssen Sie also immer auswählen, in welcher Sprache der Artikel editiert werden soll. Dies geschieht mit Hilfe einer Dropdown-Liste.

### Mandantenfähigkeit ###

OXSEARCH ist mandantenfähig. Bitte beachten Sie, dass Sie für den Fall, dass Sie Subshops pflegen, OXSEARCH für jeden Subshop separat installieren und konfigurieren müssen. Um die Funktionalität des Boostings zu garantieren, muss jeder Subshop seinen eigenen Index erhalten.

### Nutzung von jQuery ###

OXSEARCH verwendet für die Autosuggestion und einige Filter-Templates die Bibliotheken [jQuery](http://jquery.com) und [jQueryUI](http://jqueryui.com). Diese sind bei OXID bereits enthalten. Möchten Sie deren Einbindung verhindern z.B. weil Sie neuere Versionen oder ganz andere Bibliotheken verwenden wollen, müssen Sie in Ihrem Theme lediglich die Datei `widget/header/autosuggestion.tpl` anlegen, z.B. mit folgendem Inhalt:

    [{if $oViewConf->isActivated('autosuggest')}]
        <script type="text/javascript">
            var source = '[{$oViewConf->getAutosuggestionLink()}]';
        </script>
        [{oxstyle include=$oViewConf->getModuleUrl('marm/oxsearch','out/src/css/autosuggest.css')}]
        [{oxscript include=$oViewConf->getModuleUrl('marm/oxsearch','out/src/js/autosuggest.js')}]
    [{/if}]

Auf diese Weise kann das von OXSEARCH bereitgestellte Autosuggestion-Script verwendet werden, ohne weitere kopien von jQuery einzubinden.

####  Eigene Autosuggestion ####

Natürlich kann in `widget/header/autosuggestion.tpl` auch Ihr eigenes Autosuggestion-Script eingebunden werden.  Wichtig ist dafür die Funktion `[{$oViewConf->getAutosuggestionLink()}]` , welche eine gesäuberte und protokollunabhängige URL auf dem Autosuggestions-Controller von OXSEARCH liefert. Zur Übertragung des Suchbegriffs müssen Sie lediglich den Parameter `&term=` anfügen.

#### Eigene Tracking-Pixel ####

OXSEARCH erlaubt den Einbau eigener Tracking-Pixel. Dazu muss einfach das Template `widget/tracking.tpl` mit folgenden Parametern
eingebunden werden:

 * `key` ist der nach der Trackingvariable, z.B. `requested`.
 * `product` ist eine oxArticle-Instanz.
 * `delay` ist eine Optionale Verzögerung in Milliksekunden.

Statt `product` können auch die Parameter `type` und `id` angeben werden, um ein beliebiges anderes Objekt zu tracken.
Wird mehr Flexibilität bezüglich der AJAX-Einbindung gewünscht kann auch mittel der Methode `$oViewConf->getTrackingUrl($type, $id, $key, $force = false)`
eine Pixel-URL generiert werden.

#### Sicherung des Trackingpixel ####

Das Tracking-Pixel ist durch eine einfache Parameterverschlüsselung gegen unbefugte Dateneinschleusung gesichert. Der dafür
verwendete Salt lässt sich mit der Option `$this->sOxSearchTrackingPixelSalt ='NEUER SALT';` in der `config.inc.php` überschreiben.
Mittels `$this->blOxSearchTrackingPixelNoSalt = true;` lässt sich die Verschlüsselung auch ganz abschalten.
