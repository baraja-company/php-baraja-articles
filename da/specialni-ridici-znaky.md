Særlige kontroltegn i PHP
=========================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	da: saerlige-kontroltegn-i-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

PHP-strenge kan indeholde specielle kontroltegn, der har forskellige betydninger i en bestemt sammenhæng og ikke nødvendigvis opfører sig som almindelige tegn.

Mange af disse vil allerede være intuitivt velkendte for dig. Nogle er reserveret til særlige formål, og andre er reserveret til f.eks. tastaturtegn.

Skrivning af specialtegn
-----------------------

Særlige tegn skrives i dobbelte anførselstegn.

Så det er meget enkelt:

```php
$message = "Hallo\nworld.";
```

Den foregående kode indeholder et linjeskift mellem `Hello` og `world`.

Tabel over specialtegn
-------------------------

Hvis strengen er omsluttet af dobbelte anførselstegn ("), vil PHP fortolke følgende escape-sekvenser som specialtegn:

| Sekvens | Betydning |
|----------|--------|
| `\n` | linjefod (`LF` eller `0x0A (10)` i ASCII) |
| `\r` | vogn retur (`CR` eller `0x0D (13)` i ASCII) |
| ``t`` | vandret tabulator (`HT` eller `0x09 (9)` i ASCII) |
| ``v` | lodret tabulator (`VT` eller `0x0B (11)` i ASCII) |
| `\e` | escape (`ESC` eller `0x1B (27)` i ASCII) |
| `\f` | form feed (`FF` eller `0x0C (12)` i ASCII) |
| `\\\\\` | skråstreg |
| `\$` | dollartegn |
| `\"` | dobbelt-quote |
| `[0-7]{1,3}` | En sekvens af tegn, der passer til et regulært udtryk, er et tegn i oktal notation, der stiltiende løber over i en byte (f.eks. `"\400" === "\000"`) |
| `\x[0-9A-Fa-f]{1,2}` | Den sekvens af tegn, der svarer til et regulært udtryk, er et tegn i hexadecimal notation. |
| `\u{[0-9A-Fa-f]+}` | sekvensen af tegn, der matcher det regulære udtryk, er et Unicode-kodepunkt, som vil blive output til strengen som en UTF-8-repræsentation af det pågældende kodepunkt.

Ligesom med strings i enkeltkvote vil en skråstreg blive outputet, når andre tegn unddrages.

Når du afgrænser strenge med anførselstegn, skal du huske på, at de indeholdte variabler vil blive ekspanderet (værdierne af variablerne vil blive skrevet direkte til strengen). Denne adfærd kan være yderst farlig.
