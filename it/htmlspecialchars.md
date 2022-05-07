Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	it: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() è una funzione per convertire i caratteri speciali in entità HTML.

Descrizione
-----

```php
$promenna = htmlspecialchars($text);
```

Alcuni caratteri speciali hanno un significato speciale per i browser, quindi dovrebbero essere convertiti in entità. Questo impedisce la sicurezza generale degli script ed evita che la pagina sia resa in modo errato.

È più spesso usato per proteggere i moduli e qualsiasi posto dove l'utente inserisce del testo e rischia di inserire dei tag HTML.

| Carattere | Nota | Modifiche a
|------|-------------------------|-----------
| `&` | ampersand | `&amp;`
| `"` | doppia citazione (cambia quando `ENT_NOQUOTES` è disabilitato) | `&quot;`
| `'` | apostrofo (cambia quando `ENT_QUOTES` è abilitato) | `&#039;`
| Meno di, parentesi HTML | `&lt;`
| maggiore di, parentesi HTML, `&gt;`

Parametro
--------

**Stringa** da convertire

**flags** Impostazioni di comportamento diverse

**charset** Specifica il set di caratteri (codifica). Il set di caratteri predefinito è `ISO-8859-1`.

Puoi usare `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252`, e `KOI8-R`.

> Nota: Supporto solo da PHP 4.3.0 e successivi. Qualsiasi altro set di caratteri non è riconosciuto e supportato.

**double_encode** Quando `double_encode` è disabilitato, PHP non codifica le entità HTML esistenti, il default è convertire tutto.

Valori di ritorno
-----------------

Converte la stringa.

Se la stringa contiene unità non valide, all'interno del charset dato in `ENT_IGNORE` (non impostato), viene restituita una stringa vuota.

Cambiamenti nelle versioni
----------------

| Versione | Nota
|-------|---------
| 5.4.0 | Aggiunta delle costanti `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` e `ENT_HTML5`.
| 5.3.0 | Aggiunta della costante `ENT_IGNORE`.
| 5.2.3 | Aggiunta del parametro `double_encode`.
| 4.1.0 | Aggiunta del parametro `charset`.

Esempio
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test</a>',
	ENT_QUOTES
);

echo $new; // <a href="test">Test</a>
```
