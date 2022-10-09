Forældelse af kode - hvordan man opretholder kompatibilitet
===========================================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	da: foraeldelse-af-kode-hvordan-man-opretholder-kompatibilitet
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

Når man udvikler store systemer (f.eks. virksomhedsapplikationer, delte softwarepakker, biblioteker, ...), hvor flere lag og udviklere kommunikerer med hinanden, opstår problemet med at håndtere udgivelsen af nye kodeversioner.

Lad os se på en eksempelsituation, hvor vi ønsker at udvikle en delt Composer-pakke til et fællesskab af udviklere.

Semantisk versionering
--------------------

Før vi løser problemet med bagud- og fremadkompatibilitet, skal vi finde ud af, hvordan vi kan holde styr på ændringer i softwaren. I øjeblikket (2022) er den bedste måde at versionere alle ændringer på, at det sker i Git. Softwarerepositoriet kan f.eks. deles via GitHub eller GitLab. Hver softwareændring har en unik identifikator, der identificerer hvert commit og beskriver, hvad der faktisk er sket.

Følgende strategi har fungeret godt for mig, når jeg har udviklet biblioteker:

I begyndelsen af udviklingen oprettes et indledende commit i `master` (eller `main`) grenen, hvor den underliggende filstruktur er commitet.

For hver ny anmodning oprettes der en separat gren fra master, som der skal arbejdes i. Når ændringen er klar, sendes en sammenlægningsanmodning til master i form af en `Pull request`. Der foretages en kodegennemgang af anmodningen, og hvis alt er i orden, bliver ændringen slået sammen med master-versionen.

Hvis grenen indeholder en bagudkompatibel ændring (BC break, fra `Back Compatibility Break`), skal dette markeres i overensstemmelse hermed. Metoden til markering af BC-pauser behandles i de følgende kapitler.

Produktionsversionen af biblioteket er derefter tagget ved hjælp af tags, der har følgende struktur (baseret på **Semantic Versioning 2.0.0**):

Vi skriver versionsnummeret i formatet `MAJOR.MINOR.PATCH`. Forhøjelsen af versionsnumrene sker på følgende måde:

- `MAJOR` - når der er en ændring, som ikke er bagudkompatibel med andre (API)
- `MINOR` - når der tilføjes funktionalitet, samtidig med at bagudkompatibilitet opretholdes
- `PATCH` - når en fejl er rettet og bagudkompatibilitet opretholdes

Ved at bruge præudgivelser og tilføje metadata er det muligt at præcisere oplysningerne. For eksempel: `1.0.0.0-alpha`, `1.0.0.1-beta+2`.

Du kan læse mere om semantisk versionering på det officielle websted: https://semver.org.

Bagud- og fremadrettet kompatibilitet
-------------------------------

Når du designer software, skal du altid tænke på **bagudkompatibilitet** (nye funktioner og ændringer skal være kompatible med gammel kode) og i nogle tilfælde **fremadkompatibilitet** (nuværende funktioner skal være kompatible med fremtidige ændringer i grænsefladen).

Det er en stor udfordring at løse begge opgaver korrekt. Det er ikke altid muligt at foretage en ændring uden at bryde kompatibiliteten.

Når der foretages ændringer, er det altid nødvendigt at gå trinvis frem og give brugerne tilstrækkelig tid til at reagere på ændringerne.

I de følgende afsnit beskrives det, hvordan man kan tænke på dette.

Trin 1: Markering af en funktion som forældet
--------------------------------------

Den grundlæggende type trussel om kompatibilitet er fjernelse eller omdøbning af en funktion, der eksisterede tidligere. Det skyldes oftest, at de argumenter, som funktionen accepterer, er ændret, eller at der er tale om gammel logik, som skal håndteres anderledes på den nye måde.

I den første fase bør gamle dele af koden markeres som forældede, men ikke ændres på nogen måde.

I PHP er der en annotation `@deprecated` til dette, som skal skrives direkte over metoder, funktioner, egenskaber, variabler, konstanter og generelt al deprecated kode.

Det er også god praksis at skrive en begrundelse for, hvorfor en bestemt ting er forældet, og hvordan den vil blive ændret i fremtiden. Angiv f.eks. navnet på en ny funktion eller en ny anvendelsesmetode.

Et konkret eksempel på, at kode er forældet: Konstanter vil blive fjernet, det er bedre at bruge den indbyggede Enum (BC break på grund af overgangen til en nyere version af PHP):

```php
class OrderNotification
{
	/** @deprecated since 2022-05-24, brug enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'e-mail',
		TYPE_SMS = 'tekst';
```

Annotationen `@deprecated` vil kun give en stille advarsel til IDE (udviklingsværktøj) og kompileringsværktøjer. Det ødelægger ikke noget.

Fase 2: Kaldelse af en ny metode/logik
--------------------------------------

I den anden fase erstatter vi den gamle implementering med den nye, men bruger den nye metode i den gamle implementering. Dette vil hjælpe med at holde grænsefladen kompatibel, uden at brugeren bemærker det.

Eksempel: Metoden er forældet, fordi der i stedet er oprettet en ny statisk tjeneste. Da nogen kan bruge den, markeres den blot som forældet og kalder internt den nye implementering. Udvikleren kan generelt gå ud fra, at metoden vil blive fjernet helt i fremtiden.

```php
/** @deprecated since 2021-09-11 Brug Ip::get() i stedet. */
public static function userIp(): string
{
	return Ip::get();
}
```

Fase 3: Ændring af annotationer til statisk analyse
-------------------------------------------

Hvis du bruger en statisk analyse som PhpStan (kan varmt anbefales!), er det en god idé først at omskrive PHPDoc-annotationerne, før du ændrer datatyperne. Statisk analyse giver brugeren besked om, at noget er i stykker, men køretiden forbliver uberørt.

Fase 4: Smid meddelelsen væk
-----------------------

I den fjerde fase kaldes en ny metode, og samtidig udløses en fejl på `note`-niveau. Programmet fungerer stadig, men det begynder bare gradvist at gemme oplysninger i systemloggen om, at en funktion er forældet og vil blive ændret eller fjernet. Vi vil nu aktivt advare om denne type ændringer. Udvikleren vil se fejl under udvikling eller kompilering.

```php
/** @deprecated since 2021-05-01, brug UserMetaManager i stedet. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . ': Denne metode er forældet, brug UserMetaManager i stedet for.');
	return $this->userMetaManager->get($userId, $key);
}
```

Fase 5: Kast en undtagelse
------------------------

Jeg anbefaler, at du kaster en af de fatale undtagelser, før du helt fjerner metoden. Dette er især vigtigt, fordi programmet vil blive stoppet helt, og fejlen kan ikke ignoreres. I modsætning til at fjerne koden helt, får brugeren besked om, hvad der faktisk skete, og kan nemt rette fejlen.

Fase 6: Fuldstændig fjernelse af koden
-----------------------------

I den sidste fase vil den gamle kode blive fjernet fuldstændigt. Hvis en bruger ikke har rettet afhængighederne, vil hans program være ødelagt.

Alvorlige brud på BC på følsomme områder bør altid foretages i den næste `MAJOR`-udgave og bør påpeges mindst en `MAJOR`-udgave tidligere ved at give en meddelelse. Hvis du ikke gør det, vil det være meget vanskeligt at opdatere biblioteket.
