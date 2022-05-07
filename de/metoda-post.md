Empfang von Daten mit der POST-Methode
======================================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	de: empfang-von-daten-mit-der-post-methode
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

Das Senden von Daten per POST unterscheidet sich deutlich von <a href="/method-get">GET</a>, es ist sicherer, der Text kann länger sein und sein Wert kann nicht bestimmt werden, außer durch ein Formular oder eine Kopfzeile (die Sie nicht versehentlich erhalten werden).

Quelle
--------------------------

Source unterscheidet sich nicht wesentlich von der Methode <a href="/method-get">GET</a>. Es ist so ziemlich das Gleiche, nur dass die Parameter nicht in der URL angezeigt werden, sondern nur der Dateiname sichtbar ist.

```php
echo $_POST['Artikel'] ?? '';
```

Merkmale, Vorteile und Nachteile
--------------------------

- Parameter können nicht verknüpft werden, aber ein Formular muss eingereicht werden
- Kann nicht indiziert werden (bezieht sich auf den vorherigen Punkt)
- Sicherer als <a href="/method-get">GET</a> (Daten werden versteckt gesendet, Werte werden nicht in der Historie angezeigt)
- Nicht zu verwechseln mit SSL, POST ist nicht verschlüsselt
