Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	da: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() er en funktion til at konvertere specialtegn til HTML-enheder.

Beskrivelse
-----

```php
$promenna = htmlspecialchars($text);
```

Nogle specialtegn har en særlig betydning for browsere, så de bør konverteres til enheder. Dette forhindrer generel scriptsikkerhed og forhindrer, at siden bliver gengivet forkert.

Det bruges oftest til at beskytte formularer og alle steder, hvor brugeren indsætter tekst og risikerer at indsætte HTML-tags.

| Tegn | Bemærk | Ændringer til
|------|-------------------------|-----------
| `&` | ampersand | `&amp;`
| `"` | dobbelt citationstegn (ændres, når `ENT_NOQUOTES` er deaktiveret) | `&quot;`
| `'` | apostrof (ændres, når `ENT_QUOTES` er aktiveret) | `&#039;`
| `<` | mindre end, HTML parentes | `&lt;`
| `>` | større end, HTML-bøjle | `&gt;`

Parameter
--------

**String** skal konverteres

**flags** Forskellige indstillinger for adfærd

**charset** Angiver tegnsættet (kodning). Standardtegnsættet er `ISO-8859-1`.

Du kan bruge `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252` og `KOI8-R`.

> Bemærk: Understøttes kun fra PHP 4.3.0 og senere. Alle andre tegnsæt anerkendes og understøttes ikke.

**double_encode** Når `double_encode` er deaktiveret, vil PHP ikke kode eksisterende HTML-enheder, standard er at konvertere alt.

Returneringsværdier
-----------------

Konverter streng.

Hvis strengen indeholder ugyldige enheder inden for det angivne charset i `ENT_IGNORE` (ikke indstillet), returneres en tom streng.

Ændringer i versioner
----------------

| Version | Bemærk
|-------|---------
| 5.4.0 | Tilføjelse af konstanterne `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` og `ENT_HTML5`.
| 5.3.0 | Tilføjelse af konstanten `ENT_IGNORE`.
| 5.2.3 | Tilføjelse af parameteren `double_encode`.
| 4.1.0 | Tilføjelse af parameteren `charset`.

Eksempel
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test</a>',
	ENT_QUOTES
);

echo $new; // <a href="test">Test</a>
```
