Princippet om indkapsling i PPE
===============================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	da: princippet-om-indkapsling-i-ppe
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

Et af hovedprincipperne i OOP er **indkapslingsprincippet**, som siger, at komplekse problemer skal opdeles i mange små problemer, som vi kan løse uafhængigt af hinanden og samtidig. Samtidig er vi som brugere ligeglade med, hvordan det sker, og dataene (den interne tilstand) forbliver isolerede.

Hvis vi f.eks. skal løse problemet med at returnere resultatet `1,6` baseret på en brugerforespørgsel med udtrykket `(5+3)*(2/(7+3))`, er der sandsynligvis ingen af os, der kan skrive en enkelt funktion eller metode, der løser dette problem på én gang.

**TIP:** En færdig løsning på denne type eksempel findes i artiklen <a href="/pokrocila-kalkulacka">Behandling af et matematisk udtryk som en streng</a>, men vær forberedt på, at det ikke er let.

Indkapsling giver abstraktion over objekter
-----------------------------------------

Med indkapsling kan du bruge objekter "som en bruger", dvs. du kan kalde deres metoder og ikke bekymre dig om, hvordan de fungerer internt.

Lad os antage, at vi skal beregne en medarbejders løn, og at vi ønsker at bruge en eksisterende klasse fra en anden programmør til at gøre det. Vi behøver kun at kende de obligatoriske konstruktørparametre, og så kan vi "bare bruge" klassen:

```php
$mzda = new MzdaZamestnance(
    25000, // bruttoløn
    6,     // antal år i virksomheden
    10,    // antal års erfaring
    true   // er det en mand?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

Objektets parametre er fiktive og svarer ikke realistisk til den måde, hvorpå lønnen beregnes. Princippet illustreres især ved, at vi **kun behøver at kende den generelle offentlige grænseflade** og ikke engang behøver at beskæftige os med objektets **interne tilstand**, ikke engang **den interne implementering** og slet ikke **hvorfor det fungerer, som det gør**. Vi kalder blot metoden `getCista()` og får nettoudbyttet.

Indkapsling er et designspørgsmål
----------------------------

Det er vigtigt at bemærke, at **indkapsling i sig selv ikke er en funktion eller syntaks i sproget**. At en klasse og et program er indkapslet er kun et spørgsmål om, at programmøren designer programmet og tænker over koden.

Tænk altid på klasse design på denne måde:

- KISS (keep it simple), hold grænsefladen enkel, og tving ikke brugeren til at tænke unødigt. Løs den komplekse logik for brugeren, og han vil være taknemmelig.
- Brugeren af klassen (en anden programmør eller dig i fremtiden) behøver ikke at kende den interne logik overhovedet, og det bør være nok at kende metodebetegnelserne og deres parametre.
- Hvis jeg har brug for hjælpeberegninger til beregningen, som ikke er af interesse for brugeren og kun er tekniske, giver det ingen mening at oprette en getter for dem overhovedet, og de bør kun beregnes internt.
- Klassen skal opfylde algoritmens grundlæggende egenskaber, navnlig at den generelt fungerer for alle data.
- Offentligt tilgængelige metoder bør være udformet således, at de giver tilstrækkelige oplysninger til, at det er let at udvide objektet med nye funktioner i fremtiden, så vi let kan beregne nye data ud fra det, vi allerede ved.

Hold interne data ikke offentligt tilgængelige
-------------------------------

For egenskaber og metoder, der omhandler intern logik, er det fornuftigt at indstille synligheden som `private`. Den største fordel ved dette er, at de ikke vil blive kaldt udefra, og at brugeren vil være tvunget til at bruge den grænseflade, du har designet, hvilket beskytter objektets data og interne tilstand.

Lad os f.eks. have et objekt, der repræsenterer en bankkonto, hvor vi ønsker at bogføre betalinger og behandle den aktuelle saldo:

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
            throw new \Exception('Du har ikke så mange penge!');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Bemærk, at klassen kun indeholder en enkelt `privat` egenskab `$sum`, som indeholder den aktuelle saldo.

Hvis vi ønsker at få den aktuelle saldo, er der en metode `getSum()` til dette formål, men vi har ingen mulighed for at ændre den nye saldoværdi. Vi kan kun fjerne penge med metoden `pay()` eller tilføje penge med metoden `addMoney()`.

Takket være dette princip ved vi altid med sikkerhed, at ingen kan ødelægge objektet.

Hvis brugeren forsøger at betale flere penge, end der faktisk er på kontoen, vil `pay()`-metoden ikke tillade det, fordi den udfører en kontrolberegning, før den overskriver egenskaben `$sum`, og hvis saldoen skulle være negativ (mindre end nul), vil der blive sendt en fejl undtagelsesmeddelelse, og operationen stopper.

Konklusion
-----

Vi har demonstreret det grundlæggende princip om indkapsling, som giver os mulighed for at tænke bedre over abstraktion af objekter og giver os et helt nyt perspektiv.

Når du først har fået en god forståelse for dette princip, vil du se, at <a href="/proc-use-frameworks">frameworks begynder at give rigtig god mening</a>, fordi de internt indkapsler en masse smarthed, som du bare kan bruge.

Næste gang kigger vi på <a href="/edicacy-og-synlighed">edicacy og synlighed</a>.
