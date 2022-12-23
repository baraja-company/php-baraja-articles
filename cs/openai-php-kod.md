Co dělat, až příští rok vezme umělá inteligence práci programátorům?
====================================================================

> id: "0eccb5c1-9ab4-42bf-b27e-13917d616b7f"
> slug:
>   cs: openai-php-kod
>
> publicationDate: "2022-12-23 13:30:00"
> mainCategoryId: "483db7b7-5699-41fb-ba0b-d2b653bacd1f"

Možnosti jazykového modelu OpeanAI jsou neskutečné. Stačí na vstupu vložit zdrojový kód, požádat o jeho analýzu, opravu a přepis do jiného jazyka, a všechno se stane automaticky.

Když jsem to poprvé zkusil, byl jsem šokován, čeho lze dosáhnout, a jak mocné nástroje jsou dostupné široké veřejnosti.

Pojďme se podívat na příklad:

**Zadání:** Najdi bezpečnostní zranitelnost v této funkci, a uveď opravený kód:

```php
function getThisMonthName(): string
{
	$czMonths = [
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

	return $czMonths[(int) date('m')];
}
```

**Odpověď:**

Jednou z bezpečnostních zranitelností v této funkci je nebezpečí zneužití SQL injection útoku. Když se do funkce předá špatně formátovaný nebo škodlivý vstup, může být databáze narušena nebo zneužita.

Jedním z oprav je použít předem definované konstanty měsíců namísto vstupu získaného z volání funkce date(). Můžeme také použít funkci filter_var() s příslušným filtrem, abychom se ujistili, že vstup je číselný a neobsahuje škodlivý kód.

Zde je opravený kód:

```php
function getThisMonthName(): string
{
	$czMonths = [
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
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Zadání:** Převeď implementaci funkce do typescriptu.

**Odpověď:**

Zde je přepsaná implementace funkce v TypeScriptu:

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

**Zadání:** Toto není validní typescript!

**Odpověď:**

Omlouvám se za chybu. Zde je opravená verze funkce v TypeScriptu:

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

Poznámka: V TypeScriptu není funkce filter_var(), takže jsem ji nahradil voláním funkce getMonth() z třídy Date a následným převedením na číslo pomocí funkce Number(). Tato verze funkce by měla být v TypeScriptu validní.

**Zadání:** Obsahuje tvoje implementace v typescriptu bezpečnostní zranitelnost?

**Odpověď:**

Moje opravená implementace funkce v TypeScriptu by neměla obsahovat bezpečnostní zranitelnost zneužití SQL injection útoku, která byla původně přítomna v původní implementaci v PHP. Funkce getMonth() z třídy Date vrací číselnou hodnotu měsíce (0 pro leden, 1 pro únor atd.), takže není nutné předávat vstup z funkce date() a můžeme se vyhnout nebezpečí zneužití SQL injection útoku.

Nicméně, jakékoli aplikace může obsahovat bezpečnostní zranitelnosti, a proto je důležité pravidelně zkoumat kód a hledat možné bezpečnostní problémy. Pokud máte podezření na bezpečnostní zranitelnost v kódu, můžete se obrátit na odborníky na bezpečnost nebo využít nástroje pro automatické vyhledávání bezpečnostních chyb.
