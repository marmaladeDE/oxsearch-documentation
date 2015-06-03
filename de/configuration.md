# OXSEARCH-Setup

Um OXSEARCH erfolgreich in OXID einzubinden, m�ssen nun noch einige wesentliche Einstellungen im OXSEARCH-Setup vorgenommen werden. Das Setup besteht aus vier Reitern mit folgenden Einstellungsm�glichkeiten:  

- Im ersten Reiter OXSEARCH-Setup finden Sie Grund-, Such- & Filtereinstellungen.  
- Im zweiten Reiter k�nnen Sie Boosting & Synonyme konfigurieren.  
- Im dritten Reiter werden die Zugangsdaten zu Elasticsearch hinterlegt & die Indizierung gestartet.  
- Im vierten Reiter werden die Suchbegriffe fehlgeschlagener Suchen aufgelistet. 

![Moduleinstellungen f�r OXID](img/oxsearch_setup.png)  

## Elasticsearch-Verbindungsdaten und Indizierung

Bevor Sie OXSEARCH nutzen k�nnen ist es erforderlich, die Verbindungsdaten f�r Elasticsearch zu hinterlegen und die Indizierung zu starten.  

1. Wechseln Sie in den Reiter "Elasticsearch Konfiguration > Verbindungsdaten".  
2. Tragen Sie den Host in der Form [USER:PASSWORT@]ES-DOMAIN_ODER_IP ein.  
3. Legen Sie den Port fest, auf dem Elasticsearch auf Anfragen horcht, meistens ist das 9200.  
4. Speichern Sie zun�chst die eingetragenen Verbindungsdaten, bevor Sie mit dem n�chsten Schritt fortfahren.  
5. �ffnen Sie erneut die Verbindungseinstellungen zu Elasticsearch.  
6. Definieren Sie einen eindeutigen Indexnamen f�r den aktiven und den passiven Index.  
__Hinweis: der Indexname kann f�r beide Indizes gleich sein, muss aber unbedingt klein geschrieben werden, da sonst eine Fehlermeldung "Invalid JSON" auftritt.__  
Auf dem aktiven Index findet im Shop-Frontend die Suche statt, w�hrend der passive Index allein zur Indexerstellung ben�tigt wird. Bei Bedarf k�nnen die Indizes auf Knopfdruck getauscht werden. 

![Verbindungsdaten und Indexerstellung](img/oxsearch_elastic_search_config.png)

__Hinweis: Elasticsearch sollte mindestens mit einer "basic authorization" konfiguriert werden, um Manipulationen des Shop-Bestands von au�en zu vermeiden!___

## Datenimport

Der Datenimport kann auf drei verschiedene Arten durchgef�hrt werden.  
- Im Backend durch dr�cken der Schaltfl�che "Index jetzt neu aufbauen" im Reiter Elasticsearch-Konfiguration  
- mittels eines Cronjobs, der die Skriptdatei `<ShopRoot>/modules/marm/oxsearch/importer/doImport.sh` ausf�hrt  
- mit Hilfe der Update- & Delete-API  

Wenn Sie die Schaltfl�che im Backend benutzen, wirken sich vorgenommene �nderungen sofort aus. Je nach Datenmenge, kann der Neuaufbau einige Zeit in Anspruch nehmen und die Konfigurationsseite muss w�hrend des Indexaufbaus ge�ffnet bleiben, ansonsten bricht der Indexaufbau ab. 
Das Update durch den Cronjob geschieht im Hintergrund und bedarf keiner aktiven Ausf�hrung, genau wie die Schaltfl�che "Index jetzt neu aufbauen" im Backend handelt es sich hier um einen Vollimport, der in Abh�ngigkeit zur Datenmenge einige Zeit in Anspruch nehmen kann.  
Die Update- und Delete-API bietet sich an, wenn gezielte Aktualisierungen vorgenommen werden sollen. 

### Update- & Delete-API

Die Klasse marmOxsearchImport besitzt einige Methoden zur Aktualisierung und L�schung von Artikeln:

- `updateArticle($articleId, $language = 0, $index = 'active')` aktualisiert einen einzelnen Artikel.  
- `updateArticles($articleIds, $language = 0, $index = 'active')` ist das �quivalent f�r eine Liste von Artikeln.  
- `deleteArticle($articleId, $index = 'active')` l�scht einen Artikel.  
- `deleteArticles($articleIds, $index = 'active')` l�scht mehrere Artikel. 

