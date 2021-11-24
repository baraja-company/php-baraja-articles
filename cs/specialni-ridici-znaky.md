Speciální řídící znaky v PHP
============================

> id: "888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0"
> slug:
> 	cs: specialni-ridici-znaky
> 
> publicationDate: "2021-11-24 14:05:00"
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec

V PHP řetězcích mohou být obsaženy speciální řídící znaky, které mají v určitém kontextu různý význam a nechovají se nutně jako obyčejné znaky.

Řadu z nich budete již intuitivně znát. Některé jsou vyhrazeny pro speciální použití a jiné třeba pro znaky na klávesnici.

Zápis speciálních znaků
-----------------------

Speciální znaky se zapisují do dvojitých uvozovek.

Stačí tedy velmi jednoduše:

```php
$message = "Hello\nworld.";
```

Předchozí kód obsahuje zalomení řádku mezi slovem `Hello` a `world`.

Tabulka speciálních znaků
-------------------------

Pokud je řetězec uzavřen ve dvojitých uvozovkách ("), bude PHP interpretovat následující escape sekvence jako speciální znaky:

| Sekvence | Význam |
|----------|--------|
| `\n`     | linefeed (`LF` nebo `0x0A (10)` v ASCII) |
| `\r`     | carriage return (`CR` nebo `0x0D (13)` v ASCII) |
| `\t`     | horizontal tab (`HT` nebo `0x09 (9)` v ASCII) |
| `\v`     | vertical tab (`VT` nebo `0x0B (11)` v ASCII) |
| `\e`     | escape (`ESC` nebo `0x1B (27)` v ASCII) |
| `\f`     | form feed (`FF` nebo `0x0C (12)` v ASCII) |
| `\\`     | backslash |
| `\$`     | dollar sign (znak dolaru) |
| `\"`     | double-quote (dvojitá uvozovka) |
| `\[0-7]{1,3}` | posloupnost znaků odpovídající regulárnímu výrazu je znak v osmičkové notaci, který se v tichosti přeteče do bajtu. (např. `"\400" === "\000"`) |
| `\x[0-9A-Fa-f]{1,2}` | posloupnost znaků odpovídající regulárnímu výrazu je znak v hexadecimálním zápisu. |
| `\u{[0-9A-Fa-f]+}`   | posloupnost znaků vyhovující regulárnímu výrazu je kódový bod Unicode, který bude vypsán do řetězce jako reprezentace tohoto kódového bodu v UTF-8. |

Stejně jako u řetězců s jednoduchými uvozovkami se při escapování jakéhokoli jiného znaku vypíše i zpětné lomítko.

Při ohraničení řetězců pomocí uvozovek nezapomeňte na fakt, že obsažené proměnné budou expandovány (vypsány hodnoty proměnných přímo do řetězce). Toto chování může být extrémně nebezpečné.
