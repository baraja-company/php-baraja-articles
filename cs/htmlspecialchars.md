Htmlspecialchars
================

> id: "46f04729-3956-4889-bb40-58362cb46b2a"
> slugCS: htmlspecialchars
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0743dd02-12d0-4766-ba42-8fd7e9c4ae8a"

Htmlspecialchars() je funkce pro převod speciálních znaků na HTML entity.

Popis
-----

```php
$promenna = htmlspecialchars($text);
```

Některé speciální znaky mají speciální význam pro prohlížeče, proto by se měli převést na entity. Zabraňuje to obecné bezpečnosti scriptů a nedochází k tomu, že se stránka špatně vykreslí.

Nejčastěji se používá na ochranu formulářů a všech místech, kde uživatel vkládá text a hrozí vložení HTML značek.

| Znak | Poznámka                | Mění se na
|------|-------------------------|-----------
| `&`  | ampersand               | `&amp;`
| `"`  | dvojitá uvozovka (mění se, když je zakázán `ENT_NOQUOTES`) | `&quot;`
| `'`  | apostrof (mění se, když je povolen `ENT_QUOTES`) | `&#039;`
| `<`  | menší než, HTML závorka | `&lt;`
| `>`  | větší než, HTML závorka | `&gt;`

Parametr
--------

**Řetězec** pro převod

**flags** Různé nastavení chování

**charset** Určuje znakovou sadu (kódování). Výchozí znaková sada je `ISO-8859-1`.

Můžete použít: `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252` a `KOI8-R`.

> Poznámka: Podpora až od PHP 4.3.0 a novější. Jakékoliv jiné znakové sady nejsou rozpoznány a podporovány.

**double_encode** Když je vypnutý `double_encode`, tak PHP nebude kódovat existující HTML entity, výchozí hodnota je převést všechno.

Návratové hodnoty
-----------------

Převede řetězec.

Pokud řetězec obsahuje neplatné jednotky, v rámci daného charset v `ENT_IGNORE` (nenastaveno), tak se vrátí prázdný řetězec.

Změny ve verzích
----------------

| Verze | Poznámka
|-------|---------
| 5.4.0 | Přidání konstant `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` a `ENT_HTML5`.
| 5.3.0 | Přidání konstanty `ENT_IGNORE`.
| 5.2.3 | Přidání parametru `double_encode`.
| 4.1.0 | Přidání parametru `charset`.

Příklad
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test<a>',
	ENT_QUOTES
);

echo $new; // <a href="test">Test<a>
```
