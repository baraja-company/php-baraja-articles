Lommeregner i PHP: behandling af et matematisk udtryk som en streng
===================================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	da: lommeregner-i-php-behandling-af-et-matematisk-udtryk-som-en-streng
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Forestil dig, at du står over for opgaven med at behandle et simpelt matematisk eksempel, som en bruger indtaster som en tekststreng, f.eks. i et søgefelt. Typisk ønsker brugeren at udføre en simpel numerisk operation med tal. I denne artikel beskrives tankegangen og specifikke instruktioner om, hvordan du gør det.

Naiv gennemførelse
-------------------

I lang tid har jeg spekuleret på, om et simpelt matematisk udtryk kunne behandles ved hjælp af et eller andet trick for at gøre koden så kort som muligt ... og efter mange år har jeg faktisk fundet løsningen.

Overvej den givne løsning **kun som et eksempel**, fordi den er **ekstremt farlig**, og en uærlig bruger kan nemt understrege strengen, hvilket f.eks. vil slette hele programmet eller stjæle databasen!

```php
// Brugerforespørgsel
$query = '5 + 3 * 2';

// Behandling af udtrykket som almindelig PHP-kode
eval('$result = @(' . $query . ');');

// Opførelse af variabel med udtryksløsning
echo $result; // udskriver 11
```

Fidusen er, at <a href="/function-eval">eval()</a>-funktionen udfører strengen, som om den var i sammenhæng med PHP-kode. Det er skørt, men det virker. Wrapperen undertrykker fejlmeddelelser.

Håndtering af mere komplekse input
--------------------------

Ud over det faktum at **behandling af udtryk via eval() er ekstremt farligt**, giver det heller ikke en tilstrækkelig veltalende syntaks til at passe alle. Hvis brugeren overtræder blot en enkelt syntaks, kan hele udtrykket ikke behandles.

Derfor er løsningen at **forstå** og korrigere brugerens forespørgsel i henhold til det formelle aspekt først (kaldet normalisering til den kanoniske form), og derefter videregive og behandle den videre.

Jeg har tidligere programmeret [QueryNormalizer](https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) til netop denne opgave.

Selve behandlingen er en meget udfordrende opgave, fordi du skal forstå de forskellige sammenhænge korrekt. F.eks. at parenteser angiver indlejrede blokke og skal evalueres rekursivt. F.eks. kan udtrykket `5+2^(1+3/2)` ikke løses direkte, fordi brøken først skal løses, derefter skal brøken lægges til tallet i parentes, derefter løses for den hele potens og til sidst lægges på basisniveau.

For overhovedet at kunne opfylde dette krævende krav kan udtrykket ikke længere behandles som en almindelig streng, og vi er nødt til at **tage abstraktionsniveauet**. Matematik er i bund og grund en slags sprog, der beskriver forholdet mellem operationer og tal, fordi vi er nødt til at håndtere operatørprioriteter, forskellige betydninger, sammenhænge, rekursion og endda datatyper. Det er her, **query tokenization-processen** kommer ind i billedet.

> Jeg har arbejdet på problemet med matematisk tokenisering siden 2015 og har skrevet flere forskellige parsere siden da.

Den bedste af disse, som i øjeblikket driver den nye Mathematicator, er [tilgængelig opensource på GitHub] (https://github.com/mathematicator-core/tokenizer).

Formålet med tokenisering er at **parse en streng**, opdele den i grupper af mindre strenge af kendte typer og derefter konvertere dem til objekter (datatyper). Det konverterede array af objekter konverteres derefter af **clever logik til et binært træ**, der kan beskrive afhængigheder og rekursion. Dette er en meget krævende proces, fordi der er hundredvis af mulige scenarier, og brugerne kan være meget kreative i deres forespørgsler.

Den største fordel ved token arrayet er, at det meget let kan sendes videre til det næste lag, som f.eks. [udfører selve beregningen] (https://github.com/mathematicator-core/calculator) eller [tegner træet om til LaTeX] (https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

Brugen kan se elegant ud på denne måde:

```php
$tokenizer = new Tokenizer(/* nogle afhængigheder */);

// Konverter matematisk formel til et array af tokens:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Nu kan du konvertere tokens til et mere brugbart format:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Returnerer typede tokens med metadata

// Render til LaTeX
echo $tokenizer->tokensToLatex($objectTokens);

// Render til fejlfindingstræet (ekstremt hurtigt):
echo $tokenizer->renderTokensTree($objectTokens);
```

Se procedurer
-----------------

Et stort antal brugere vil sætte pris på, at programmet **når det beregner, viser proceduren** for at vise, hvordan det har gjort det. Dette er faktisk også nyttigt for programmøren, fordi han i det mindste let kan finde ud af, hvor der er en fejl i beregningen, og rette algoritmen i overensstemmelse hermed. Når du kombinerer alt dette med maskinlæring baseret på automatiserede tests, får du noget fantastisk.

Se hvordan `QueryNormalizer` var i stand til at forstå din forespørgsel, videregive dataene til tokenizer, den renderede forespørgslen til LaTeX i overensstemmelse med den og videregav derefter objekttræet til beregneren, som returnerede det samlede resultat.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

Proceduren repræsenteres ved, at beregneren gennemløber indtastningstræet og evaluerer en regel ad gangen i henhold til de tokens og regler, som det indeholder. Når en regel evalueres, anbringes trinoplysningerne i et array. Indimellem kan et trin vise sig at være forkert, og vi er nødt til at gå tilbage og tage en anden vej i beregningen, men der ligger en hel del magi bag, som vil forblive skjult indtil videre, og du kan studere det i implementeringen.

Konklusion
-----

Ovenstående procedure beskriver, hvordan man elegant håndterer matematiske udtryk, hvor vi har tal, operationer og relationer med dem. Denne fremgangsmåde kan f.eks. ikke ændre udtryk eller løse ligninger, men det ser vi på næste gang.

*Hvis du har andre ideer til, hvordan man kan behandle matematik effektivt, hører jeg gerne fra dig.*
