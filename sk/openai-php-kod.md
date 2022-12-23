Čo robiť, keď umelá inteligencia na budúci rok pripraví programátorov o prácu?
==============================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	sk: co-robit-ked-umela-inteligencia-na-buduci-rok-pripravi-programatorov-o-pracu
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

Možnosti jazykového modelu OpeanAI sú neuveriteľné. Stačí zadať zdrojový kód, požiadať o jeho analýzu, opravu a prepísanie do iného jazyka a všetko sa uskutoční automaticky.

Keď som to prvýkrát vyskúšal, bol som šokovaný, čo všetko sa dá dosiahnuť a aké výkonné nástroje sú dostupné širokej verejnosti.

Pozrime sa na príklad:

**Otázka:** Nájdite bezpečnostnú chybu v tejto funkcii a poskytnite opravený kód:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Január',
		'Február',
		'Marec',
		'apríl',
		'Máj',
		'jún',
		'Júl',
		'August',
		'September',
		'Október',
		'November',
		'December',
	];

	return $czMonths[(int) date('m')];
}
```

**Odpoveď:**

Jednou z bezpečnostných chýb tejto funkcie je nebezpečenstvo zneužitia útoku SQL injection. Ak sa funkcii odovzdá zle naformátovaný alebo škodlivý vstup, môže dôjsť k ohrozeniu alebo zneužitiu databázy.

Jednou z opráv je použitie preddefinovaných konštánt mesiaca namiesto vstupu získaného z volania funkcie date(). Môžeme tiež použiť funkciu filter_var() s vhodným filtrom, aby sme zabezpečili, že vstup je číselný a neobsahuje škodlivý kód.

Tu je opravený kód:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Január',
		'Február',
		'Marec',
		'apríl',
		'Máj',
		'jún',
		'Júl',
		'August',
		'September',
		'Október',
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

**Otázka:** Preveďte implementáciu funkcie do typového skriptu.

**Odpoveď:**

Tu je prepísaná implementácia funkcie v jazyku TypeScript:

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

**Požiadavka:** Toto nie je platný typový skript!

**Odpoveď:**

Ospravedlňujem sa za chybu. Tu je opravená verzia funkcie v jazyku TypeScript:

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

Poznámka: V jazyku TypeScript neexistuje funkcia filter_var(), preto som ju nahradil volaním funkcie getMonth() z triedy Date a jej následným prevodom na číslo pomocou funkcie Number(). Táto verzia funkcie by mala byť platná v jazyku TypeScript.

**Otázka:** Obsahuje vaša implementácia Typescriptu bezpečnostnú chybu?

**Odpoveď:**

Moja opravená implementácia funkcie v jazyku TypeScript by nemala obsahovať bezpečnostnú chybu zneužitia útoku SQL injection, ktorá bola pôvodne prítomná v pôvodnej implementácii PHP. Funkcia getMonth() triedy Date vracia číselnú hodnotu mesiaca (0 pre január, 1 pre február atď.), takže nemusíme odovzdávať vstup z funkcie date() a môžeme sa vyhnúť nebezpečenstvu zneužitia útoku SQL injection.

Každá aplikácia však môže obsahovať bezpečnostné chyby, preto je dôležité pravidelne kontrolovať kód a hľadať potenciálne bezpečnostné problémy. Ak máte podozrenie na bezpečnostnú zraniteľnosť vo svojom kóde, môžete sa obrátiť na bezpečnostných expertov alebo použiť automatizované nástroje na skenovanie bezpečnostných zraniteľností.
