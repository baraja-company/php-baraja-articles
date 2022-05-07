Špeciálne riadiace znaky v PHP
==============================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	sk: specialne-riadiace-znaky-v-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

Reťazce PHP môžu obsahovať špeciálne riadiace znaky, ktoré majú v určitom kontexte iný význam a nemusia sa správať ako bežné znaky.

Mnohé z nich sú vám už intuitívne známe. Niektoré sú vyhradené na špeciálne použitie a iné sú vyhradené napríklad pre znaky na klávesnici.

Písanie špeciálnych znakov
-----------------------

Špeciálne znaky sa píšu v dvojitých úvodzovkách.

Je to teda veľmi jednoduché:

```php
$message = "Hello\nworld.";
```

Predchádzajúci kód obsahuje medzi slovami `Hello` a `world` zalomenie riadku.

Tabuľka špeciálnych znakov
-------------------------

Ak je reťazec uzavretý v dvojitých úvodzovkách ("), PHP bude interpretovať nasledujúce escape sekvencie ako špeciálne znaky:

| Sekvencia | Význam |
|----------|--------|
| `\n` | linefeed (`LF` alebo `0x0A (10)` v ASCII) |
| `\r` | návrat vozíka (`CR` alebo `0x0D (13)` v ASCII) |
| ``t` | horizontálny tabulátor (`HT` alebo `0x09 (9)` v ASCII) |
| `\v` | vertikálny tabulátor (`VT` alebo `0x0B (11)` v ASCII) |
| `\e` | escape (`ESC` alebo `0x1B (27)` v ASCII) |
| `\f` | Form feed (`FF` alebo `0x0C (12)` v ASCII) |
| `\\\` | spätné lomítko |
| `\$` | znak dolára
| `\"` | dvojité úvodzovky |
| `[0-7]{1,3}` | Sekvencia znakov zodpovedajúca regulárnemu výrazu je znak v osmičkovej notácii, ktorý sa potichu preleje do bajtu. (napr. `"\400" === "\000"`) |
| `\x[0-9A-Fa-f]{1,2}` | Postupnosť znakov zodpovedajúca regulárnemu výrazu je znak v hexadecimálnom zápise. |
| `\u{[0-9A-Fa-f]+}` | postupnosť znakov zodpovedajúca regulárnemu výrazu je kódový bod Unicode, ktorý sa do reťazca vypíše ako reprezentácia tohto kódového bodu v UTF-8.

Rovnako ako pri reťazcoch s jednoduchými úvodzovkami sa pri escapovaní akéhokoľvek iného znaku vypíše spätné lomítko.

Pri ohraničovaní reťazcov úvodzovkami majte na pamäti, že obsiahnuté premenné budú expandované (hodnoty premenných budú zapísané priamo do reťazca). Takéto správanie môže byť veľmi nebezpečné.
