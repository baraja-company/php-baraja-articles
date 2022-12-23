Hvad skal vi gøre, når kunstig intelligens næste år tager jobbet fra programmører?
==================================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	da: hvad-skal-vi-gore-nar-kunstig-intelligens-naeste-ar-tager-jobbet-fra-programmorer
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

Mulighederne i OpeanAI-sprogmodellen er utrolige. Du skal blot indtaste kildekoden, bede om at få den analyseret, rettet og omskrevet til et andet sprog, og alt sker automatisk.

Da jeg prøvede det første gang, blev jeg chokeret over, hvad man kan opnå, og hvor effektive værktøjer der er tilgængelige for offentligheden.

Lad os se på et eksempel:

**Spørgsmål:** Find sikkerhedshullet i denne funktion, og giv den lappede kode:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Januar',
		'Februar',
		'Marts',
		'April',
		'maj',
		'Juni',
		'Juli',
		'August',
		'September',
		'Oktober',
		'November',
		'December',
	];

	return $czMonths[(int) date('m')];
}
```

**Svar:**

En af sikkerhedshullerne i denne funktion er faren for at udnytte SQL-injektionsangreb. Når dårligt formateret eller skadeligt input sendes til funktionen, kan databasen blive kompromitteret eller udnyttet.

En løsning er at bruge foruddefinerede månedskonstanter i stedet for input fra et kald til funktionen date(). Vi kan også bruge funktionen filter_var() med et passende filter for at sikre, at input er numerisk og ikke indeholder skadelig kode.

Her er den korrigerede kode:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Januar',
		'Februar',
		'Marts',
		'April',
		'maj',
		'Juni',
		'Juli',
		'August',
		'September',
		'Oktober',
		'November',
		'December',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Spørgsmål:** Konverter funktionsimplementeringen til typescript.

**Svar:**

Her er den omskrevne implementering af funktionen i TypeScript:

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if (month === false || month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

**Request:** Dette er ikke et gyldigt typescript!

**Svar:**

Jeg undskylder for fejlen. Her er den korrigerede version af funktionen i TypeScript:

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = Number(new Date().getMonth()) + 1;

	if (month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

Bemærk: Der findes ingen filter_var() funktion i TypeScript, så jeg erstattede den ved at kalde getMonth() fra Date-klassen og derefter konvertere den til et tal ved hjælp af Number(). Denne version af funktionen bør være gyldig i TypeScript.

**Spørgsmål:** Indeholder din implementering af Typescript en sikkerhedssårbarhed?

**Svar:**

Min korrigerede implementering af funktionen i TypeScript bør ikke indeholde den sikkerhedssårbarhed i form af SQL-injektionsangreb, som oprindeligt var til stede i den oprindelige PHP-implementering. getMonth()-funktionen i Date-klassen returnerer en numerisk værdi for måneden (0 for januar, 1 for februar osv.), så vi behøver ikke at sende input fra date()-funktionen og kan undgå faren for SQL-injektionsangreb.

Alle programmer kan dog indeholde sikkerhedshuller, så det er vigtigt at undersøge koden regelmæssigt for at finde potentielle sikkerhedsproblemer. Hvis du har mistanke om en sikkerhedssårbarhed i din kode, kan du kontakte sikkerhedseksperter eller bruge automatiserede værktøjer til scanning af sikkerhedssårbarheder.
