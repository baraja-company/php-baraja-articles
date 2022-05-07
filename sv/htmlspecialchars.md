Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	sv: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() är en funktion för att konvertera specialtecken till HTML-enheter.

Beskrivning
-----

```php
$promenna = htmlspecialchars($text);
```

Vissa specialtecken har en speciell betydelse för webbläsare och bör därför omvandlas till enheter. Detta förhindrar allmän säkerhet för skript och förhindrar att sidan återges felaktigt.

Det används oftast för att skydda formulär och alla ställen där användaren lägger in text och riskerar att lägga in HTML-taggar.

| Tecken | Anmärkning | Ändringar i
|------|-------------------------|-----------
| `&` | ampersand | `&amp;`
| `"` | dubbla citationstecken (ändras när `ENT_NOQUOTES` är inaktiverat) | `&quot;`
| `'` | apostrof (ändras när `ENT_QUOTES` är aktiverat) | `&#039;`
| `<` | mindre än, HTML-parentes | `&lt;`
| `>` | större än, HTML-parentes | `&gt;`

Parameter
--------

**sträng** som ska konverteras

**flags** Olika inställningar för beteende

**charset** Anger teckenuppsättning (kodning). Standardteckensättningen är `ISO-8859-1`.

Du kan använda `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252` och `KOI8-R`.

> Obs: Stöd endast från PHP 4.3.0 och senare. Alla andra teckenuppsättningar känns inte igen och stöds inte.

**double_encode** När `double_encode` är inaktiverat kommer PHP inte att koda befintliga HTML-enheter, standard är att konvertera allt.

Returvärden
-----------------

Konverterar strängen.

Om strängen innehåller ogiltiga enheter, inom den givna teckenuppsättningen i `ENT_IGNORE` (ej inställd), returneras en tom sträng.

Ändringar i versioner
----------------

| Version | Anteckning
|-------|---------
| 5.4.0 | Tillägg av konstanterna `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` och `ENT_HTML5`.
| 5.3.0 | Tillägg av konstanten `ENT_IGNORE`.
| 5.2.3 | Parametern `double_encode` läggs till.
| 4.1.0 | Parametern `charset` läggs till.

Exempel
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test</a>',
	ENT_QUOTES
);

echo $new; // <a href="test">Test</a>
```
