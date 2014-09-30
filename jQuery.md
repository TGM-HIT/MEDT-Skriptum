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

Für die folgenden Beispiele wird ein einfaches HTML-Grundgerüst als Grundlage genommen.
Dieses enthält lediglich mehrere Überschriften (`h1`) sowie Absätze (`p`):

```HTML
<!doctype html>
<html>
	<head>
		<title>jQuery Testseite</title>
		<script src="jquery-1.11.1.min.js"></script>
	</head>

	<body>
		<h1>Erste &Uuml;berschrift</h1>
		<p>Erster Absatz</p>
		<h1>Zweite &Uuml;berschrift</h1>
		<p>Zweiter Absatz</p>
		<h1>Dritte &Uuml;berschrift</h1>
		<p>Dritter Absatz</p>
	</body>
</html>
```

## Selektoren (Grundlagen)

Selektoren in jQuery dienen dazu, spezifische Elemente aus dem DOM abzurufen.
Das [Selektor Kapitel](http://api.jquery.com/category/selectors/ "Selectors | jQuery API Documentation") aus der [jQuery API Dokumentation](http://api.jquery.com) enthält eine Vielzahl von möglichen Selektoren.
Hier sollen nur die einfachsten Varianten aufgeführt werden, weitere Details können jederzeit in der Dokumentation nachgelesen werden bzw. werden in diesem Dokument bei Bedarf behandelt.

Der einfachste Selektor besteht einfach aus einem Tag und sieht wie folgt aus: `$("tag")`, wobei `tag` jedes beliebige Tag aus der zugehörigen HTML-Seite (bzw. dem DOM) sein kann.

Im obigen Beispiel würde zum Beispiel der Selektor `$("h1")` die Überschrift zurückliefern (wobei die genaue Ausgabe je nach Browser unterschiedlich aussehen kann):

```
> $("h1")
< [<h1>Erste Überschrift</h1>, <h1>Zweite Überschrift</h1>, <h1>Dritte Überschrift</h1>]
```

Falls die Seite mehr als ein Element enthält, das dem entsprechenden Selektor entspricht, so werden alle Elemente zurückgeliefert.
Mit den zusätzlichen Optionen `:first` und `:last` kann spezifisch auf das erste oder letzte Element des Typs zugegriffen werden.
Soll auf das n-te Element zugegriffen werden, so muss die zusätzliche Option `:eq(n)` verwendet werden (wobei wie in der Informatik üblich mit 0 begonnen wird).

Der Selektor um auf die erste Überschrift der Seite zuzugreifen sieht also wie folgt aus: `$("h1:first")` oder aber `$("h1:eq(0)")`.

```
> $("h1:first")
< [<h1>Erste Überschrift</h1>]
> $("h1:eq(1)")
< [<h1>Zweite Überschrift</h1>]
> $("h1:last")
< [<h1>Dritte Überschrift</h1>]
```

Zusätzlich zu den Tags kann auch auf HTML-Klassen oder IDs zugegriffen werden, wobei hier die gleiche Syntax wie in CSS verwendet wird.
Um also alle Elemente einer Klasse zu erhalten, wird der Selektor `$(".class")` verwendet.
Da IDs eindeutig sein sollten, sollte auch der Selektor `$("#id")` jeweils nur das Element mit der entsprechenden ID zurückliefern.

Weiters lassen sich Selektoren durch Beistriche kombinieren, wobei das Ergebnis dann nur jene Elemente sind, die alle Selektoren erfüllen.

## DOM-Manipulation (Grundlagen)

Elemente aus dem DOM, die man über einen Selektor abgefragt hat, kann man mit jQuery direkt bearbeiten/modifizieren.
So kann man zum Beispiel mit `.text()` direkt auf den Inhalt des Elements zugreifen; falls der Selektor mehr als ein Element zurückliefert, so werden deren Inhalte automatisch aneinandergehängt.

```
> $("h1").text()
< "Erste ÜberschriftZweite ÜberschriftDritte Überschrift"
> $("h1:first").text()
< "Erste Überschrift"
```

Ebenso kann man den Inhalt eines Elements mit der Methode `.text("Neuer Text")` ändern; falls der Selektor mehr als ein Element enthält, so wird der Text bei jedem dieser Elemente geändert.

```
> $("h1:first").text("Neuer Text")
< [<h1>Neuer Text</h1>]
> $("h1:first").text()
< "Neuer Text"
```

Änderungen werden sofort im DOM übernommen und somit in der HTML-Darstellung vom Browser aktualisiert dargestellt.

Mit `.attr("name")` lässt sich von einem Tag ein entsprechendes Attribut abfragen; mit `attr("name", "wert")` wiederum ein Attribut überschreiben bzw. neu erstellen.
Dies kann zum Beispiel für das `src` Attribut von Bildern (also `img`-Tags) verwendet werden.

Elemente aus dem DOM (und somit auch aus der zugehörigen HTML-Darstellung) lassen sich mit `.hide()` verstecken und mit `.show()` wieder anzeigen.
Dieses Beispiel versteckt zuerst die letzte Überschrift und zeigt diese anschließend wieder an.
Dabei wird automatisch ein neues Style-Attribut beim Element angelegt, welches für diese Funktionalität benötigt wird. 

```
> $("h1:last").hide()
< [<h1 style="display: none;">Dritte Überschrift</h1>]
> $("h1:last").show()
< [<h1 style="display: block;">Dritte Überschrift</h1>]
```

Soll ein Element ganz gelöscht werden, so kann die Methode `.remove()` verwendet werden.

```
> $("h1")
< [<h1>Erste Überschrift</h1>, <h1>Zweite Überschrift</h1>, <h1>Dritte Überschrift</h1>]
> $("h1:last").remove()
< [<h1>Dritte Überschrift</h1>]
> $("h1")
< [<h1>Erste Überschrift</h1>, <h1>Zweite Überschrift</h1>]
```

Ein neues Element lässt sich mit `$("<tag>")` anlegen - hier werden im Unterschied zu den Selektoren die spitzen Klammern beim Tag benötigt.
Ein derartig neu erstelltes Element hat allerdings noch keinerlei Zuordnung zum DOM und muss erst hinzugefügt werden.
Dazu dienen die folgenden Methoden:

* `.after(neuesTag)` legt ein neues Tag nach dem vom Selektor zurückgelieferten Element an.
* `.before(neuesTag)` legt ein neues Tag vor dem vom Selektor zurückgelieferten Element an.
* `.append(neuesTag)` legt ein neues Tag innerhalb des vom Selektor zurückgelieferten Elements an (am Ende).
* `.prepend(neuesTag)` legt ein neues Tag innerhalb des vom Selektor zurückgelieferten Elements an (am Anfang).

Falls der Selektor mehr als ein Element enthält, so beeinflussen diese Methoden alle zurückgelieferten Elemente, d.h. das neue Tag wird mehrfach angelegt.

Zusätzlich gibt es auch die Methoden `.appendTo(selektor)`, `prependTo(selektor)`, `.insertAfter(selektor)` und `.insertBefore(selektor)` welche die gleichen Auswirkungen wie die obigen Methoden haben, allerdings auf das neue Tag angewendet werden müssen.

Die beiden Varianten `abs.insertAfter("h1:last")` und `$("h1:last").after(abs)` sind gleichwertig.

```
> var abs = $("<p>").text("Ein neuer Absatz")
< undefined
> abs
< [<p>Ein neuer Absatz</p>]
> abs.insertAfter("h1:last")
< [<p>Ein neuer Absatz</p>]
> $("h1:last").after(abs)
< [<h1>Dritte Überschrift</h1>]
```

Um ein neues Element innerhalb eines anderen Tags anzulegen, kann die Methode `.append()` bzw. `.appendTo()` verwendet werden.
Dies kann zum Beispiel dazu verwendet werden, wenn ein neues Feld in einen `div`-Container hinzugefügt werden soll, oder eine neue Zeile an eine Tabelle angehängt werden soll.

# Abrufen und Einfügen von Web-Content (AJAX)

Externe Inhalte werden üblicherweise entweder in JSON (JavaScript Object Notation) oder XML (eXtensible Markup Language)  zur Verfügung gestellt.
Für jQuery (bzw. generell in JavaScript) sind Inhalte im JSON-Format deutlich einfacher zu verarbeiten.

Ein einfaches Beispiel für ein Web Service ist der [Random User Generator](http://randomuser.me "Random User Generator").
Im Gegensatz zu vielen Web Services, bietet dieser lediglich die Abfrage von Daten (mittels eines GET-Requests) an, auch wird kein eigener API-Key benötigt und die Anfragen können unverschlüsselt und unsigniert durchgeführt werden.
Dadurch ist dieses Web Service sehr einfach in eigene Seiten zu integrieren.

Eine einfache Anfrage an (http://api.randomuser.me) liefert die Daten von einem zufällig generierten Benutzer zurück, wie das folgende Beispiel zeigt:

```JSON
{
  results: [{
    user: {
      gender: "female",
      name: {
        title: "ms",
        first: "lois",
        last: "williams"
      },
      location: {
        street: "1969 elgin st",
        city: "frederick",
        state: "delaware",
        zip: "56298"
      },
      email: "lois.williams50@example.com",
      username: "heavybutterfly920",
      password: "enterprise",
      salt: ">egEn6YsO",
      md5: "2dd1894ea9d19bf5479992da95713a3a",
      sha1: "ba230bc400723f470b68e9609ab7d0e6cf123b59",
      sha256: "f4f52bf8c5ad7fc759d1d4156b25a4c7b3d1e2eec6c92d80e508aa0b7946d4ba",
      registered: "1288182167",
      dob: "146582153",
      phone: "(555)-942-1322",
      cell: "(178)-341-1520",
      SSN: "137-37-8866",
      picture: {
        large: "http://api.randomuser.me/portraits/women/55.jpg",
        medium: "http://api.randomuser.me/portraits/med/women/55.jpg",
        thumbnail: "http://api.randomuser.me/portraits/thumb/women/55.jpg",
      },
      version: "0.4.1"
    },
    seed: "graywolf"
  }]
}
```

Dabei werden geschwungene Klammern (`{}`) verwendet um Objekte darzustellen; die Bezeichnungen vor dem Doppelpunkt sind die jeweiligen Attribute des Objekts.
Mit eckigen Klammern (`[]`) werden Arrays dargestellt.

Um dieses Web Service mittels jQuery abzufragen können die Methoden `$.get()`, `$.getJSON()` und `$.ajax()` verwendet werden.
Jede dieser Methoden benötigt eine Callback-Methode, die in JavaScript auch anonym sein darf.

```
> $.get("http://api.randomuser.me", function(data) { console.log(data); });
< Object
    results: Array[1]
      0: Object
        seed: "5258f7b7aa3076b0"
        user: Object
        __proto__: Object
      length: 1
      __proto__: Array[0]
    __proto__: Object
```

```
> $.getJSON("http://api.randomuser.me", function(data) { console.log(data); });
< Object
    results: Array[1]
      0: Object
        seed: "89073c213d20c00e"
        user: Object
        __proto__: Object
      length: 1
      __proto__: Array[0]
    __proto__: Object
```

```
> $.ajax({
    url: 'http://api.randomuser.me/',
    dataType: 'json',
    success: function(data){
      console.log(data);
    }
  });
< Object
    results: Array[1]
      0: Object
        seed: "5f814b856f86a39e"
        user: Object
        __proto__: Object
      length: 1
      __proto__: Array[0]
    __proto__: Object
```

Diese drei Beispiele machen eine Anfrage auf das Web Service und geben - sobald die Anfrage beantwortet wurde - das Ergebnis wieder aus.
Die Verarbeitung passiert dabei asynchron, d.h. nach dem Stellen der Anfrage läuft das Skript weiter und wird bei Abschluss der Anfrage unterbrochen.

Die Funktion, die nach Abschluss der Anfrage aufgerufen wird kann entweder wie in den Beispielen oben anonym definiert werden, d.h. direkt an der Stelle wo sie aufgerufen wird.
Alternativ kann auch eine Funktion im Vorhinein definiert werden, welche dann einfach nur übergeben wird, wie das folgende Beispiel zeigt:

```
> function callback(data) {
    console.log(data);
  }
< undefined
> callback("Test")
< Test
> $.ajax({
    url: 'http://api.randomuser.me/',
    dataType: 'json',
    success: callback
  })
< Object
    results: Array[1]
      0: Object
        seed: "a79ef58d165fa02a"
        user: Object
        __proto__: Object
      length: 1
      __proto__: Array[0]
    __proto__: Object
```

Natürlich sollten die Inhalte, die das Web Service zurückliefert auch in der Seite inkludiert werden und nicht nur wie in den bisherigen Beispielen in der Konsole ausgegeben werden.
Dazu können nun wieder die jQuery Selektoren und die Methoden zur DOM-Manipulation verwendet werden.

Um zum Beispiel einen neuen Absatz mit dem Inhalt `Hallo Vorname!` zu erstellen, kann folgende Funktion als Callback verwendet werden:

```JavaScript
function callback(data) {
  var abs = $("<p>").text("Hallo " + data.results[0].user.name.first + "!");
  $("h1:last").after(abs);
}
```

Nach Aufruf des AJAX-Requests sollte nun ein neuer Absatz in der HTML-Seite dargestellt werden:

```JavaScript
$.ajax({
  url: 'http://api.randomuser.me/',
  dataType: 'json',
  success: callback
});
```

Beim Random User Web Service kann man mit dem Parameter `results` die Menge der Rückgabewerte beeinflussen, falls man mehr als einen Benutzer automatisch zurückgeben soll.
Um hier automatisch mehrere Absätze im Dokument zu erstellen, muss in der Callback Funktion eine Schleife integriert werden:

```JavaScript
function callback(data) {
  for (var i=0; i<data.results.length; i++) {
    var abs = $("<p>").text("Hallo " + data.results[i].user.name.first + "!");
    $("h1:last").after(abs);
	}
}

$.ajax({
  url: 'http://api.randomuser.me/?results=5',
  dataType: 'json',
  success: callback
});
```

Im Resultat des Random User Webservices sind auch Bilder der Benutzer enthalten.
Wenn man diese in die Seite integrieren will, so müssen automatisch entsprechende `img`-Tags erstellt werden, wobei hier die URL im `src`-Attribut und nicht im Inhalt selber übergeben wird.

Das folgende Beispiel fügt das Bild des Benutzers in die Seite ein:

```JavaScript
function callback(data) {
  var url = data.results[0].user.picture.thumbnail;
  var img = $("<img>", {src: url});
  img.insertAfter("h1:first");
}
```


