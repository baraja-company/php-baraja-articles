Asynchrone Weltsicht
====================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	de: asynchrone-weltsicht
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

Mir ist schon lange aufgefallen, dass unsere Welt asynchron und dezentralisiert ist. Wenn man sich dessen bewusst wird und darüber nachdenkt, wie man es zu seinem Vorteil nutzen kann, kann sich leicht ein ganzes, großartiges Konzept für die Lösung komplexer Probleme herausbilden. In diesem Beitrag möchte ich einige der Ideen erläutern, die ich bereits verwende. Sie stammen nicht aus einer bestimmten Quelle, sondern sind eher eine Kombination aus verschiedenen Erfahrungen und meinen eigenen Ideen. Diese Grundsätze gelten nicht für alle Fälle.

Definition des Umfelds und der Ziele
-------------------------

Fast alle Projekte, die ich je in Angriff genommen habe (entweder allein oder als Teil eines Teams), hatten ein ziemlich genau definiertes Ziel. Dieser Ansatz ist sehr sinnvoll, denn es ist wichtig zu wissen, was wir wollen. Andererseits glaube ich, dass es in einem komplexen Umfeld unmöglich ist, ein bestimmtes Ziel zu definieren, und oft stellen wir bei der Umsetzung fest, dass wir eigentlich woanders hinwollen.

Anstelle eines bestimmten Ziels baue ich also einen Bereich verschiedener Ziele auf, zu denen es Alternativen gibt, oder sogar ein völlig neues, isoliertes, unabhängiges Ziel kann das richtige Ziel sein, wenn es Sinn macht.

Beispiele aus der Praxis:

- Ich muss schrittweise in andere Märkte expandieren. Eines der Unterziele, die ich bei der Umsetzung entdeckt habe, könnte die maschinelle Übersetzung des Internets sein.
- Ich muss 6 Sendungen an verschiedenen Orten in Prag abholen und kenne nicht den kürzesten Weg. Ich will es nicht auf komplizierte Weise herausfinden, also gehe ich einfach zum am weitesten entfernten Punkt und ändere meinen Plan für andere Routen auf dem Weg. Wenn ich zum Beispiel in Anděl umsteige, entscheide ich mich für die Straßenbahn, die zuerst ankommt, was sich dynamisch auf den nächsten Routenplan auswirkt.
- Ich muss diesen Beitrag schreiben, aber ich habe nicht eine Stunde Zeit am Stück. Deshalb kann ich es in Teilen schreiben, während ich mit der U-Bahn fahre, als einzelne Kapitel. Die endgültige Zusammenführung in dieses Formular werde ich dann in Zukunft vornehmen.
- Ich weiß nicht, wie der Algorithmus zur Berechnung des Warenrückkaufs für meinen Kunden funktioniert, den ich neu programmieren muss. Also stelle ich die Anfrage an mehrere Personen gleichzeitig und löse währenddessen etwas anderes. Asynchron über 2 Tage hinweg werden mehrere verschiedene Antworten eingehen, auf deren Grundlage ich erst später eine Entscheidung treffen werde.
- Ich muss PHP auf dem Server für eine lange Zeit loswerden, also schreibe ich nach und nach eine Seite nach der anderen in React um. Ich verlagere die Websites nach und nach in die Cloud und baue sie auf statisch generiertem Nect auf.

Keines der Ziele ist ein Endziel. Wenn Sie mit einer Fläche und nicht mit einem Punkt arbeiten, ist es viel wahrscheinlicher, dass Sie ihn treffen. Gleichzeitig funktioniert die Motivation hier sehr gut, denn das Schlimmste, was passieren kann, ist, dass man sein Ziel erreicht und dann nichts mehr hat, worauf man sich in Zukunft freuen kann.

Es ist auch wichtig zu sagen, dass man ein relativ stabiles Umfeld braucht, in dem man Fehler machen kann, um eine ähnliche Denkweise zu haben. Dieser Ansatz kann keine bestimmte Frist für die Lösung einer bestimmten Einzelaufgabe garantieren. Andererseits kann es die Lösung möglichst vieler paralleler Aufgaben in möglichst kurzer Zeit optimieren, wenn auch nicht unbedingt in der Reihenfolge, in der sie in der Warteschlange angekommen sind.

