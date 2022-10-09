Flerårig formular
=================

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	da: flerarig-formular
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

Nogle gange er vi nødt til at opdele formularen i flere dele (sider), behandle dem separat og derefter samle dem til et resultat.

Denne artikel beskriver metoder og designmønstre til at gøre dette.

> **Note:**
>
> Spørgsmålet om at opdele en formular i flere trin er meget komplekst, især hvis man vil gøre det godt. Jeg har mødt mange metoder i min levetid, som jeg vil diskutere her. Nogle tilgange ser tiltalende ud, men er naive og fungerer kun i særlige tilfælde. For hver tilgang beskriver jeg, hvornår den giver mening, og hvilke tilgange der ikke giver mening.

Udformning af en teoretisk løsning
-------------------------

Normalt er målet at få de grundlæggende data fra den første formular på den første side, validere dem, gemme dem et "sted" og vise den næste side.

Når brugeren kommer til den sidste side, skal hele formularen indsendes, og input skal behandles.

På hvert trin er det vigtigt altid at validere alle data omhyggeligt og give brugeren mulighed for at springe tilbage gennem siderne, så han/hun kan rette dataene tilbage, hvis han/hun støder på en fejl. Hvis formularen desuden skal gengives betinget på grundlag af allerede indhentede data, er dette en meget krævende proces.

Gennemførelse af selve formularerne
--------------------------------

Vi kan enten selv implementere de enkelte formularer i ren HTML og derefter håndtere behandlingen i PHP eller bruge færdige løsninger som <a href="https://doc.nette.org/cs/3.0/forms">Nette-formularer</a>.

> **Eksempel fra livet:**
>
> Meget ofte sender nybegyndere programmører mig e-mails og stiller tilsyneladende enkle spørgsmål, som der findes en færdig løsning på. F.eks. specifikt om formularbehandling i PHP.
>
> Jeg anbefaler altid at springe helt over manuel behandling og bruge en færdig løsning i stedet. I virkeligheden er det meget kompliceret at implementere korrekt, f.eks. validering af den indtastede e-mail og at 2 passwords inden for 2 felter passer sammen, mens vi ønsker at omdirigere brugeren tilbage til den forudfyldte formular i overensstemmelse med hans data og med fejlmeddelelser i tilfælde af en fejl.
>
> Fordi folk **ikke ved, at de ikke ved, at de ikke ved, at de ikke ved**, og så i stedet for at investere 1 times tid i at lære en færdig løsning på 99,99 % af problemerne, foretrækker de at vælge deres egen løsning, som de bruger snesevis af timer på at fejlfinde, og der er stadig tilfælde, hvor formularer ikke virker, skaber fejl, har sikkerhedshuller og ikke beskytter de indtastede data.

Så målet med dette trin er at implementere flere sider på forskellige URL'er, som vil indeholde tomme formularer.

Jeg anbefaler at implementere hver enkelt formular uafhængigt af de andre (atomisk) og håndtere tilstandspassage på et andet applikationslag. Årsagen er, at hver enkelt formular i praksis vil håndtere datavalidering forskelligt, skrive sine output forskelligt, håndtere fejl forskelligt, og vi vil sandsynligvis udvide eller ændre den med tiden, så vi behøver ikke at kende konteksten for hele processen og ændre snesevis af websteder for at gøre det.

Statsoverførsel
---------------

Når vi behandler den første formular, skal vi først validere de modtagne data, og hvis de er korrekte, skal vi omdirigere brugeren til det andet trin. Det er en god idé at håndtere omdirigering som en HTTP-redirect, fordi det nemt kan ske, at dataene ikke er gyldige, og i så fald vil vi gerne sende brugeren tilbage til den første formular og ikke til det næste trin.

Vi kan grundlæggende gemme tilstande på 4 måder:

**Anbefales ikke:**

- **Lagres ingen steder** og sendes fuldstændigt i URL'en. Dette har den ulempe, at brugeren kan ændre de data, der allerede er sendt i URL'en, og dermed forfalske input. Alternativt kan vi afsløre følsomme oplysninger som f.eks. adgangskoder i URL'en.
- **Føj løbende til <a href="/sessions">sessions</a>**, dvs. indsæt trinvis nyerhvervede data efter nøgle i feltet. Dette har den ulempe, at hvis programmet laver en fejl, sidder brugeren fast i sessionerne og kan ikke løse fejlen på nogen måde (bortset fra at slette cookies, hvilket er ekstremt svært for de fleste), og med en ufærdig formular er der desuden en risiko for, at dataene forbliver præudfyldt og kan ses af andre. Det kan dog være meget værre, hvis sessionen har en meget kort gyldighedsperiode (f.eks. 5 minutter), og brugeren mister dataene fra det første trin, når han udfylder det sidste trin ... det kan blive ret irriterende.

**Anbefales:**

- **Depositering i databasen og overførsel af identifikator**. Første gang formularen sendes, gemmer vi alle de indsamlede data i en databasetabel og genererer en tilfældig identifikator (f.eks. en 10 tegn lang streng), der kan sendes mellem siderne som en parameter. Fordelen ved dette er, at når vi behandler en formular, kan vi skrive de nyligt hentede og validerede data direkte ind i tabellen, og i tilfælde af fejl har vi fysiske sikkerhedskopier af de analyserede formularer og kan handle på dem. Hvis en ordre f.eks. er ufuldstændig, kan vi sende en e-mail til brugeren om, at han/hun ikke har gennemført den, og øge chancen for et salg.
- **Save to currently logged in account** fungerer nøjagtig på samme måde som videresendelse via ID, bortset fra at vi i stedet for en tilfældig identifikator (token) bruger en session til ID'et for den aktuelt loggede bruger (hvis der er en). Fordelen er, at vi kan vise de forudfyldte data til brugeren vilkårligt i fremtiden.

Konklusion
-----

Ingen af de ovennævnte løsninger er perfekte og den eneste rigtige. Jeg kombinerer selv flere tilgange, når jeg arbejder på løsninger i flere trin. Typisk løser jeg f.eks. en indkøbskurv som en databasetabel, som jeg tildeler de data, jeg allerede har indsamlet, og binder den enten til en bruger (hvis han er logget ind) eller til en session (hvis han ikke er logget ind, og vi ikke kender hinanden endnu).
