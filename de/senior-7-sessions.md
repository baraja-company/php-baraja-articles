Wie man mit plötzlichen PHP-Skriptabstürzen umgeht
==================================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	de: wie-man-mit-ploetzlichen-php-skriptabstuerzen-umgeht
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

Eine Geschichte von Ende 2016, als ich buchstäblich von einem Kollegen gerettet wurde: In einer PHP-Anwendung beschließt man, Bilder über ein Proxy-Skript einzuchecken, das unter anderem ihre Abmessungen und andere Parameter entsprechend der eingehenden Anfrage anpassen kann. Im Rahmen der Optimierung sichern Sie die generierten Varianten auch physisch auf der Festplatte.

Doch im Produktionsbetrieb kommt es plötzlich zu einer enormen Belastung und Tausenden von Anfragen in der Warteschlange. Die Bilder werden nacheinander für jeden Benutzer geladen. Das Aktualisieren von Seiten und das Anklicken von Links funktioniert nicht. Die Anwendung scheint völlig eingefroren zu sein. Man kann nur warten, bis alles verarbeitet ist.

Was könnte das Problem sein? Ich habe 3 wichtige Hinweise im Text aufgeführt, um eine schnelle Suche nach dem Problem zu ermöglichen. Hotfix hat eine triviale Lösung.