Das Prinzip ist vergleichbar mit der Art und Weise, wie Berechnungen beispielsweise bei der parallelen Programmierung ablaufen. Kurz gesagt, Sie zerlegen die Aufgaben in einzelne Threads, die verschiedene Teilaufgaben auf einmal lösen, und sobald Sie alle Antworten erhalten haben, können Sie die Lösung zusammenstellen.

Bereitstellung neuer Softwareversionen
--------------------------------

Damit habe ich überall zu kämpfen.

Die Softwareentwicklung läuft normalerweise so ab, dass man eine stabile Version hat, die für die Benutzer produktiv ist, dann gibt es eine Testumgebung, in der neue Entwicklungen durchgeführt werden, und von Zeit zu Zeit werden Neuigkeiten aus der Testumgebung in die Produktionsumgebung übertragen, was als Release bezeichnet wird. Dieses Verfahren ist relativ sicher und langsam.

Da ich allmählich zu einer Mikro-Frontend-Architektur übergegangen bin, bei der das Frontend der Anwendung (das, was der Benutzer sieht) vollständig vom Backend (wo die Berechnungen und die Datenverarbeitung stattfinden) getrennt ist, kann der Freigabeprozess anders funktionieren.

Ich habe noch eine Produktionsumgebung, die immer funktioniert. Daraus wird für jede Aufgabe ein neuer Zweig erstellt, der sich mit einer bestimmten Funktion befasst. Da ich Vercel (einen sehr guten Cloud-Anbieter) zum Hosten des Frontends verwende, kann ich für jeden Entwicklungszweig eine eigene URL haben.

Sobald eine neue Funktion getestet ist, wird sie sofort zusammengeführt und in einem asynchronen Prozess in der Produktion eingesetzt. Daher kann es durchaus vorkommen, dass jeden Tag etwa 15 neue Versionen bereitgestellt werden.

Vieles davon hat mit einer Idee zu tun, die mir auf der LMC vermittelt wurde: "Eine Funktion, die Sie einem Kunden morgen zur Verfügung stellen, auch wenn sie noch nicht perfekt ist, hat für ihn einen viel höheren Wert als eine Funktion, die zwar perfekt ausgetestet ist, aber erst in einem Jahr verfügbar sein wird. Das kann aber nur funktionieren, wenn Sie eine Möglichkeit haben, die Nachrichten schnell zu verbreiten - typischerweise die Webentwicklung.

Was ich am Next.js-Framework (das auf Recto aufbaut) und an Vercel sehr mag, ist das Prinzip, dass man sein GitHub-Repository direkt mit Vercel verbindet, und bei jedem Commit gibt es einen automatischen Build, Tests und dann ein Deployment in die Produktion, wenn alles in Ordnung ist. Der Entwickler muss sich also um nichts kümmern, und die App wird stündlich ohne Aufwand bereitgestellt. Es ist sehr wichtig, Routinevorgänge zu formalisieren und dann zu automatisieren.

Veröffentlichung von Inhalten
----------------

Ich habe höhere Dutzende von Themen und Beiträgen bei mir in Apple Notes aufgeschlüsselt. Zu jedem Thema fallen mir immer wieder neue Ideen ein, die ich aufschreiben und nach Stichworten sortieren kann. Wenn mehrere Notizen zu einem Thema erstellt werden, wandle ich sie in Kapitel um und konvertiere dann die Gruppe der Kapitel in Artikel und FB-Posts.

Merkmale dieses Prinzips:

- Ich weiß nicht im Voraus, wie viele Artikel ich jemals veröffentlichen werde,
- Aber ich weiß auch, dass es eine Menge sein kann,
- So kann ich sicherstellen, dass jeder Beitrag eine Fülle von Informationen, Erkenntnissen und Ideen enthält, die im Laufe der Zeit gereift sind (die Reifung dieses Beitrags dauerte etwa 3 Monate),
- Das ist nachhaltig und macht mir Spaß, weil ich nicht eine ganze Menge Text auf einmal schreiben muss und dabei Gefahr laufe, etwas zu vergessen.