Der optionale Parameter $language w�hlt die Sprache anhand der Sprachnummern von OXID, w�hrend $index die Wahl zwischen aktivem (active) und inaktiven (inactive) Index erlaubt.

Verwendungsbeispiel:
- `$sOxid = oxRegistry::getConfig()->getRequestParameter('oxid');`
- `oxRegistry::get('marmOxsearchImport')->updateArticle($sOxid);`

__Hinweis: Die Listenmethoden sind mit Bedacht zu verwenden, da sie keine Begrenzung enthalten und gro�e Artikelzahlen zu Speicher- oder Laufzeit�berschreitungen f�hren k�nnen.__  

## Reiter OXSEARCH-Setup

Die Grundeinstellungen von OXSEARCH sind in folgende Abschnitte unterteilt:  
- Grundeinstellungen f�r wesentliche Konfigurationsm�glichkeiten von OXSEARCH  
- Sucheinstellungen zur Festlegung von Suchkriterien  
- Produktfilter  

### Grundeinstellungen

![OXSEARCH-Setup](img/oxsearch_setup.png)

- Aktivierung des Moduls 
- Listenansicht: legt fest, ob f�r Kategorielisten der OXID- oder OXSEARCH-Standard verwendet wird. Erst bei gesetztem Haken profitieren Sie von vorgenommenen Filtereinstellungen.  
- Suche: Hier entscheiden Sie, ob f�r die Suche die Einstellungen von OXID verwendet oder ob die Filtereinstellungen von OXSEARCH genutzt werden. Nur bei Aktivierung des Kontrollk�stchens kommen in OXSEARCH vorgenommene Filtereinstellungen zur Geltung.  
__Hinweis: Diese Option erfordert eine erneute Indizierung!__ 
- Autosuggestion unterbreitet w�hrend der Sucheingabe Vorschl�ge zu Suchbegriffen  
- Seitennummer an Artikel-URL anh�ngen: Bei mehrseitigen Listen wird ab Seite 2 die Seiten-Nummer in der URL hinterlegt. 
- Aktuelle Promotion �berschreibt Kategoriesortierung: Diese Einstellung nimmt Einfluss darauf, ob die Sortierung innerhalb einer Kategorie von der aktuellen Promotion ersetzt wird. Mit der Promotion von Artikeln k�nnen Sie Einfluss darauf nehmen, ob ein bestimmter Artikel oder eine Gruppe von Artikel ganz vorne oder weiter hinten in der Liste angezeigt wird. Promotionen k�nnen Sie in OXID unter Kundeninformation > Aktionslisten oder direkt am Artikel unter Artikel verwalten > Artikel > Reiter Erweitert (Boost-Wert) pflegen.  
- Elternkategorien enthalten alle Produkte ihrer Kinder: Wenn Ihr Shop beispielsweise eine Kategorie "Computer und Zubeh�r" enth�lt, die aus den Unterkategorien "Computer" und "Zubeh�r" besteht, k�nnen Sie hier beeinflussen, ob bei der Auswahl der Kategorie alle zugeordneten Artikel der Unterkategorien angezeigt werden oder ob der Kunde sich erst f�r eine der genannten Unterkategorien entscheiden muss.  
__Hinweis: Diese Option erfordert eine neue Indizierung!__ 
- Artikel beim Speichern erneut indizieren: Wenn die Indizierung der Artikel durch einen ChronJob automatisch erfolgt, ist diese Option optional.  
- oxLocator f�r Suchergebnis-Detailseiten abschalten: Der oxLocator f�gt den Detailseiten der Artikel Navigationslinks hinzu, mit deren Hilfe man zwischen den Artikeln hin- und herspringen kann (u. a. vorheriger Artikel, n�chster Artikel, etc.).  
___Hinweis: Der aktivierte oxLocator wirkt sich negativ auf die Performance aus.___
- Trennzeichen f�r Attribute mit mehreren Wertauspr�gungen: Wenn ein Artikel beispielsweise aus mehreren Materialien besteht, besteht das Attribut "Material" aus zwei Wertauspr�gungen. Das Trennzeichen stellt sicher, dass beide Materialien f�r den betreffenden Artikel gespeichert werden.  

### Sucheinstellungen

