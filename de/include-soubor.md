Einschließen (Falten von Seiten aus Stücken)
============================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	de: einschliessen-falten-von-seiten-aus-stuecken
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP ist ursprünglich eine Schablonensprache, die entwickelt wurde, um das Zusammensetzen von Seiten zu erleichtern.

Unterstützte Formate
-------------------

Folding funktioniert wie Text, daher ist es ratsam, entsprechende Formate wie `.html` oder `.md` zu verwenden.

Wenn eine PHP-Datei eingefügt wird, wird ihr Inhalt so ausgeführt, als ob sie physisch an der eingefügten Stelle vorhanden wäre.

Falten von Seiten und Einfügen von gemeinsamen Inhalten
---------------------------------------------

Oft müssen wir mehrere Seiten erstellen, die einen gemeinsamen Inhalt haben - zum Beispiel ein Menü.

In einfachem HTML würden wir zunächst eine Seite mit einem Menü erstellen und diese dann mehrfach kopieren. Aber in PHP können wir den gesamten Prozess automatisieren.

Wir haben eine Datei `menu.html`, in der der Inhalt des Menüs steht, und eine Datei `index.php`, in der wir den Inhalt und das Menü ablegen.

Ein einfaches Beispiel:

```php
<div class="Seite">
    <div class="Inhalt">
        <?php
            include __DIR__
            . '/Artikel/' . ($_GET['Seite'] ?? 'Index') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menü.html';
        ?>
    </div>
</div>
```

Dieses Skript fügt automatisch den Seiteninhalt aus dem Verzeichnis `/article` ein und liest den Dateinamen entsprechend der Benutzereingabe (URL-Parameter `?page=...`). Wenn kein Parameter übergeben wird, wird "index.html" verwendet.

Die URL könnte also beispielsweise so aussehen: "beispiel.com?page=contacts" und "/article/contacts.html" laden.
