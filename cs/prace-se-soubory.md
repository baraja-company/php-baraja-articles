Práce se soubory
================

> id: "12330cfb-5729-419a-b2e4-cb3d1db998cc"
> slugCS: prace-se-soubory
> publicationDate: "2020-02-16 16:30:05"
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f

V PHP existuje celá řada funkcí pro práci se soubory.

- <a href="/fopen">fopen()</a>, low-level přístup k souborům na disku
- <a href="/file-get-contents">file_get_contents()</a>, načtení obsahu souboru nebo URL
- <a href="/file-put-contents">file_put_contents()</a>, uložení řetězce do lokálního souboru.

Diskové funkce
--------------

- `unlink($path)`, smazání souboru
- `copy($from, $to)`, kopírování souboru z jednoho místa na druhé (může být i z URL na lokální disk)
- `rename()`, přejmenování nebo přesun souboru na disku
- `chmod()`, změna oprávnění pro čtení, zápis nebo spouštění souboru