In den Sucheinstellungen k�nnen Sie Suchfilter festlegen. Alle diese Ergebnisse wirken sich auf die Suchergebnisse aus.  
- Gewichtete Suchfilter: Hier werden die Wertigkeiten f�r Suchfelder angegeben. Ein klassischer Anwendungsfall w�re, dass die Artikeltitel mit einer Wertigkeit von 10 h�her gewichtet wird als der Suchbegriff in der Kurzbeschreibung, die beispielhaft mit 3 angegeben wird.  
- Attribute Durchsuchen: Attribute sind Eigenschaften, die ein Artikel enthalten kann (Verschiedene Gr��en, Farben, etc.). Ist diese Option aktiviert, k�nnte der Endkunde im Shop-Frontend beispielsweise Suchanfragen mit Attributsauspr�gungen `blau` ausl�sen.  
- Varianten durchsuchen: Die Variante eines Artikels hat immer einen Elternartikel, von dem die Variante erbt. Im Fall von Bekleidung in verschiedenen Gr��en w�rde das bedeuten, dass es einen Artikel f�r ein bestimmtes Kleidungsst�ck gibt und die Varianten (verschiedene Farben und Gr��en) die jeweiligen Kindartikel sind. Im Frontend wird nur ein Artikel angezeigt und innerhalb des Artikels kann der Kunde mittels einer Dropdown-Liste die pr�ferierte Variante w�hlen.  
- Zus�tzliche Fuzzy-Suche: Die Fuzzy-Suche korrigiert Tippfehler wie Buchstabendreher. Mindestens 30 % des Wortes m�ssen dabei richtig geschrieben sein. 
- Wildcard-Suche: Hier k�nnen Sie entscheiden, ob f�r die Suche auch Platzhalter genutzt werden k�nnen. Wildcards sind sinnvoll, wenn der Name eines Artikels aus zusammengesetzten W�rtern besteht. Je nachdem, aus wie Vielen W�rtern der Begriff zusammen gesetzt ist, werden betroffene Artikel nicht immer zuverl�ssig gefunden. "Kiteboard" im OXID Demoshop besteht nur aus zwei zusammen gesetzten W�rtern und w�rde auch ohne Wildcard-Suche gefunden werden, wenn es jetzt aber "Kiteboard-H�llen" gibt, w�rden ohne Wildcard-Suche haupts�chlich alle Treffer mit "Kiteboard" und "H�lle" ausgegeben werden.  
___Hinweis: Die Wildcard-Suche wirkt sich negativ auf die Performance aus!___  
- Bei Einzelergebnissen nicht zum Artikel weiterleiten: Mit dieser Option beeinflussen Sie, ob im Falle eines einzigen Treffers auf die Suchergebnisseite oder direkt zum Artikel weitergeleitet wird. 
- Zeige Suchvorschl�ge: W�hrend der Sucheingabe wird mit Hilfe von Autosuggestion der Suchbegriff vervollst�ndigt.  
- OXSEARCH-Keys: Hier legen Sie fest, welche Felder (Artikelbezeichnung, Kurzbeschreibung, Langbeschreibung, etc.) in die Suche einbezogen werden sollen.
- Zeige gefundene Kategorien: Bei aktivierter Option sind in der Trefferliste im Frontend alle Kategorien sichtbar, in denen Treffer gefunden wurden.  
- Zeige gefundene Hersteller listet gefundene Hersteller des gesuchten Artikels  
- Zeige gefundene Links: Ist diese Option aktiviert, kann man im Shop beispielsweise Zahlungs- und Versandbedingungen oder �ffnungszeiten suchen.  
- Cutoff-H�ufigkeit: Hier k�nnen Sie einen absoluten oder relativen Wert festlegen, der h�ufig auftauchenden W�rtern wie "der", "die", "das" eine geringere Relevanz zuordnet und in einem Subquerry verarbeitet, damit die Trefferliste m�glichst exakte Ergebnisse liefert.  
- Preisbereich f�r Suchergebnisse (von ... bis ...): Hier k�nnen Sie die angezeigten Artikel auf den von Ihnen gew�nschten Preisbereich beschr�nken. Dies ist sehr hilfreich, wenn Sie beispielsweise kostenlose Artikel, wie Dreingaben aus den Suchergebnissen ausschlie�en wollen.
- Abfrage-Verkn�pfung: In dieser Select-Box w�hlen Sie aus, ob alle oder mindestens einer der abgefragten Kriterien zutreffen.  

### Produktfilter

