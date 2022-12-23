Vad ska vi göra när artificiell intelligens tar jobb från programmerare nästa år?
=================================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	sv: vad-ska-vi-goera-naer-artificiell-intelligens-tar-jobb-fran-programmerare-naesta-ar
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

Möjligheterna med OpeanAI:s språkmodell är otroliga. Det är bara att mata in källkoden, be om analys, korrigering och omskrivning till ett annat språk, och allt sker automatiskt.

När jag provade det första gången blev jag chockad över vad man kan åstadkomma och hur kraftfulla verktygen är tillgängliga för allmänheten.

Låt oss titta på ett exempel:

**Fråga:** Hitta säkerhetssårbarheten i denna funktion och ge den rättade koden:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Januari',
		'Februari',
		'Mars',
		'April',
		'Maj',
		'Juni',
		'Juli',
		'Augusti',
		'September',
		'Oktober',
		'November',
		'December',
	];

	return $czMonths[(int) date('m')];
}
```

**Svar:**

En av säkerhetsbristerna i denna funktion är risken för att utnyttja SQL-injektionsattacker. När dåligt formaterad eller skadlig indata skickas till funktionen kan databasen äventyras eller utnyttjas.

En lösning är att använda fördefinierade månadskonstanter i stället för indata som erhålls från ett anrop till funktionen date(). Vi kan också använda funktionen filter_var() med ett lämpligt filter för att se till att inmatningen är numerisk och inte innehåller skadlig kod.

Här är den korrigerade koden:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Januari',
		'Februari',
		'Mars',
		'April',
		'Maj',
		'Juni',
		'Juli',
		'Augusti',
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

**Fråga:** Konvertera funktionsimplementationen till typescript.

**Svar:**

Här är den omskrivna implementeringen av funktionen i TypeScript:

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

**Förfrågan:** Detta är inte ett giltigt typescript!

**Svar:**

Jag ber om ursäkt för felet. Här är den korrigerade versionen av funktionen i TypeScript:

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

Observera: Det finns ingen funktion filter_var() i TypeScript, så jag ersatte den genom att anropa getMonth() från Date-klassen och sedan konvertera den till ett tal med Number(). Denna version av funktionen bör vara giltig i TypeScript.

**Fråga:** Innehåller din implementering av Typescript en säkerhetsbrist?

**Svar:**

Min korrigerade implementering av funktionen i TypeScript bör inte innehålla den säkerhetsbrist i form av en SQL-injektionsattack som ursprungligen fanns i den ursprungliga PHP-implementeringen. Funktionen getMonth() i Date-klassen returnerar ett numeriskt månadsvärde (0 för januari, 1 för februari osv.), så vi behöver inte skicka in data från date()-funktionen och kan undvika faran för en SQL-injektionsattack.

Alla program kan dock innehålla säkerhetsbrister, så det är viktigt att regelbundet granska koden för att leta efter potentiella säkerhetsproblem. Om du misstänker en säkerhetsbrist i din kod kan du kontakta säkerhetsexperter eller använda automatiska verktyg för att skanna säkerhetsbrister.
