## Promotionen ##

Um bestimmte Artikel, Kategorien oder Hersteller besonders hervorzuheben, können Sie in OXID unter `Kundeninformationen->Aktionen verwalten` Promotionen anlegen. Promotionen werden bereits existierende Filter angewandt.

Für die Erstellung von Promotionen ist folgendes zu beachten:  
- Die Groß- und Kleinschreibung von Attributen oder Kategorien ist generell zu beachten.  
- Sofern Ihre Promotion aus mehreren Attributen besteht, darf kein Leerzeichen zwischen Attributnamen und Trennzeichen vorkommen. Die Syntax lautet immer: `Attribut_1,Attribut_2`.  
__Hinweis: Eine Promotion kann aus mehreren Attributen, auch mit mehreren Wertausprägungen, bestehen, niemals aber aus mehr als einer Kategorie!__
- Für den Fall, dass die Promotion dynamische Kategorien enthält, __muss__ die 32stellige Kategorie-ID verwendet werden. Diese können Sie mit Entwicklertools wie Firebug oder Developer aus dem selektierten Link der jeweiligen Kategorie entnehmen.  
- Bei Verwendung mehrerer Wertausprägungen wird "und" als Verknüpfungsoperator verwendet.
