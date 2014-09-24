# Grundlagen und Installation

## Was ist jQuery?

jQuery ist eine freie JavaScript-Bibliothek, die mittlerweile nahezu in allen dynamischen Webseiten verwendet wird.
Diese kann daher mittlerweile als Standard-Bibliothek für JavaScript gesehen werden.
Diese Bibliothek enthält Funktionen, die dem Entwickler den Umgang mit typischen Aufgaben der dynamischen Web-Programmierung abnehmen bzw. zumindest deutlich erleichtern können:

* Elemente aus dem DOM-Baum (also der dargestellten HTML-Seite) selektieren sowie
* ausgewählte Elemente manipulieren (Farbe, Größe, Inhalt, ...).
* Event-Behandlung für sämtliche Objekte im DOM-Baum,
* Animationen und Effekte sowie
* asynchron externe Daten abfragen und integrieren (AJAX).

## Installation

Um jQuery zu verwenden, muss in der HTML-Seite einfach nur die entsprechende JavaScript-Datei eingebunden werden.
Dazu kann diese entweder lokal gespeichert und eingebunden werden, oder aber aus einem Content Delivery Network (CDN) bezogen werden.
Die Einbindung über ein CDN hat einige Vorteile, daher sollte dieser Variante meist der Vorzug gegeben werden.
Jedoch gibt es auch Gründe die dagegen sprechen (z.B. die Notwendigkeit Online zu sein, eventuelle Sicherheitsbedenken).
Dennoch ist davon auszugehen, das die Vorteile überwiegen:

* jQuery wurde bereits vom Browser geladen, da auch andere Seiten jQuery aus dem gleichen CDN laden, und der Browser die Daten zwischengespeichern kann.
* Der Ladevorgang ist schneller, da die Bibliothek parallel zum eigenen Seiteninhalt geladen werden kann.
  Oft ist auch die Quelle aus einem CDN näher als der eigene Server, da CDN geografisch verteilte Serverfarmen nutzen.
* Die Daten werden aus dem CDN geladen, somit wird kein lokaler Netzwerkverkehr erzeugt; dies spart lokale Bandbreite und somit oft auch Kosten.

### jQuery lokal

jQuery wird von der [Download-Seite](http://jquery.com/download/ "jQuery Download Seite") heruntergeladen und lokal gespeichert.
Es gibt dabei verschiedene Versionen (1.x und 2.x, wobei der wesentliche Unterschied ist, das 2.x ältere Versionen des Internet Explorers nicht mehr unterstützt) und Varianten (Production und Development, wobei diese inhaltlich identisch sind, lediglich enthält die Development-Version eine lesbare Formatierung und Kommentare und ist dadurch auch deutlich größer.

Um die lokal gespeicherte Version der Bibilothek in eine Seite zu inkludieren, muss diese einfach über das Skript-Tag in dieser Seite geladen werden.
In diesem Beispiel wurde die minimalisierte Version (d.h. die Production-Version) von jQuery in der Version 1.11.1 heruntergeladen, daraus ergibt sich auch der Dateiname `jquery-1.11.1.min.js`.

```HTML
<!doctype html>
<html>
	<head>
		<title>jQuery Testseite</title>
		<script src="jquery-1.11.1.min.js"></script>
	</head>

	<body>
		<h1>jQuery Testseite</h1>
	</body>
</html>
```

### jQuery aus einem CDN


# Einführung in Selektoren und DOM-Manipulation

# Abrufen und Einfügen von Web-Content (AJAX)

