Co zrobić, gdy w przyszłym roku sztuczna inteligencja odbierze pracę programistom?
==================================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	pl: co-zrobic-gdy-w-przyszlym-roku-sztuczna-inteligencja-odbierze-prace-programistom
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

Możliwości modelu językowego OpeanAI są niesamowite. Wystarczy wprowadzić kod źródłowy, poprosić o jego analizę, korektę i przepisanie na inny język, a wszystko dzieje się automatycznie.

Kiedy po raz pierwszy spróbowałem, byłem zszokowany tym, co można osiągnąć i jak potężne narzędzia są dostępne dla ogółu społeczeństwa.

Przyjrzyjmy się przykładowi:

**Pytanie:** Znajdź lukę bezpieczeństwa w tej funkcji i podaj załatany kod:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Styczeń',
		'Luty',
		'Marzec',
		'Kwiecień',
		'Maj',
		'Czerwiec',
		'Lipiec',
		'Sierpień',
		'Wrzesień',
		'Październik',
		'Listopad',
		'Grudzień',
	];

	return $czMonths[(int) date('m')];
}
```

**Odpowiedź:**

Jedną z luk bezpieczeństwa w tej funkcji jest niebezpieczeństwo wykorzystania ataku SQL injection. Kiedy źle sformatowane lub złośliwe dane wejściowe są przekazywane do funkcji, baza danych może zostać naruszona lub wykorzystana.

Jedną z poprawek jest użycie predefiniowanych stałych miesiąca zamiast danych wejściowych uzyskanych z wywołania funkcji date(). Możemy również użyć funkcji filter_var() z odpowiednim filtrem, aby upewnić się, że dane wejściowe są numeryczne i nie zawierają złośliwego kodu.

Oto poprawiony kod:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Styczeń',
		'Luty',
		'Marzec',
		'Kwiecień',
		'Maj',
		'Czerwiec',
		'Lipiec',
		'Sierpień',
		'Wrzesień',
		'Październik',
		'Listopad',
		'Grudzień',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Pytanie:** Konwertuj implementację funkcji na typescript.

**Odpowiedź:**

Oto przepisana implementacja funkcji w TypeScript:

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

**Request:** To nie jest prawidłowy skrypt!

**Odpowiedź:**

Przepraszam za błąd. Oto poprawiona wersja funkcji w TypeScript:

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

Uwaga: W TypeScript nie ma funkcji filter_var(), więc zastąpiłem ją wywołując getMonth() z klasy Date, a następnie konwertując ją na liczbę za pomocą Number(). Ta wersja funkcji powinna być poprawna w TypeScript.

**Pytanie:** Czy Twoja implementacja Typescript zawiera lukę bezpieczeństwa?

**Odpowiedź:**

Moja poprawiona implementacja funkcji w TypeScript nie powinna zawierać luki bezpieczeństwa w postaci exploita ataku SQL injection, który występował w oryginalnej implementacji PHP. Funkcja getMonth() klasy Date zwraca numeryczną wartość miesiąca (0 dla stycznia, 1 dla lutego itd.), więc nie musimy przekazywać danych wejściowych z funkcji date() i możemy uniknąć niebezpieczeństwa exploita ataku SQL injection.

Jednak każda aplikacja może zawierać luki w zabezpieczeniach, dlatego ważne jest okresowe badanie kodu w poszukiwaniu potencjalnych problemów z bezpieczeństwem. Jeśli podejrzewasz, że w Twoim kodzie znajduje się luka bezpieczeństwa, możesz skontaktować się z ekspertami ds. bezpieczeństwa lub skorzystać z automatycznych narzędzi do skanowania luk w zabezpieczeniach.
