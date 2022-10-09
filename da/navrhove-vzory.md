Designmønstre i PHP
===================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	da: designmonstre-i-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

Designmønstre er en måde at tænke programmering på.

De giver en samling af råd, færdige metoder, bedste praksis og indsigt i udvikling. For hvert programmeringsparadigme og hver opgavetype er der visse designmønstre, der er bedst egnede.

Fordele - hvorfor bruge designmønstre?
---------------------------------------

I programmering er det gentagende at løse visse typer problemer, så det giver mening at vælge én metode til at løse disse problemer og gentage den metode.

Den største fordel opstår især under teamudvikling, når alle ved, hvordan applikationen skal udvikles (i henhold til hvilket designmønster), og de anvender det bare. Dette eliminerer de unødvendige mange timers fejlfinding af mærkeligt skrevet kode og forsøg på at forstå de principper, som forfatteren har tænkt sig.

MVC - et eksempel på anvendelse af et designmønster
--------------------------------------

Mit foretrukne designmønster er `MVC` (fra `Model View Controller`), som siger, at programmet er opdelt i 3 uafhængige lag, der kalder hinanden sekventielt og videregiver data til hinanden.

Når en side gengives, kan det f.eks. se ud, som om den først beslutter, hvilken type side det er (f.eks. en kategoridetalje), og kalder derfor `CategoryController` med metoden `detail`.

Et konkret eksempel (jeg forenkler meget):

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Note:**
>
> Dette er kun en prøvekode, der forklarer princippet i `MVC`-designmønsteret.
>
> I en rigtig implementering ville vi være nødt til at finde ud af, hvordan vi f.eks. kan få et eksempel på `CategoryManager` og hvordan vi kan overføre det til egenskaben. Typisk bruges `Dependency injection` til denne type opgaver.

Før siden med kategoridetaljerne vises, kaldes `CategoryController` først for at modtage den faktiske anmodning (dvs. vi viser kategoridetaljerne med et bestemt ID, som routeren f.eks. får i URL'en), hente dataene (ved at spørge den tilsvarende `Model`) og sende de endelige data til skabelonen til visning.

Den store fordel ved dette princip er, at vi kan skrive mange modeller (applikationslogik), som er uafhængige af den måde, dataene præsenteres på (skabelonen), hvilket resulterer i genanvendelig kode. Hvis vi ønsker at bruge `CategoryManager` i et andet projekt, skal vi blot sende dataene gennem `Controller` på en bestemt måde, som vil blive gengivet i overensstemmelse med den skabelon, der er defineret af projektet selv, mens applikationslogikken forbliver den samme, og ingen bekymrer sig om det, fordi softwarelaget har opfyldt sin aftalte grænseflade og sit ansvar.

> **Praktisk note:**
>
> `MVC` designmønsteret bruges af de fleste moderne frameworks såsom Nette, Symfony, Laravel og andre.
>
> Vi kan også støde på `MVC` i udvikling af mobilapps og andre typer software, hvor vi skal hente data og gengive dem i en skabelon i overensstemmelse med side- eller visningstypen.

Vigtige designmønstre til webudvikling
---------------------------------------

Generelt er der mange designmønstre inden for programmering, som ikke er egnede til webudvikling. Denne liste beskriver de vigtigste mønstre, som jeg selv bruger, og som du bør være bekendt med.

Du kan finde <a href="/categories-design-patterns">en komplet liste over alle designmønstre</a>, eksempler på deres anvendelse og detaljerede forklaringer på en separat side.

- **MVC** - Princippet om at opdele et program i `Model` (programlogik og data), `View` (skabelon og datavisning) og `Controller` (der forbinder `Model` og `View`).
- **Dependency injection** - I stedet for at oprette klasseinstanser definerer vi såkaldte services, som vi automatisk injicerer. Koden indrømmer alle afhængigheder, enkelte dele kan udskiftes, og vi opbygger programmet dynamisk.
- **Singleton (Singleton)** - Hver klasse har kun én instans (personligt bruger jeg dette i kombination med `Dependency injection`, hvor hver tjeneste kun har én instans, som sendes på tværs af programmet).
- **Lazy Initialization (forsinket initialisering)** - Vi opretter en instans af et objekt, når der først er brug for det.
- **Adapter** - Hvis vi har brug for kommunikation mellem to inkompatible klasser, konverterer `Adapter` data fra den ene type til den anden (typisk konverteres native PHP-datatyper til databasedatatyper og tilbage igen).
- **Kommando** - I stedet for at udføre en specifik opgave direkte (f.eks. sende en e-mail), husker vi bare, at det var meningen, at det skulle ske. En anden eller den samme proces vil derefter samle anmodningen op og behandle den. Den største fordel er, at vi kan behandle applikationen dovent (og derfor meget hurtigt), gentage fejlslagne handlinger og blot gemme parametrene for opkaldet. Samtidig vil dette give os mulighed for at modtage anmodninger fra forskellige klienter via API'er osv. Afsendte handlinger kan også sættes i kø, prioriteres og eventuelt annulleres før evalueringen.
- **Strategi** - Indkapsler en slags algoritmer eller objekter, der skal ændres, så de er udskiftelige for klienten.

Der findes mange flere designmønstre, men disse var de vigtigste, som du bør kende.

Anti-mønster
------------

Nogle programmeringsudviklingsteknikker betragtes som "anti-mønstre", hvilket er det stik modsatte af et designmønster. Det er normalt en teknik, der producerer mærkelig kode, som ikke er let at fejlfinde og vedligeholde, og som opfører sig "magisk".

Et typisk eksempel er brugen af <a href="/global-variable">globale variabler</a>.
