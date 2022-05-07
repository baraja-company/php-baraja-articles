Specjalne znaki sterujące w PHP
===============================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	pl: specjalne-znaki-sterujace-w-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

Łańcuchy PHP mogą zawierać specjalne znaki sterujące, które mają różne znaczenie w danym kontekście i niekoniecznie zachowują się jak zwykłe znaki.

Wiele z nich będzie Ci już intuicyjnie znanych. Niektóre z nich są zarezerwowane do specjalnych zastosowań, a inne są zarezerwowane na przykład dla znaków klawiatury.

Pisanie znaków specjalnych
-----------------------

Znaki specjalne są zapisywane w cudzysłowach.

Jest to więc bardzo proste:

```php
$message = "Świat Hello.";
```

Powyższy kod zawiera przerwę między wierszami `Hello` i `world`.

Tabela znaków specjalnych
-------------------------

Jeśli łańcuch jest ujęty w cudzysłów ("), PHP zinterpretuje następujące sekwencje specjalne jako znaki specjalne:

| Sekwencja | Znaczenie |
|----------|--------|
| ``n`` | linefeed (`LF` lub `0x0A (10)` w ASCII) |
| | carriage return (`CR` lub `0x0D (13)` w ASCII) |
| ``t` | tabulator poziomy (`HT` lub `0x09 (9)` w ASCII) |
| ``v` | tabulator pionowy (`VT` lub `0x0B (11)` w ASCII) |
| ``e`` | escape (`ESC` lub `0x1B (27)` w ASCII) |
| `f` | form feed (`FF` lub `0x0C (12)` w ASCII) |
| backslash |
| znak dolara |
| | cudzysłów |
| `[0-7]{1,3}` | Sekwencja znaków odpowiadająca wyrażeniu regularnemu to znak w notacji ósemkowej, który po cichu przepełnia się na bajt. (np. `"400" == "2000"`).
| Sekwencja znaków odpowiadająca wyrażeniu regularnemu jest znakiem w notacji szesnastkowej. | Sekwencja znaków odpowiadająca wyrażeniu regularnemu jest znakiem w notacji szesnastkowej.
| sekwencja znaków pasujących do wyrażenia regularnego jest punktem kodowym Unicode, który zostanie zapisany w łańcuchu jako reprezentacja UTF-8 tego punktu kodowego.

Podobnie jak w przypadku łańcuchów jednokwotowych, przy ucieczce przed jakimkolwiek innym znakiem zostanie użyty odwrotny ukośnik.

Podczas ograniczania łańcuchów cudzysłowami należy pamiętać, że zawarte w nich zmienne zostaną zinterpretowane (wartości zmiennych zostaną zapisane bezpośrednio do łańcucha). Takie zachowanie może być bardzo niebezpieczne.
