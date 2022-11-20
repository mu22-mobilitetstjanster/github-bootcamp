[Innehåll](README.md)

*Uppdaterad HT 2022, Anpassad för java *

1. [Metoder för felsökning](#metoder-fr-felskning)
2. [Praktiska metoder](#praktiska-metoder)
3. [När något går fel](#nr-ngot-gr-fel)
4. [Automatiska tester](#automatiska-tester)



# Fel i koden
## Metoder för felsökning
När något går fel i din app, är det viktigt att ha ett strukturerat angreppssätt.

När något blir fel, betyder det att det finns en skillnad mellan *vad du sagt till datorn* och *vad du vill att datorn ska göra*. Ofta har vi gjort ett *underförstått antagande*, om vad variabler borde ha för värden, som visar sig vara fel.

#### 1 Vad förväntar du dig ska hända? Vad är det som faktiskt händer?
Detta är den första fråga du kommer att få om du ber någon om hjälp. Se till att du själv funderar över den först.

#### 2 Vad får du för felmeddelande?
Detta är den andra frågan du kommer att få om du ber någon om hjälp. Ofta finns en bra ledtråd till lösningen dold i ett (mer eller mindre) kryptiskt felmeddelande. Läs igenom felet du får ordentligt. Ofta får man 10 rader fel, men 1 av dem innehåller en beskrivning av felet.

#### 3 Vilken fil? Vilken kodrad? Vilken variabel?
Vi har nästan alltid tillgång till exakta positionen i koden som datorn hittar felet på.

#### 4 Vad har dina variabler för värden?
För 90% av fallen beror felet på att en variabel får ett annat värde än du trodde. Var hyperskeptisk och dubbelkolla allt.

#### 5 Sök på internet
Nu har du kanske bildat dig en uppfattning om vad felet handlar om. Kopiera det exakta felmeddelandet och klistra in det i en sökmotor. Över 99% av fallen finns det någon annan som haft exakt samma problem som du. Stackoverflow är en bra resurs, som man ofta hittar lösningen på.

## Praktiska metoder
#### 1 Divide and conquer
Läs mer här: [dev.to: a gentle introduction to divide and conquer algorithms](https://dev.to/brandonskerritt/a-gentle-introduction-to-divide-and-conquer-algorithms-1ga)

*Använd om du inte vet var felet inträffar. Till exempel om kompilatorn/webbläsaren inte talar om vilken fil och rad felet inträffar på, eller om orsaken till felet händer någon annan stans.*

Kommentera bort hälften av kodraderna med `/*  */`. Kör programmet. Är felet kvar?
+ Om felet är kvar: nu vet du att felet är i kodraderna som du inte kommenterade bort.
+ Om felet är borta: felet var i kodraderna som du kommenterade bort.

Du har eliminerat hälften av alla möjliga kodrader som felet kan gömma sig på. Upprepa metoden med den halva som felet kan gömma sig i. När du halverat några gånger har du till slut bara en rad kvar. Då vet du att felet inträffar på den raden.


#### 2 Logga: vilka rader körs
Använd möjligheten att skriva ut strängar på konsolen. (System.out.println i java) Lägg till en utskrift i varje kodblock: först i funktion, sist i funktion, if-sats och for-loop. Skriv till exempel ut ett löpnummer, alltså en siffra som ökar i värde.

```java
public static void buggyFunction() {
	System.out.println('1');
	var suspiciousVariable = true;
	System.out.println('2');
	...
	System.out.println('7');
	return;
}
```

Om du ser "2" i konsolen, vet du att felet inträffar i raden som är precis efter utskriften.

#### 3 Skriv ut värdet på variabler
Nu vet du vilken rad som misslyckas. Nästa steg är att ta reda på *vad* som går fel. Lägg till variabler i dina utskrifter.
```java
public static void buggyFunction(String mystery) {
	System.out.println('1: ' +  mystery);
	...
	mystery += x;
	System.out.println('7: ' + mystery);
	return;
}
```


## När något går fel
#### Fånga felet med `try / catch`.
Vad man gör sedan beror på appen man bygger.
```java
var data = null, message = ''
try {
	data = tryToGetDataFromDatabase();
	message = 'Got data, everything is fine';
} catch(Exception e) {
	// error.message är ett felmeddelande som beskriver det som gick fel
	message = 'Something went wrong';
}
```


#### Återkoppla till användaren
+ Tala om vad som gått fel
	+ Exempel: ingen internetuppkoppling, får inte kontakt med servern
+ Tala om vad användaren kan göra åt det
	+ Exempel: kontrollera din internetuppkoppling, försök igen senare