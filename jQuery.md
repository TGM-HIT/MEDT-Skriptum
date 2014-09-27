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

Wie oben erwähnt kann jQuery auch direkt aus einem Content Delivery Network eingebunden werden.
Dazu muss nur das `src` Attribut des `script`-Tags aus dem obigen Beispiel durch die entsprechende URL ersetzt werden.
jQuery wird in verschiedenen CDNs geführt, welches davon verwendet wird ist prinzipiell egal.

Hier die entsprechenden URLs für verschiedene Content Delivery Networks (MaxCDN ist das offizielle Content Delivery Network von jQuery, diesem sollte also Vorzug gegeben werden):

* MaxCDN: `//code.jquery.com/jquery-1.11.1.min.js`
* Google CDN: `//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js`
* Microsoft CDN: `http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js`
* CloudFlare CDN: `//cdnjs.cloudflare.com/ajax/libs/jquery/1.11.1/jquery.min.js`
* jsDelivr CDN: `//cdn.jsdelivr.net/jquery/1.11.1/jquery.min.js`

Gegebenenfalls ist bei der URL vorne noch das Protokoll (`http`) anzuführen, falls während der Entwicklung über `file:` auf die Dateien zugegriffen werden sollte.
Bei einer Entwicklung über einen Webserver wird das Protokoll vom Web-Browser automatisch korrekt ergänzt.

Hier noch ein Beispiel mit der entsprechenden Einbindung von jQuery über das offizielle Content Delivery Network:

```HTML
<!doctype html>
<html>
	<head>
		<title>jQuery Testseite</title>
		<script src="//code.jquery.com/jquery-1.11.1.min.js"></script>
	</head>

	<body>
		<h1>jQuery Testseite</h1>
	</body>
</html>
```


# Einführung in Selektoren und DOM-Manipulation

Für die folgenden Beispiele wird das obige HTML-Grundgerüst als Grundlage genommen, d.h. eine quasi leere HTML-Datei, die lediglich jQuery lädt und eine einzelne Überschrift (`h1`) enthält.

## Selektoren (Grundlagen)

Selektoren in jQuery dienen dazu, spezifische Elemente aus dem DOM abzurufen.
Das [Selektor Kapitel](http://api.jquery.com/category/selectors/ "Selectors | jQuery API Documentation") aus der [jQuery API Dokumentation](http://api.jquery.com) enthält eine Vielzahl von möglichen Selektoren.
Hier sollen nur die einfachsten Varianten aufgeführt werden, weitere Details können jederzeit in der Dokumentation nachgelesen werden bzw. werden in diesem Dokument bei Bedarf behandelt.

Der einfachste Selektor besteht einfach aus einem Tag und sieht wie folgt aus: `$("tag")`, wobei `tag` jedes beliebige Tag aus der zugehörigen HTML-Seite (bzw. dem DOM) sein kann.

Im obigen Beispiel würde zum Beispiel der Selektor `$("h1")` die Überschrift zurückliefern (wobei die genaue Ausgabe je nach Browser unterschiedlich aussehen kann):

```
> $("h1")
< [<h1>jQuery Testseite</h1>]
```

Falls die Seite mehr als ein Element enthält, das dem entsprechenden Selektor entspricht, so werden alle Elemente zurückgeliefert.
Mit den zusätzlichen Optionen `:first` und `:last` kann spezifisch auf das erste oder letzte Element des Typs zugegriffen werden.
Soll auf das n-te Element zugegriffen werden, so muss die zusätzliche Option `:eq(n)` verwendet werden (wobei wie in der Informatik üblich mit 0 begonnen wird).

Der Selektor um auf die erste Überschrift der Seite zuzugreifen sieht also wie folgt aus: `$("h1:first")` oder aber `$("h1:eq(0)")`.

Zusätzlich zu den Tags kann auch auf HTML-Klassen oder IDs zugegriffen werden, wobei hier die gleiche Syntax wie in CSS verwendet wird.
Um also alle Elemente einer Klasse zu erhalten, wird der Selektor `$(".class")` verwendet.
Da IDs eindeutig sein sollten, sollte auch der Selektor `$("#id")` jeweils nur das Element mit der entsprechenden ID zurückliefern.

Weiters lassen sich Selektoren durch Beistriche kombinieren, wobei das Ergebnis dann nur jene Elemente sind, die alle Selektoren erfüllen.

## DOM-Manipulation (Grundlagen)


# Abrufen und Einfügen von Web-Content (AJAX)

