Verarbeitung von Vorschaubildern und Metainformationen von Vimeo
================================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	de: verarbeitung-von-vorschaubildern-und-metainformationen-von-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

Wenn wir Videos von Vimeo in eine Seite einbetten (als HTML-Einbettung), möchten wir oft auch das Bild und andere nützliche Informationen wie die Länge des Videos, den vollständigen Titel, den Autor und so weiter erhalten.

Glücklicherweise bietet Vimeo eine einfache HTTP-API, über die wir alle Daten auf der Grundlage des Video-Tokens lesen können.

Um zu vermeiden, dass Sie die API selbst schreiben müssen, verwenden Sie einfach [ready package](https://github.com/baraja-core/vimeo-video-api), das die API vollständig integriert.

Sie installieren das Paket mit dem Befehl:

```shell
composer require baraja-core/vimeo-video-api
```

Es ist einfach zu bedienen. Sie erstellen eine Instanz des Dienstes `Baraja\VimeoAPI\VimeoVideoAPI`, um gemäß der Dokumentation mit Vimeo zu kommunizieren, rufen die Methode `getInfo()` auf, übergeben das Video-Token und erhalten detaillierte Informationen als `VideoInfo`-Entität, aus der alle verfügbaren Informationen gelesen werden können (nicht alle Informationen sind immer für jedes Video verfügbar).

Auf diese Weise können Sie auch private und öffentlich nicht verfügbare Videos abfragen. Aber Sie müssen immer ihr Token kennen.

Auflistung aller verfügbaren Informationen
---------

Eine einfache Möglichkeit, die Bibliothek zu nutzen, sieht folgendermaßen aus:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Video-Token als Ganzzahl
$info = $api->getInfo($token);

echo var_dump($info); // listet alles auf

// Geben Sie die Länge des Videos in Sekunden aus:
echo 'Die Länge des Videos beträgt:' . $info->getDuration();
```

Die Variable "$info" speichert alle beschreibenden Informationen über ein bestimmtes Video. Eine Übersicht über alle verfügbaren Methoden [finden Sie in der Implementierung] (https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
