Grundlæggende filosofi om objektorienteret programmering
========================================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	da: grundlaeggende-filosofi-om-objektorienteret-programmering
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

Objektorienteret programmering er et paradigme, et syn på, hvordan man programmerer. Du vil snart selv kunne se, at OOP medfører en ret grundlæggende forenkling af alle de almindelige problemer og vanskeligheder, som løses igen og igen i den virkelige programmering.

Den grundlæggende idé
-----------------

Den grundlæggende idé med objektorienteret programmering er at opdele et stort program (en kompleks opgave) i mange små dele, som vi kan løse elegant og uafhængigt af hinanden.

Hvis vi f.eks. programmerer et reservationssystem til flybilletter, er det et meget komplekst projekt, som løser tusindvis af sager. Hvis vi kan nedbryde hele den komplekse logik i mange lag og dele, kan vi nemt forstå hele det komplekse system og programmere de enkelte delopgaver uafhængigt af hinanden.

Praktiske fordele ved OOP og opdeling af kode i objekter
------------------------------------------------

Ud over det akademiske synspunkt er der mange praktiske grunde til at bruge OOP:

- Programmet opretter en <a href="/encapsulation">encapsulation</a>, hvilket betyder, at data altid behandles i en lokal kontekst, som er få, så vi ikke behøver at tænke på hele programmet på én gang.
- Vi kan opdele en applikation i hundredtusindvis af små dele, som kan udvikles af forskellige personer parallelt og blot arrangere en fælles grænseflade.
- Vi kan se på applikationen fra forskellige lag (abstrakt på hele moduler eller lokalt på specifikke funktioner, der løser en specifik algoritme).
- Ved fejlsøgning af programmet (debugging) kan vi trinvis ændre programmet, udelade eller erstatte nogle dele.
- Vi kan bedre anvende **designmønstre** og dermed forebygge fejl og kompleksitet i koden, før de opstår.

Personligt kan jeg ikke forestille mig, at hold med mere end én person programmerer anderledes.

> **Note:**
>
> Brugen af objekter giver lidt mere hukommelse og CPU-overhead på computeren, så det vil reducere programmets ydeevne en smule. I et virkeligt miljø er dette imidlertid ligegyldigt, fordi omprogrammering af programmet til objekter giver et vist tab af ydeevne (normalt enheder af procent), men det sparer programmører tid (normalt ti til hundredvis af procent). Mennesketid er altid meget dyrere (og meget begrænset) end computertid.
>
> > Glem ikke, at OOP også medfører en stor forenkling af hele applikationen og gør det muligt at færdiggøre store applikationer på rimelig tid. En lang række komplekse applikationer ville være næsten umulige at programmere uden objekter.

Forholdet til den virkelige verden
-------------------------

Det grundlæggende mål med OOP i softwaredesign er så vidt muligt at simulere egenskaber, adfærd og principper fra den virkelige verden. Objekter i OOP repræsenterer reelle enheder. Denne måde at tænke på gør det muligt for os at opbygge enorme komplekse systemer, der kan forstås godt, løse virkelige problemer internt, som de ville blive løst uden en computer, og principperne kan forklares til rigtige mennesker.

Hvis vi f.eks. implementerer et program til indholdsstyring, giver det mening at lægge al den interne logik i mange rigtige enheder (artikel, forfatter, kategori) og at opbygge sessioner ikke i henhold til kunstigt genererede record-id'er (som det normalt sker i databaser), men i henhold til rigtige relationer.

Eksempel på en konkret gennemførelse:

```php
class Article
{
    private Author $author;

    /** @var Kategori[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

Som vi kan se, indeholder klassen `Article` ikke kun tekniske parametre som f.eks. ID'et for posten med forfatteren, men den er en reel binding til entiteten Author, som igen har sine egne egenskaber.

For en forklaring af den specifikke implementering og syntaks, se <a href="/uvod-do-oop">introduktionsvejledning</a>.