In diesem Reiter k�nnen Sie Produktfilter konfigurieren.
- SEO-URLS f�r gefilterte Seiten verwenden: Wenn diese Option deaktiviert ist, werden alle Filter in Form von Parametern an die URL angeh�ngt. Je nach gew�hlten Filtern kann die L�nge der URL zu Problemen f�hren, so dass die gew�nschte Seite nicht angezeigt werden kann.
- Anzahl gefundener Artikel am Filterwert anzeigen: Ist diese Option aktiviert, wird im Frontend sichtbar, wie viele Artikel f�r den aktivierten Filter gefunden wurden.
- Selektierte Filter nicht nach vorne sortieren: in einer Multiselectbox werden ausgew�hlte Filter nicht nach vorne sortiert, sondern in der urspr�nglichen Filtersortierung angezeigt. Je nach Anzahl der Elemente in der Selektbox bedeutet dies bei nicht gesetztem Haken, dass Sie scrollen m�ssen, um die selektierten Filter zu finden.  
- Kategoriefilter aktivieren: In der Ergebnisliste werden die Kategorien angezeigt, die den Suchbegriff enthalten.  
- Trennzeichen f�r dynamische Kategorien: Dynamische Kategorien werden von Elasticsearch aus bestehenden Artikeln bef�llt. Die Beispielkategorie "Geschenkartikel unter 100 EUR" k�nnte alle Artikel unter 100 EUR enthalten. Das Trennzeichen f�r dynamische Kategorien muss sich von dem f�r statische Kategorien unterscheiden.  
*** Stimmt so nicht, wie erfolgt die Mehrsprachige Pflege? *** 
- Attribute: Hier k�nnen Sie Attribute f�r Artikel wie Farbe und Gr��e bei Bekleidung definieren, auf die OXSEARCH filtern kann. F�r den Fall, dass Sie einen mehrsprachigen Shop pflegen, k�nnen Sie hier einen in der Datenbank festzulegenden Identifier hinterlegen, der f�r die �bersetzung der Feldnamen auf die Sprachdatei der von Ihnen gew�nschten Sprache verweist. Die Sprachdateien f�r mehrsprachige Shops sind im OXID-Verzeichnis unter 
	/application/views/azure/[Sprachverzeichnis]	
in lang.php zu pflegen.  
- Artikel: Hier k�nnen Sie Filter auf Artikelinformationen wie Preis und Gewicht setzen.  
- Skript: Hier k�nnen Sie Scripts erstellen, die mehrere Werte pr�fen und im Shop beispielsweise als Sonderposten, Saisonartikel, etc. angezeigt werden.  

## Reiter Synonyme und suchbare Links

Der n�chste Punkt in den OXSEARCH-Einstellungen befasst sich mit Synonymen und suchbaren Links. Er ist in folgende Abschnitte unterteilt:  
- dynamisches Boosting  
- Suchbare Links  
- Synonyme  

### Dynamisches Boosting

Das dynamische Boosting erm�glicht es, die Position von Produkten in der Kategorieansicht oder bei Suchergebnissen zu beeinflussen. Dies geschieht anhand sich dynamisch �ndernder Werte wie Verkaufszahlen oder Detailseitenaufrufe. Sie k�nnen hier die Faktoren festlegen, mit denen die einzelnen Statistiken gewichtet werden.  
![Dynamisches Boosting](img/oxsearch_dynamisches_boosting.png)  
- Dynamisches Boosting aktivieren: Wenn das dynamische Boosting deaktiviert ist, profitieren Sie nicht von den weiter unten festgelegten Wichtungskriterien.  
- Auf Kategorien anwenden: Wenn Sie sich f�r diese Option entscheiden, wird die Kategoriesortierung verworfen.  
- Boostfaktor Anzahl der Verk�ufe  
- Boostfaktor in den Warenkorb gelegt  
- Boostfaktor Anzahl der Detailseitenaufrufe  
- Boostfaktor Promotionen: Dies ist ein vorgeschlagener Defaultwert von OXID. Ihren W�nschen entsprechend k�nnen Sie dieses Feld beliebig konfigurieren, beispielsweise nach Anzahl der Verk�ufe.  
- Relevanzexponent: Mit ihm legen Sie fest, mit welchem Faktor die Wichtungen potenziert werden. Dies dient zur Abwertung von unscharfen Treffern im Suchergebniss.
Folgende Formel wird angewandt:  
Boostfaktor Wert multipliziert mit Relevanz potenziert mit Relevanzexponent  
Die Werte eines Artikels werden dabei addiert.
__Hinweis: Wir empfehlen folgende Werte:  
- In den Warenkorb gelegt: 10  
- Anzahl Detailseitenaufrufe: 1  
- Anzahl Verk�ufe: 100  __
- Debug-Modus aktivieren: Wenn der Administrator im Frontend eingeloggt ist sehen Sie, wie sich Ihre Gewichtungskriterien auf eine Suche oder Kategorieliste auswirken.  

