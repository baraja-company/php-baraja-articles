Principen för inkapsling i PPE
==============================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	sv: principen-foer-inkapsling-i-ppe
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

En av huvudprinciperna för OOP är **kapslingsprincipen**, som säger att komplexa problem bör delas upp i många små problem som vi kan lösa oberoende av varandra och samtidigt. Samtidigt bryr vi som användare oss inte om hur det sker och data (interna tillstånd) förblir isolerade.

Om vi till exempel löser problemet med att återge resultatet "1,6" baserat på en användarfråga med uttrycket "5+3"*(2/(7+3)", kan förmodligen ingen av oss skriva en enda funktion eller metod som löser problemet på en gång.

**TIP:** En färdig lösning på den här typen av exempel finns i artikeln <a href="/pokrocila-kalkulacka">Bearbetning av ett matematiskt uttryck som en sträng</a>, men var beredd på att det inte är lätt.

Kapsling ger abstraktion över objekt
-----------------------------------------

Med kapsling kan du använda objekt "som en användare", dvs. anropa deras metoder utan att behöva oroa dig för hur de fungerar internt.

Antag att vi ska beräkna en anställds lön och att vi vill använda en befintlig klass från en annan programmerare för att göra det. Vi behöver bara känna till de obligatoriska konstruktörsparametrarna och vi kan "bara använda" klassen:

```php
$mzda = new MzdaZamestnance(
    25000, // bruttolön
    6,     // antal år i företaget
    10,    // antal års erfarenhet
    true   // är det en man?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

Objektets parametrar är fiktiva och motsvarar inte realistiskt hur lönen beräknas. Principen illustreras särskilt av det faktum att vi **ndast behöver känna till det allmänna offentliga gränssnittet** och inte ens behöver ta itu med **objektets interna tillstånd**, inte ens **det interna genomförandet** och definitivt inte **varför det fungerar som det gör**. Vi anropar helt enkelt metoden `getCista()` och får fram nettoutbetalningen.

Kapsling är en konstruktionsfråga
----------------------------

Det är viktigt att notera att **kapsling i sig inte är en funktion eller syntax i språket**. Att en klass och ett program är inkapslat är bara en fråga om att programmeraren utformar programmet och tänker på koden.

Tänk alltid på klassdesign på det här sättet:

- KISS (keep it simple), håll gränssnittet enkelt och tvinga inte användaren att tänka i onödan. Lös den komplexa logiken för användaren och han kommer att vara tacksam.
- Användaren av klassen (en annan programmerare eller du i framtiden) behöver inte känna till den interna logiken alls och det räcker med metodnamnen och deras parametrar.
- Om jag behöver hjälpberäkningar för beräkningen som inte är av intresse för användaren och som endast är tekniska, är det inte meningsfullt att skapa en getter för dem överhuvudtaget och de bör endast beräknas internt.
- Klassen måste uppfylla algoritmens grundläggande egenskaper, särskilt att den fungerar generellt för alla data.
- Allmänt tillgängliga metoder bör utformas så att de ger tillräckligt med information för att man enkelt ska kunna utöka objektet med nya funktioner i framtiden, så att vi enkelt kan beräkna nya data utifrån det vi redan vet.

Håll interna uppgifter hemliga.
-------------------------------

För egenskaper och metoder som handlar om intern logik är det klokt att ställa in synligheten som `private`. Den största fördelen med detta är att de inte kan anropas utifrån och att användaren tvingas använda ditt utformade gränssnitt, vilket skyddar objektets data och interna tillstånd.

Låt oss till exempel ha ett objekt som representerar ett bankkonto där vi vill bokföra betalningar och hantera det aktuella saldot:

```php
class BankAccount
{
    private int $sum;

    public function __construct(int $startSum)
    {
        $this->sum = $startSum >= 0 ? $startSum : 0;
    }

    public function getSum(): int
    {
        return $this->sum;
    }

    public function pay(int $price): void
    {
        $newSum = $this->sum - $price;
        if ($newSum < 0) {
            throw new \Exception('Du har inte så mycket pengar!');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Observera att klassen endast innehåller en enda privat egenskap `$sum`, som innehåller det aktuella saldot.

Om vi vill få fram det aktuella saldot finns det en metod för detta, "getSum()`, men vi har inget sätt att ändra det nya saldovärdet. Vi kan bara ta bort pengar med metoden `pay()` eller lägga till pengar med metoden `addMoney()`.

Tack vare denna princip kan vi alltid vara säkra på att ingen kan bryta objektet.

Om användaren försöker betala mer pengar än vad som finns på kontot tillåter inte metoden `pay()` detta eftersom den utför en kontrollberäkning innan den skriver över egenskapen `$sum`, och om saldot skulle vara negativt (mindre än noll), skickas ett felmeddelande och åtgärden avbryts.

Slutsats
-----

Vi har visat den grundläggande principen om kapsling, som gör det möjligt att tänka bättre på abstraktion av objekt och ger oss ett helt nytt perspektiv.

När du väl har förstått den här principen kommer du att se att <a href="/proc-use-frameworks">rameworks börjar bli väldigt meningsfulla</a>, eftersom de internt kapslar in en massa smartheter som du bara kan använda.

Nästa gång ska vi titta på <a href="/detektering-och-synlighet">detektering och synlighet</a>.
