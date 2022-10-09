Lokalt følsom hashing
=====================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	da: lokalt-folsom-hashing
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

Princippet for de fleste hashing-funktioner til fingeraftryk af dokumenter er, at de altid returnerer det samme output for hvert enkelt input. Dette kaldes deterministisk adfærd. Samtidig vil en lille ændring i input medføre en stor ændring i output (og dermed give en helt anden hash). Dette er især nyttigt, når vi vil kontrollere, om inddatadokumentet er ændret, og hvis det er, får vi noget helt andet. Et eksempel på en sådan funktion er MD5 eller SHA1.

Hvis vi ønsker at udlede indholdet af det oprindelige input fra hash-koden, er der ingen nem måde at gøre dette på, og vi har intet andet valg end at bruge brute force på hver enkelt mulighed, indtil vi får et resultat. Det skyldes, at vi på grund af den store ændring i output ikke kan afgøre ved gentagelser, om vi nærmer os målet eller ej.

Nogle hashing-funktioner såsom BCrypt returnerer et andet output for det samme input hver gang for at gøre dem robuste over for password-hashing. Faktisk indeholder outputtet direkte salt, hvilket gør et såkaldt ordbogsangreb umuligt. Denne funktion er kun nyttig til hashing af adgangskoder, men er meget uegnet til gennemgang af dokumenter.

Kontrol af dokumentlighed og søgning efter indholdsduplikering
-----------------------------------------------------------

Forestil dig, at vi løser en algoritme for en søgemaskine, der ønsker at kontrollere, hvor meget en webside er blevet ændret. Alternativt kan den hurtigt kontrollere, om teksten fra en side ligner teksten på en anden side eller endda er fuldstændig dubleret.

En måde at kontrollere dette på er at sammenligne hver side med hinanden, hvilket er meget ressourcekrævende for systemet. En anden mulighed er at beregne en SHA1-hash, men dette hasher indholdet af hele dokumentet, og hvis siden ændres med blot ét tegn, får vi en anden hash - så det er ikke egnet til disse formål.

En mulig løsning er derfor at beregne hash-koden fra forskellige områder af dokumentet på forskellig vis og derefter kigge efter ændringer i hash-funktionens output.

Princippet om lokalfølsom hashing
----------------------------------

Vi har downloadet HTML-koden til en webside, som vi ønsker at beregne en hash for. Vi opdeler dokumentet i forskellige regioner (celler) ved hjælp af en algoritme, der skal konfigureres korrekt.

Vi hasher hver region uafhængigt af hinanden ved hjælp af en fælles algoritme og sammenkæder resultatet til en enkelt streng.

Output kan så f.eks. være `3791744029724361934` (denne indholdshash blev leveret af Ahrefs-værktøjet).

Hvis vi f.eks. ved, at indholdet af overskriften repræsenterer de første 5 tegn og fodteksten repræsenterer de sidste 5 tegn, ved vi, at midten af strengen repræsenterer sidens indhold. Hvis indholdet i sidefoden ændres på alle sider (f.eks. hvis webmasteren tilføjer et nyt link, opdateringsdatoen ændres osv.), så er det kun nogle få tegn i hash-koden i højre side, der ændres lidt, når den nye version af siden downloades, og vi ved, at kun sidefoden er ændret, men at indholdet er uændret. Så vi kan ignorere en sådan ændring og behøver ikke at gennemgå hele webstedet.

Hvordan optimerer Google webcrawling?
----------------------------------------

Internetrobotter skal spare, hvor de kan. Webcrawling er meget dyrt, og opdateringer koster beregningstid.

Da jeg f.eks. observerede Googles robotalgoritme, kunne jeg konstatere, at den kun reagerer på store ændringer i indholdet. Hvis siden kun ændres lidt, crawles den på normal vis. Men når f.eks. footer og header på et websted ændres væsentligt, vurderes det som et redesign, og det meste af webstedet gennemgås på én gang for at få det nye udseende så hurtigt som muligt.

Den registrerer også dubletter på tværs af websteder på samme måde. Når en gruppe af sider er meget ens eller har samme indhold, får de en meget ens hash, som robotten kan bruge til hurtigt at kontrollere, at dokumenterne ligner hinanden, og kan vælge den kanoniske side og ignorere resten.

Implementering af hashing-funktionen
-----------------------------

Der findes ingen færdigudviklet implementering af den lokalfølsomme hashing-funktion i PHP. Samtidig kender jeg ikke til nogen frit tilgængelig pakke, der implementerer denne funktion godt.
