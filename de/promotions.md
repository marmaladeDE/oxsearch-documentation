## Promotionen ##

Um bestimmte Artikel, Kategorien oder Hersteller besonders hervorzuheben, können Sie in OXID unter `Kundeninformationen->Aktionen verwalten` Promotionen anlegen. Promotionen werden bereits existierende Filter angewandt.

Für die Erstellung von Promotionen ist folgendes zu beachten:  
- Die Groß- und Kleinschreibung von Attributen oder Kategorien ist generell zu beachten.  
- Sofern mehrere Attribute in der Promotion auftauchen, darf kein Leerzeichen zwischen Attributnamen und Trennzeichen vorkommen. Die Syntax lautet immer: `Kategorie_1,Kategorie_2`.  
- Für den Fall, dass die Promotion dynamische Kategorien enthält, __muss__ die 32stellige Kategorie-ID verwendet werden. Diese können Sie mit Entwicklertools wie Firebug oder Developer aus dem selektierten Link der jeweiligen Kategorie entnehmen.  
- Bei Verwendung mehrerer Wertausprägungen wird "und" als Verknüpfungsoperator verwendet.
