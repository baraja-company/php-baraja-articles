Was tun, wenn künstliche Intelligenz den Programmierern nächstes Jahr die Arbeit wegnimmt?
==========================================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	de: was-tun-wenn-kuenstliche-intelligenz-den-programmierern-naechstes-jahr-die-arbeit-wegnimmt
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

Die Möglichkeiten des OpeanAI-Sprachmodells sind unglaublich. Geben Sie einfach den Quellcode ein, lassen Sie ihn analysieren, korrigieren und in eine andere Sprache umschreiben, und alles geschieht automatisch.

Als ich es zum ersten Mal ausprobierte, war ich schockiert, was man alles erreichen kann und wie leistungsfähig die Werkzeuge sind, die der Allgemeinheit zur Verfügung stehen.

Schauen wir uns ein Beispiel an:

**Frage:** Finden Sie die Sicherheitslücke in dieser Funktion und stellen Sie den gepatchten Code zur Verfügung:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Januar',
		'Februar',
		'März',
		'April',
		'Mai',
		'Juni',
		'Juli',
		'August',
		'September',
		'Oktober',
		'November',
		'Dezember',
	];

	return $czMonths[(int) date('m')];
}
```

**Antwort:**

Eine der Sicherheitsschwachstellen in dieser Funktion ist die Gefahr eines SQL-Injection-Angriffs. Wenn schlecht formatierte oder bösartige Eingaben an die Funktion übergeben werden, kann die Datenbank kompromittiert oder ausgenutzt werden.

Eine Lösung besteht darin, vordefinierte Monatskonstanten anstelle von Eingaben zu verwenden, die aus einem Aufruf der Funktion date() stammen. Wir können auch die Funktion filter_var() mit einem geeigneten Filter verwenden, um sicherzustellen, dass die Eingabe numerisch ist und keinen bösartigen Code enthält.

Hier ist der korrigierte Code:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Januar',
		'Februar',
		'März',
		'April',
		'Mai',
		'Juni',
		'Juli',
		'August',
		'September',
		'Oktober',
		'November',
		'Dezember',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Frage:** Konvertieren Sie die Funktionsimplementierung in Typescript.

**Antwort:**

Hier ist die neu geschriebene Implementierung der Funktion in TypeScript:

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

**Anfrage:** Dies ist kein gültiges Typescript!

**Antwort:**

Ich entschuldige mich für den Fehler. Hier ist die korrigierte Version der Funktion in TypeScript:

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

Hinweis: In TypeScript gibt es keine filter_var()-Funktion, daher habe ich sie durch den Aufruf der Funktion getMonth() aus der Date-Klasse ersetzt und dann mit der Funktion Number() in eine Zahl konvertiert. Diese Version der Funktion sollte in TypeScript gültig sein.

**Frage:** Enthält Ihre Implementierung von Typescript eine Sicherheitslücke?

**Antwort:**

Meine korrigierte Implementierung der Funktion in TypeScript sollte nicht die Sicherheitslücke eines SQL-Injection-Angriffs enthalten, die in der ursprünglichen PHP-Implementierung vorhanden war. Die Funktion getMonth() der Date-Klasse gibt einen numerischen Wert des Monats zurück (0 für Januar, 1 für Februar usw.), so dass wir keine Eingabe von der Funktion date() übergeben müssen und die Gefahr eines SQL-Injection-Angriffs vermeiden können.

Jede Anwendung kann jedoch Sicherheitslücken enthalten. Daher ist es wichtig, den Code regelmäßig auf potenzielle Sicherheitsprobleme hin zu überprüfen. Wenn Sie eine Sicherheitslücke in Ihrem Code vermuten, können Sie sich an Sicherheitsexperten wenden oder automatische Tools zum Scannen von Sicherheitslücken verwenden.
