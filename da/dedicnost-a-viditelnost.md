Arvelighed og synlighed i PPE
=============================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	da: arvelighed-og-synlighed-i-ppe
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

En af de grundlæggende egenskaber ved objektorienteret programmering er **arvning** og <a href="/indkapsling">indkapsling</a>. Med disse funktioner kan du nemt opbygge kompleks applikationslogik, samtidig med at du bevarer en god læsbarhed af implementeringen.

Princippet om arv
-------------------

Arv udtrykker, at implementeringen af en klasse er baseret på en anden klasse. I OOP-terminologi taler vi om **descendant** (den klasse, der arver) og **ancestor** (den klasse, som vi arver).

Generelt fungerer arv ved, at efterkommeren får alle forfædrenes funktioner og enten overtager dem nøjagtigt som forfaderen havde dem, ændrer dem på sin egen måde eller overskriver dem helt og bruger sin egen implementering.

Anvendelsen af denne fremgangsmåde er meget bred, og arv anvendes af en række <a href="/design-patterns">designmønstre</a>.

Reel brug af arv - præsentanter i en applikation
--------------------

Arv er velegnet til at designe såkaldte **presenters**, som er en særlig type klasse, der repræsenterer linking-logikken i **MVC**-designmønstret.

Lad os f.eks. have en trio af siderne `Homepage`, `Contact` og `Login`.

Når hver side implementeres, gentages en stor del af logikken (f.eks. accept af en forespørgsel, opbygning af URL'en, rendering af skabelonen og indsendelse af den resulterende HTML). Det er derfor praktisk at implementere en enkelt forfader med denne logik og blot bruge den i efterkommere.

Vi starter med først at definere forfaderen (klassens navn er ligegyldigt, jeg bruger en konvention fra Nette-rammen):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implementering af metoden til opbygning af URL'en
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // logik for gengivelse af skabelon
   }
}
```

Da jeg definerede klassen, brugte jeg det nye nøgleord `abstract`, som siger, at klassen `BasePresenter` er abstrakt. Det betyder, at vi ikke kan oprette en instans af den, men kun skal bruge den, så en anden klasse arver og implementerer den. Abstraktion har andre nyttige fordele, som vi vil diskutere senere. En klasse behøver ikke at være abstrakt for at kunne arves - det er blot en af de mulige indstillinger.

Nu kan vi implementere en anden klasse, f.eks. `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // logik for gengivelse
       $this->renderTemplate('hjemmeside', [
          'kontaktLink' => $this->link('Kontakt:standard'),
       ]);
   }
}
```

Nu har du en fungerende `HomepagePresenter`-klasse. Bemærk, at klassen er `final`, hvilket betyder, at den ikke længere kan arves, hvilket garanterer, at metoderne vil blive brugt præcis som vi har angivet dem.

Da vi implementerede klassen, oprettede vi en ny metode `run()`, som kun `HomepagePresenter` kan håndtere. Inde i metoden kalder vi metoden `renderTemplate()` og `link()`, som klassen ikke indeholder. Det er dog ligegyldigt, fordi nøgleordet `extends` fortæller os, hvor metoderne skal arves fra, så de bruges.

Takket være arv kunne vi opnå genanvendelighed af kode, fordi metoder, når de først er skrevet, kan bruges flere steder.

Overriding af implementeringen af en bestemt metode
------------

Det kan meget ofte være nyttigt at tilsidesætte en bestemt metodes adfærd under arv. Hvis vi f.eks. ønsker at ændre opførslen af metoden `link()` fra det foregående eksempel i `ContactPresenter`, ville det se således ud:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // logik for gengivelse
      echo $this->link('Hjemmeside:standard', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Hvis du vil tilsidesætte implementeringen, skal du blot definere metoden igen i barnet og tilsidesætte metodekroppen. Det er vigtigt at overholde grænsefladen og implementere de samme inputargumenter.

Synlighed ved arvelighed
--------------------------

Nogle gange vil vi gerne skjule nogle metoder i forbindelse med arv og kun bruge dem internt. Alternativt kan du kun tillade, at de anvendes under arveforløb og ikke som en offentlig grænseflade.

Generelt er der derfor nogle enkle regler for synlighed. Vi betegner metoder med `public`, `protected` eller `private`, og reglerne for synlighed er som følger:

- Alle kan kalde `offentlige` metoder hvor som helst, dvs. når der oprettes en instans af både en forfader og en efterkommer.
- Kun en forfader eller efterkommer kan kalde `beskyttede` metoder, men de kan ikke kaldes fra den offentlige grænseflade, når en instans oprettes. Det er interne metoder til at opnå arv (et godt eksempel er metoden `link()` i det foregående eksempel).
- Kun den aktuelle klasse kan kalde `private`-metoder, uanset indstillingerne for arvelighed og offentlig grænseflade.

Ændring af synlighed ved kørselstid
----------------------------

I meget specifikke tilfælde kan det være nyttigt at ændre synligheden af en metode under kørslen og derefter kalde den. Dette bruges f.eks. af forskellige Doctrine-biblioteker.

For at ændre synligheden bruger vi den native **ReflectionClass**-klasse, der er implementeret af PHP selv.
