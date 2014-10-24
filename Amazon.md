# Inhaltsverzeichnis

{:toc}

# Verwenden der Amazon Product Advertising API mit jQuery

## Einführung

Die *Amazon Product Advertising API* kann dazu verwendet werden, um Daten von Amazon abzufragen. Dies kann die Suche nach Produkten aber auch die Abfrage eines einzelnen Produkts sein. Es ist sogar möglich damit den Einkaufswagen zu bearbeiten.

### Notwendige Vorbereitungen

Für einen einfachen Einstieg kann die [Getting Started Guide](http://docs.aws.amazon.com/AWSECommerceService/latest/GSG/Welcome.html "Amazon Product Advertising API - Getting Started Guide") verwendet werden. Um die Amazon Web Services (zu denen auch die Product Advertising API gehört) verwenden zu können, müssen zuerst verschiedene Amazon-Registrierungen durchgeführt werden:

* Registrierung im *Amazon Partnerprogram* (*Amazon Associate*), welches verwendet wird um Abfragen mit einer spezifischen Quelle zu verbinden. In diesem Programm bekommt man ein *AssociateTag* zugewiesen, welches die Quelle der Abfrage (z.B. WebSite von der die Abfrage stammt) zuordnet.
  
  Der spezifische Zweck dieser Zuordnung ist, das man als *Associate* bei Verkäufen eine prozentuelle Beteiligung bekommt, d.h. wenn ein Produkt von Amazon auf der WebSite dargestellt wird, und der Besucher der Verlinkung nach Amazon folgt und das Produkt dort kauft, erhält der WebSite-Betreiber eine Beteiligung.

* Registrieren bei den *Amazon Web Services* und erstellen der Zugriffsschlüssel. Für die Identifizierung bei den Web Services werden Zugriffsschlüssel benötigt (*Access Key ID* und *Secret Access Key*), über die der Zugang zu den Web Services authentifiziert wird.

  Amazon verwendet diese Schlüssel, um den Zugriff auf die Web Services überhaupt erst zu erlauben bzw. um dort verschiedene Begrenzungen (z.B. maximale Anzahl von Anfragen in einem bestimmten Zeitraum) durchzusetzen.

Nach erfolgreicher Registrierung dieser beiden Identifikationen, sollte die Funktionalität geteste werden.

### Testen der Product Advertising API

Zum einfachen Testen der Abfragen stellt Amazon den [Signed Requests Helper](http://associates-amazon.s3.amazonaws.com/signed-requests/helper/index.html "Amazon Product Advertising API - Signed Requests Helper") zur Verfügung. Dieser benötigt die oben erstellten Schlüssel um eine Abfrage durchzuführen.

Nach Eingabe der entsprechenden Schlüssel (*Access Key ID* und *Secret Access Key*) kann eine einfache Beispiel-Abfrage in das erste Formularfeld eingefügt werden. Hier eine Beispielanfrage, die Informationen zu einem Buch ([The Lies of Locke Lamora](http://www.amazon.com/The-Lies-Locke-Lamora-Gollancz/dp/0575079754/ref=sr_1_1?ie=UTF8&qid=1412758483&sr=8-1&keywords=0575079754 "Amazon: The Lies of Locke Lamora by Scott Lynch, Paperback Edition")) abfragen soll (das entsprechende *AssociateTag* muss hierbei allerdings noch eingefügt werden):

```
http://ecs.amazonaws.com/onca/xml?Service=AWSECommerceService
&Version=2011-08-01
&Operation=ItemLookup
&ItemId=0575079754
&AssociateTag=  --  AssociateTag
&IncludeReviewsSummary=no
&ResponseGroup=ItemAttributes,Images,EditorialReview
```

Es handelt sich dabei um eine `ItemLookup` Anfrage, die eben Informationen über ein bestimmtes Objekt in Amazon anfragt. Das Objekt wird dabei über die `ItemId` angegeben. In der `ResponseGroup` wird dem Web Service mitgeteilt, welche Informationen überhaupt zurückgeliefert werden sollen.

Mit dem Button *Display Signed URL* erstellt der *Signed Requests Helper* eine neue URL mit der die Product Advertising API abgefragt werden kann. Es wird dabei automatisch die *Access Key ID* sowie ein aktueller Zeitstempel eingesetzt. Zusätzlich wird eine Signatur für die Anfrage erstellt (wozu der *Secret Access Key* verwendet wird) und angehängt, damit Amazon weiß, das die Anfrage auch wirklich von einer authentischen Quelle durchgeführt wird.

Fügt man diese Anfrage im Browser ein, erhält man als Antwort ein recht umfangreiches XML, die hier in stark gekürzter Form wiedergegeben wird:

```xml
<?xml version="1.0" ?>
<ItemLookupResponse xmlns="http://webservices.amazon.com/AWSECommerceService/2011-08-01">
  <OperationRequest><!-- ... --></OperationRequest>
  <Items>
    <Request>
      <IsValid>True</IsValid>
      <ItemLookupRequest>
        <IdType>ASIN</IdType>
        <ItemId>0575079754</ItemId>
        <ResponseGroup>ItemAttributes</ResponseGroup>
        <ResponseGroup>Images</ResponseGroup>
        <ResponseGroup>EditorialReview</ResponseGroup>
        <VariationPage>All</VariationPage>
        <IncludeReviewsSummary>no</IncludeReviewsSummary>
      </ItemLookupRequest>
    </Request>

    <Item>
      <ASIN>0575079754</ASIN>
      <DetailPageURL><!-- ... --></DetailPageURL>
      <ItemLinks><!-- ... --></ItemLinks>
      <!-- ... -->
      <ImageSets>
        <ImageSet Category="primary">
          <SwatchImage>
            <URL>http://ecx.images-amazon.com/images/I/513ySc5aehL._SL30_.jpg</URL>
            <Height Units="pixels">30</Height>
            <Width Units="pixels">20</Width>
          </SwatchImage>
          <!-- ... -->
          <LargeImage>
            <URL>http://ecx.images-amazon.com/images/I/513ySc5aehL.jpg</URL>
            <Height Units="pixels">500</Height>
            <Width Units="pixels">325</Width>
          </LargeImage>
        </ImageSet>
      </ImageSets>
      
      <ItemAttributes>
        <Author>Scott Lynch</Author>
        <Binding>Paperback</Binding>
        <EAN>9780575079755</EAN>
        <EANList>
          <EANListElement>9780575079755</EANListElement>
        </EANList>
        <Edition>New Ed</Edition>
        <ISBN>0575079754</ISBN>
        <!-- ... -->
        <ProductGroup>Book</ProductGroup>
        <ProductTypeName>ABIS_BOOK</ProductTypeName>
        <PublicationDate>2007-02-01</PublicationDate>
        <Publisher>Gollancz</Publisher>
        <Studio>Gollancz</Studio>
        <Title>The Lies of Locke Lamora (Gollancz)</Title>
      </ItemAttributes>
      
      <EditorialReviews>
        <EditorialReview>
          <Source>Product Description</Source>
          <Content>They say that the Thorn of Camorr can beat anyone in a fight. They say he steals from
            the rich and gives to the poor. They say he's part man, part myth, and mostly street-corner
            rumor. And they are wrong on every count. Only averagely tall, slender, and god-awful with a
            sword, Locke Lamora is the fabled Thorn, and the greatest weapons at his disposal are his wit
            and cunning. He steals from the rich - they're the only ones worth stealing from - but the
            poor can go steal for themselves. What Locke cons, wheedles and tricks into his possession is
            strictly for him and his band of fellow con-artists and thieves: the Gentleman Bastards.
            
            Together their domain is the city of Camorr. Built of Elderglass by a race no-one remembers,
            it's a city of shifting revels, filthy canals, baroque palaces and crowded cemeteries. Home
            to Dons, merchants, soldiers, beggars, cripples, and feral children. And to Capa Barsavi, the
            criminal mastermind who runs the city. But there are whispers of a challenge to the Capa's
            power. A challenge from a man no one has ever seen, a man no blade can touch. The Grey King
            is coming. A man would be well advised not to be caught between Capa Barsavi and The Grey
            King. Even such a master of the sword as the Thorn of Camorr. As for Locke Lamora ...
            </Content>
          <IsLinkSuppressed>0</IsLinkSuppressed>
        </EditorialReview>
      </EditorialReviews>
    </Item>
  </Items>
</ItemLookupResponse>
```

## Abfrage mittels JavaScript

Um eine Anfrage an das Amazon Product Advertising API zu stellen, muss - wie im obigen Beispiel mit dem *Signed Requests Helper* - die Abfrage entsprechend bearbeitet und signiert werden, bevor diese abgesendet werden kann.

Ein Beispiel für den genauen Ablauf eines Requests findet sich in der [Amazon Product Advertising API - Developer Guide](http://docs.aws.amazon.com/AWSECommerceService/latest/DG/Welcome.html): [Example REST Requests](http://docs.aws.amazon.com/AWSECommerceService/latest/DG/rest-signature.html "Amazon Product Advertising API - Developer Guide - Example REST Requests"). Nachdem dieser Vorgang für jede Anfrage durchgeführt werden muss, bietet es sich an hier entsprechende Funktionen bzw. eine Helper-Klasse zu erstellen.

### Funktionen zur Verarbeitung einer Anfrage

Für die in der oben erwähnten Dokumentation aufgeführten Schritte werden jeweils eigene Funktionen erstellt, die den entsprechenden Schritt durchführen. Für dieses Beispiel ist die ursprüngliche Anfrage (aus dem obigen Beispiel) in der Variablen `requestURL` gespeichert:

```JavaScript
var requestURL = "http://ecs.amazonaws.com/onca/xml?Service=AWSECommerceService&Version=2011-08-01&Operation=ItemLookup&ItemId=0575079754&AssociateTag=--AssociateTag--&IncludeReviewsSummary=no&ResponseGroup=ItemAttributes,Images,EditorialReview"
```

Zusätzlich wird die *Access Key ID* und der *Associate Tag* auch noch als weiterer Parameter benötigt:

```JavaScript
requestURL = requestURL + "&AssociateTag=blablubb"
requestURL = requestURL + "&AccessKeyID=1234567890"
```

#### Erstellen eines Zeitstempels

Der Zeitstempel muss als GMT angegeben werden, und zwar in einer ISO-konformen Formatierung.

In JavaScript kann die Klasse `Date` verwendet werden, um die aktuelle Zeit abzurufen und in das entsprechende Zielformat umzuwandeln.

```JavaScript
var time = new Date()
var isoTime = time.toISOString()
```

Dieser Zeitstempel wird nun als zusätzlicher Parameter zum Request hinzugefügt:

```JavaScript
requestURL = requestURL + "&Timestamp=" + isoTime
```

#### Encoding der Sonderzeichen in der Anfrage

Eine Anfrage kann verschiedene Sonderzeichen beinhalten. Diese müssen durch die entsprechenden Ersetzungsregeln behandelt und ersetzt werden. Hierbei sollen allerdings nicht sämtliche Sonderzeichen ersetzt werden, sondern lediglich die in der Anfrage vorkommenden `,` und `:` Symbole:

```JavaScript
requestURL = requestURL.replace( /,/g, "%2C" )
requestURL = requestURL.replace( /:/g, "%3A" )
```

Um sämtliche Sonderzeichen zu ersetzen kann die Methode `encodeURIComponent(string)` verwendet werden, diese wandelt aber auch die gültigen Symbole `?` und `=` um, was nicht erwünscht ist.

Im Idealfall sollten die einzelnen Parameter einzeln komplett encodiert werden. Dazu müsste der Request in einzelne Parameter aufgeteilt werden, und dann jeder Parameter einzeln in Schlüssel und Werte aufgeteilt werden, und jedes Element für sicht encodiert werden.

#### Aufteilen der Anfrage in einzelne Parameter

Die Anfrage besteht ja aus einer Vielzahl verschiedener Parameter. Diese müssen dem Web Service in sortierter (nach Zeichenwerten, also nicht alphanumerisch!) übergeben werden. Dazu muss die Anfrage in die einzelnen Parameter aufgeteilt und diese anschließend sortiert werden.

```JavaScript
var urlParts = requestURL.split( "?" )
var url = urlParts[0].replace( "%3A", ":" )
var parameters = urlParts[1].split( "&" )
```

Optional könnten vor dem Zusammenfügen die einzelnen Parameter encodiert werden.

#### Sortieren und zusammenfügen der Parameter

Die Parameter müssen in sortierter Reihenfolge übergeben werden. Dazu wird das Parameter-Array sortiert und anschließend wieder mit dem `&`-Symbol zusammengefügt:

```JavaScript
var parameters = parameters.sort().join( "&" )
```

#### Spezifische Daten hinzufügen

Die Amazon Web Services benötigen vor der Signatur zusätzliche 3 Zeilen, die der Zeichenkette hinzugefügt werden müssen:

```
GET
webservices.amazonaws.com
/onca/xml
```

```JavaScript
var toSign = "GET\nwebservices.amazonaws.com\n/onca/xml\n" + parameters
```

Das zu signierende Ergebnis sollte nun wie folgt aussehen:

```
GET
webservices.amazonaws.com
/onca/xml
AccessKeyID=1234567890&AssociateTag=blablubb&IncludeReviewsSummary=no&ItemId=0575079754&Operation=ItemLookup&ResponseGroup=ItemAttributes%2CImages,EditorialReview&Timestamp=2014-10-08T10:46:14.884Z&Version=2011-08-01
```

#### Erstellen der Signatur für den Request

Oben erstellter String (`toSign`) muss nun mit dem *Secret Access Key* entsprechend als HMAC mit dem SHA-256 Hash-Algorithmus signiert werden. Hier wird die [crypto-js](https://code.google.com/p/crypto-js/) JavaScript Bibliothek verwendet.

```JavaScript
$.getScript("http://crypto-js.googlecode.com/svn/tags/3.1.2/build/rollups/hmac-sha256.js")
var hash = CryptoJS.HmacSHA256( toSign, SecretAccessKey )
```

Bei Verwendung von "1234567890" als *Secret Access Key* wird folgende Signatur erzeugt:

```JavaScript
hash.toString(CryptoJS.enc.Base64)
"fIca5dL3efZgtKRCoK5NCfkLr7j8mHAxfQQSqqGkL+s="
```

Diese Signatur enthält möglicherweise wieder Sonderzeichen, die mittels `encodeURIComponent` wieder korrept kodiert werden sollten.

#### Anhängen der Signatur an den Request

Zum Abschluss muss diese Signatur noch an die Anfrage angehängt werden, und die gesamte Anfrage wieder zusammengestellt werden:

```JavaScript
parameters = url + "?" + parameters + "&Signature=" + encodeURIComponent( hash.toString(CryptoJS.enc.Base64) )
```

Das Ergebnis lautet dann wie folgt:

```
http://ecs.amazonaws.com/onca/xml?AccessKeyID=1234567890&AssociateTag=blablubb&IncludeReviewsSummary=no&ItemId=0575079754&Operation=ItemLookup&ResponseGroup=ItemAttributes%2CImages,EditorialReview&Timestamp=2014-10-08T10:46:14.884Z&Version=2011-08-01&Signature=fIca5dL3efZgtKRCoK5NCfkLr7j8mHAxfQQSqqGkL%2Bs%3D
```

Für eine reale Anfrage muss natürlich entsprechend das *AssociateTag* sowie die *Access Key ID* angepasst werden und für die Signatur der reale *Secret Access Key* verwendet werden.

#### Gesamtfunktion

Die obigen Teilaufgaben können in einer Funktion zusammengefasst werden. Die obige Funktionalität wurde hier noch um die zusätzliche Funktion `encodeNameValuePairs` erweitert, die jedes Parameter-Paar für sich einzeln korrekt kodiert.

```JavaScript
// Von "AWS Signed Requests Helper" übernommen:
// http://associates-amazon.s3.amazonaws.com/signed-requests/helper/index.html
function encodeNameValuePairs( pairs ) {
	for ( var i = 0; i < pairs.length; i++ ) {
		var name = "";
		var value = "";
		
		var pair = pairs[i];
		var index = pair.indexOf( "=" );
		
		// take care of special cases like "&foo&", "&foo=&" and "&=foo&"
		if ( index == -1 ) {
			name = pair;
		} else if ( index == 0 ) {
			value = pair;
		} else {
			name = pair.substring( 0, index );
			if ( index < pair.length - 1 ) {
				value = pair.substring( index + 1 );
			}
		}
		
		// decode and encode to make sure we undo any incorrect encoding
		name = encodeURIComponent( decodeURIComponent( name ) );
		
		value = value.replace( /\+/g, "%20" );
		value = encodeURIComponent( decodeURIComponent( value ) );
		
		pairs[i] = name + "=" + value;
	}
	
	return pairs;
}

// Verwendet CryptoJS zur Signierung:
// http://crypto-js.googlecode.com/svn/tags/3.1.2/build/rollups/hmac-sha256.js
// http://crypto-js.googlecode.com/svn/tags/3.1.2/build/components/enc-base64-min.js
function signRequest( requestURL, AssociateTag, AccessKeyID, SecretAccessKey ) {
	requestURL = requestURL + "&AssociateTag=" + AssociateTag
	requestURL = requestURL + "&AWSAccessKeyId=" + AccessKeyID
	
	var time = new Date()
	var gmtTime = new Date( time.getTime() + time.getTimezoneOffset() * 60000 )
	var isoTime = time.toISOString()	
	requestURL = requestURL + "&Timestamp=" + isoTime
	
	var urlParts = requestURL.split( "?" )
	var url = urlParts[0]
	var parameters = urlParts[1].split( "&" )
	parameters = encodeNameValuePairs( parameters )
	parameters = parameters.sort().join( "&" )

	var toSign = "GET\necs.amazonaws.com\n/onca/xml\n" + parameters
	var hash = CryptoJS.HmacSHA256( toSign, SecretAccessKey )

	var signedRequest = url + "?" + parameters
	signedRequest = signedRequest + "&Signature=" + encodeURIComponent( hash.toString(CryptoJS.enc.Base64) )
	return signedRequest
}
```

Die Funktion `signRequest` erstellt als Rückgabewert bei korrekten Parametern eine URL, die direkt für den Zugriff auf die AWS verwendet werden kann.

### Verwendung eines Proxies bei fehlenden AWS Zugangsdaten

Falls die Anmeldung bei AWS für die *Access Key ID* und den *Secret Access Key* nicht möglich ist, kann unter [aws.ocrs.at](http://aws.ocrs.at) ein Proxy verwendet werden, der aus den übergebenen Parametern automatisch einen signierten Request erstellt.

Das *AssociateTag* muss dabei jedoch auf jeden Fall mit angegeben werden. Bei Aufruf der URL und Übergabe der üblichen Parameter der *Product Advertising API* liefert diese Seite die signierte URL zurück:

In diesem Beispiel ist auch wieder das *AssociateTag* zu ersetzen:
```
http://aws.ocrs.at/?Service=AWSECommerceService&Version=2011-08-01&Operation=ItemLookup&ItemId=0575079754&AssociateTag=--AssociateTag--&IncludeReviewsSummary=no&ResponseGroup=ItemAttributes,Images,EditorialReview
```

Diese Abfrage liefert als Ergebnis die signierte URL:
```
http://ecs.amazonaws.com/onca/xml?AWSAccessKeyId=AKIAJVM5HKLGATQIH3GQ&AssociateTag=--AssociateTag--&IncludeReviewsSummary=no&ItemId=0575079754&Operation=ItemLookup&ResponseGroup=ItemAttributes%2CImages%2CEditorialReview&Service=AWSECommerceService&Timestamp=2014-10-14T11%3A57%3A08Z&Version=2011-08-01&Signature=zfO5JwQwk5zWXdjFBqltvzRNVqug0%2Btd03GKu%2BuThpM%3D
```

Nachdem die Abfrage über JavaScript aufgrund der CORS-Richtlinien von Amazon nicht erlaubt ist (d.h. eine Anfrage funktioniert zwar von einem Server oder auch direkt über den Browser, nicht jedoch über AJAX), gibt es einen Proxy, der die entsprechende Anfrage automatisch beantwortet:

```
http://aws.ocrs.at/request.php?Service=AWSECommerceService&Version=2011-08-01&Operation=ItemLookup&ItemId=0575079754&AssociateTag=--AssociateTag--&IncludeReviewsSummary=no&ResponseGroup=ItemAttributes,Images,EditorialReview
```

Diese Abfrage lässt sich auch direkt mit jQuery ausführen:
```JavaScript
$.ajax({
	url: "http://aws.ocrs.at/request.php?Service=AWSECommerceService&Version=2011-08-01&Operation=ItemLookup&ItemId=0575079754&AssociateTag=--AssociateTag--&IncludeReviewsSummary=no&ResponseGroup=ItemAttributes,Images,EditorialReview",
	dataType:"xml",
	success: function(xml) { console.log(xml) }
})
```

## Verarbeiten der Anfrage mit jQuery