Und dann der Veröffentlichungsprozess.

Ich veröffentliche viele Inhalte zuerst im Stillen im Web, damit Google sie zuerst bemerkt und die Seite indiziert wird. Wenn es eine gute Keyword-Abdeckung hat und ich insgesamt zufrieden damit bin, geht das Thema zum Beispiel in die sozialen Medien.

Bei vielen Themen weiß ich, dass sie nicht von Interesse sind und Sie eher verärgern werden. Gleichzeitig möchte ich sie aber auch online haben, weil jemand in Zukunft danach suchen könnte. In diesem Fall bleibt der Artikel nur im Internet. Ein Beispiel für diese Artikel sind die verschiedenen Overlay-Artikel, die den gesamten Themenbereich verlinken, damit ich möglichst viele Themen auf der Seite abdecken kann. Titelartikel sind oft eher technisch und langweilig. Oder es handelt sich um maschinell erstellte Kategorien, in denen ich mehrere Artikel zu Themen zusammenfasse, um möglichst viele Schlüsselwörter abzudecken, nach denen dann jemand suchen möchte.

Wissen, Bildung, Prüfung
------------------------------

Ich spiele gerne mit Technologien, bei denen nicht von vornherein klar ist, wozu sie eines Tages gut sein werden.

Wie maschinelle Übersetzung. Auf den ersten Blick macht es keinen Sinn, Dutzende von Artikeln für Leser, mit denen ich nicht in Kontakt stehe, in Fremdsprachen zu übersetzen. Andererseits kann es einer der Schritte sein, um in der Zukunft eine Entscheidung zu treffen, für die die Voraussetzungen erfüllt sein müssen.

Im Allgemeinen weiß niemand, wie die Zukunft aussehen wird. Daher halte ich es für äußerst sinnvoll, so viele Möglichkeiten wie möglich abzudecken, die man zumindest oberflächlich verstehen und vielleicht in Zukunft angehen möchte. Es ist gut, Schritte nicht nur als eine gelöste Aufgabe zu betrachten, sondern als einen nie endenden evolutionären Prozess, der kein Endziel hat.

Funktioniert dieser Ansatz auch bei der Durchführung von kundenspezifischen Projekten?
--------------------------------------------------------

Die meiste Zeit nicht.

Sie brauchen einen Kunden, der Ihr Partner ist und mit dem Sie beide die bestmögliche Lösung finden wollen, auch wenn Sie von vornherein wissen, dass Sie diese Lösung erst am Ende herausfinden werden.

In unserer Branche nennt man das agile Entwicklung, und diese Grundsätze bauen darauf auf. Andererseits habe ich festgestellt, dass die agile Entwicklung, wie ich sie kenne, nicht direkt zu mir passt, und ich denke mir gerne gebogene Alternativen aus.

Bei der Agilität sehe ich das Prinzip des Engagements sehr stark in der Frage, wer was innerhalb eines bestimmten Sprints löst. Ich halte es für sinnvoller, die Umgebung so zu gestalten, dass man einen riesigen Rückstau an Aufgaben hat, die je nachdem, was im Moment am dringendsten benötigt wird oder wozu ich geistig in der Lage bin, gelöst werden. Wenn ich z. B. unterwegs bin, neige ich dazu, mehr Entwicklungsaufgaben zu lösen, die ich draußen ohne Computer erledigen kann. Wenn ich wieder im Büro bin, nehme ich mehr algorithmus- und rechenintensive Aufgaben in Angriff, weil ich mich voll konzentrieren und viel geistige Energie investieren kann.

Ich empfehle dieses Prinzip nicht, wenn Sie einen brandneuen E-Shop einrichten oder Ihr Unternehmen gerade pleite geht. Sie brauchen eine stabile Umgebung, die von selbst läuft, und schon sind Sie ein Schmuckstück. Gleichzeitig wissen Sie aber auch, dass Ihr stabiles Umfeld eines Tages zu Ende sein wird, wenn Sie nichts unternehmen.