### Suchbare Links

Hier legen Sie fest, welche Links in der Suche aufgef�hrt werden sollen, wenn diese in den Sucheinstellungen aktiviert sind. Beispiele w�ren Zahlungs- und Versandbedingungen oder �ffnungszeiten, die auch in der Autosuggestion auftauchen.  
![Suchbare Links](img/suchbare_links.png)  
Folgende Angaben werden f�r einen Link ben�tigt:  
- Titel  
- extern  
- Seite  
___Hinweis: f�r die suchbaren Links m�ssen mehrsprachige Versionen einer Seite explizit festgelegt werden, f�r die AGB-Seite m�ssten Sie beispielsweise auch die terms of service-Seite auff�hren!  
Jeder suchbare Link ben�tigt mindestens ein Schl�sselwort, sonst wird er ohne Fehlermeldung verworfen!___  

### Synonyme ###

Hier k�nnen Sie Synonyme pflegen.  
![Synonyme](img/oxsearch_synonyme.png)  
Wenn die Statistik der Suchanfragen ohne Ergebnisse zeigt, dass "Softdrinks" keine Treffer ergeben, k�nnen Sie "alkoholfreie Getr�nke" als Synonym festlegen, damit "Softdrinks" k�nftig zum gew�nschten Ergebnis f�hrt.  
___Hinweis: Die Festlegung von Synonymen erfordert eine Aktualisierung des Index!___  

### Suchanfragen ohne Ergebnis ###

Diese Seite zeigt Ihnen an, welche Suchanfragen ohne Ergebnis blieben.  
![Suchanfragen ohne Ergebnis](img/oxsearch_suchanfragen_ohne_ergebnis.png) 
Bei der Auswertung bietet es sich an, Synonyme zu pflegen, die mit den ergebnislosen Suchanfragen verwandt sind.  

# Anmerkungen # 
## Landing-Pages und Promotionen ## 

Landing-Pages und Promotionen lassen sich �ber das Aktionen-Interface von OXID anlegen und mit dynamisch anzuwendenden Filtern ausstatten. Anwendungsbeispiele w�ren Wochen- und Monatsangebote oder beliebteste Artikel.  

## Kategorien ## 

Kategorien k�nnen dynamisch mit Artikeln bef�llt werden. Verwenden Sie hierzu in OXID unter Artikel verwalten > Kategorien > den Tab __"Dynamische Artikelwahl"__, unter welchem Sie die in der OXSEARCH-Konfiguration definierten Filter mit Werten belegen k�nnen. Die verwendbaren Filter m�ssen daf�r nicht einmal aktiv sein, sondern lediglich mit einem eindeutigen Parameternamen versehen werden. Unter dem Tab __Sichtbare Filter__ k�nnen sie zus�tzlich ausw�hlen, welche Filter f�r die jeweilige Kategorie sichtbar sein sollen.  
Beispiel f�r sinnvolle dynamische Kategorie w�ren Geschenkartikel oder Wochen/Monatsangebote. 

## Mehrsprachigkeit ##

F�r die Mehrsprachigkeit Ihres Shops empfiehlt es sich, eine Sprachvariable zu definieren. Wenn ein entsprechender Filter auf die Variable gesetzt ist, wird bei mehrsprachigen Varianten einer Seite oder eines Artikels die richtige angezeigt, im Frontend sieht der Nutzer in jedem Artikel per default die in seiner Sprache verf�gbare Version. Jede Sprache verf�gt dabei �ber ihren eigenen Index und muss separat gepflegt werden. Bei der Artikelpflege m�ssen Sie also immer ausw�hlen, in welcher Sprache der Artikel editiert werden soll. Dies geschieht mit Hilfe einer Dropdown-Liste.  

## Mandantenf�higkeit ##

OXSEARCH ist mandantenf�hig. Bitte beachten Sie, dass Sie f�r den Fall, dass Sie Subshops pflegen, OXSEARCH f�r jeden Subshop separat installieren und konfigurieren m�ssen. Um die Funktionalit�t des Boostings zu garantieren, muss jeder Subshop seinen eigenen Index erhalten.  

## Support von OXID-Standardfunktionalit�ten ##

Da OXSEARCH auf OXID basiert, werden die meisten Standardfunktionen von OXID unterst�tzt. Die folgenden wenigen Funktionen werden nicht unterst�tzt:  
- Rollen und Rechte im Frontend  
- Varnish  
Bei Bedarf k�nnen nicht funktionierte Grundfunktionen jedoch projektspezifisch angepasst werden.  