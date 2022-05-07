Särskilda kontrolltecken i PHP
==============================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	sv: saerskilda-kontrolltecken-i-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

PHP-strängar kan innehålla särskilda kontrolltecken som har olika betydelser i ett visst sammanhang och som inte nödvändigtvis beter sig som vanliga tecken.

Många av dessa är redan intuitivt bekanta för dig. Vissa är reserverade för särskilda ändamål och andra är reserverade för t.ex. tangentbordstecken.

Skriva specialtecken
-----------------------

Specialtecken skrivs inom dubbla citationstecken.

Det är alltså mycket enkelt:

```php
$message = "Hej\nworld.";
```

Den föregående koden innehåller ett radbyte mellan `Hello` och `world`.

Tabell över specialtecken
-------------------------

Om strängen är omsluten av dubbla citattecken (") tolkar PHP följande escape-sekvenser som specialtecken:

| Sekvens | Betydelse |
|----------|--------|
| `\n` | radmatning (`LF` eller `0x0A (10)` i ASCII) |
| `\r` | vagnretur (`CR` eller `0x0D (13)` i ASCII) |
| ``t`` | horisontell tabulering (`HT` eller `0x09 (9)` i ASCII) |
| ``v` | vertikal tabulering (`VT` eller `0x0B (11)` i ASCII) |
| `\e` | escape (`ESC` eller `0x1B (27)` i ASCII) |
| `\f` | form feed (`FF` eller `0x0C (12)` i ASCII) |
| `\\\\` | backslash |
| `\$` | dollartecken |
| `\"` | dubbelcitat |
| `[0-7]{1,3}` | Den teckensekvens som motsvarar ett reguljärt uttryck är ett tecken i oktalnotering som tyst övergår till en byte. (t.ex. `"\400" === "\000"`) |
| `\x[0-9A-Fa-f]{1,2}` | Den sekvens av tecken som motsvarar ett reguljärt uttryck är ett tecken i hexadecimal notation. |
| `\u{[0-9A-Fa-f]+}` | sekvensen av tecken som matchar det reguljära uttrycket är en Unicode-kodpunkt, som kommer att skrivas ut i strängen som en UTF-8-representation av den kodpunkten.

Precis som för strängar med enkel citering kommer ett backslash att skrivas ut när du undviker andra tecken.

När du avgränsar strängar med citationstecken ska du komma ihåg att de ingående variablerna kommer att expanderas (värdena för variablerna kommer att skrivas direkt till strängen). Detta beteende kan vara extremt farligt.
