Wie ein Programmierer in einem internen Entwicklungsteam lebt
=============================================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	de: wie-ein-programmierer-in-einem-internen-entwicklungsteam-lebt
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

Für Ehrlichkeit ist ein hoher Preis zu zahlen.

Diese Website war schon immer eine Beschreibung der Realität, die Menschen in der IT-Branche erleben, daher möchte ich auf meine Erfahrungen bei der Arbeit in Entwicklungsteams eingehen. Im Folgenden finden Sie allgemeine Erfahrungen, die ich in verschiedenen Unternehmen gemacht habe. Keine Erfahrung wird mit einem bestimmten Unternehmen in Verbindung gebracht und ist nicht unbedingt als Kritik zu verstehen.

Unternehmen wollen in der Regel keine fleißigen und proaktiven Mitarbeiter
----------------------------------------------

Haben Sie viele Ideen? Wollen Sie innovativ sein? Macht es Ihnen Spaß, elegante Lösungen für die komplexen Probleme zu finden, die Ihr Team löst und die das halbe Unternehmen plagen? Sind Sie sich der Bedeutung von Sicherheit, Softwaredesign und dem Auffinden von Projektengpässen bewusst?

Wahrscheinlich werden Sie im Entwicklungsteam für lange Zeit unglücklich sein.

Teamarbeit ist etwas, mit dem ich mich in letzter Zeit sehr schwer tue - ich meine, ich schwimme geradezu darin und versuche, die für mich völlig unintuitiven Prinzipien zu verstehen, an die ich mich halten soll.

- Teamarbeit bedeutet, eine Lösung zu entwickeln, die vom gesamten Team verstanden wird. Das ist oft nicht das Beste. Es ist fast nie elegant. In der Tat ist es immer ineffizient. Aber es hat den großen Vorteil, dass das ganze Team es versteht und es langfristig verwaltet werden kann. Dies ist ein äußerst wichtiger Gedanke. Denn bei großen Projekten gibt es nicht so viel Druck, effizient zu sein, weil man als Unternehmen durch Mitarbeiter skalieren kann und genug Geld hat. Es ist viel wichtiger, Dinge pünktlich zu liefern, auch wenn sie teilweise kaputt sind und mit vorher unbekannten Einschränkungen, aber pünktlich. Es hat mit der Idee zu tun, dass die Lösung, die Sie morgen liefern (auch wenn sie unvollkommen ist), für den Kunden viel mehr Wert hat als eine 100%ig fehlerfreie Lösung, die erst in einem Jahr verfügbar sein wird.
- Es ist falsch, innovativ zu sein und Dinge zu verdrängen. Das hat damit zu tun, dass es in den Unternehmen echte Menschen gibt, die Familien haben und nur während der Geschäftszeiten arbeiten wollen. Daraus folgt zwangsläufig, dass die Menschen nur begrenzt Zeit haben, um Neues zu lernen und auszuprobieren. Im Grunde genommen müssen die Unternehmen die Arbeitszeit der Mitarbeiter mit Bildungs- und Weiterbildungsmaßnahmen füllen, die für die künftige Arbeit genutzt werden können. Eine interessante Folge davon ist, dass Entscheidungen und das Erlernen neuer Dinge auf die notwendige Zeit verschoben werden. Ich für meinen Teil verstehe diesen Ansatz und finde ihn sehr sinnvoll. Wenn ich Angestellte hätte, würde ich auch ruhige Leute, die z.B. 15 Jahre im Unternehmen bleiben, den Leuten vorziehen, die einen guten Überblick haben und weiterziehen wollen, aber das geht auf Kosten des Teams. Es stimmt, dass die kollektive Lösung nie die gleiche sein wird wie die individuelle, aber das bedeutet nicht, dass sie schlechter ist.

Menschen wie ich sind eine potenzielle Quelle für Probleme und Konflikte. Es ist cool, das so zu sehen.

Suchen Sie bei der Codeüberprüfung einfach nach objektiven Fehlern
----------------------------------------

Jedes Unternehmen hat seine eigenen Regeln für den Kodex. Leider. Das hat mich anfangs sehr geärgert.

Wenn Sie eine der ausgereifteren Sprachen wie .NET verwenden, sind die Codestyle-Regeln in der Regel ähnlicher. Erst dann stellt man fest, dass es immer noch PHP und JavaScript in der Welt gibt. Vor allem die Sache mit PHP ist manchmal etwas frustrierend. PHP ist eine sehr ausgereifte Sprache mit vielen großartigen Funktionen, aber meiner Erfahrung nach wird sie in Unternehmen nur zu etwa einem Drittel ihres gesamten Potenzials genutzt. Die Gründe dafür sind unterschiedlich, meistens ist es Trägheit.

In dieser Hinsicht habe ich lange nach einer prozeduralen Lösung für die Codeüberprüfung gesucht. Ich spiele schon seit langem mit PhpStan und verfolge die Ideen rund um das Programm. Die Grundidee hinter PhpStan ist, dass es nur objektive Fehler hervorhebt, die mit Sicherheit irgendwann auftreten werden. Je mehr ich PhpStan erforsche und auf die nächste Stufe bringe, desto mehr bestätige ich innerlich, wie wahr das ist.

Es wäre schön, wenn PhpStan zumindest in der Hälfte der tschechischen Unternehmen implementiert wäre. Noch besser wäre es, wenn es mindestens Stufe 6 wäre. In der Praxis habe ich gesehen, dass die besten Unternehmen Stufe 4 oder 5 haben.

Erst neulich habe ich mich gefragt, ob es eine wirklich laufende Produktionsanwendung geben kann, die ein eigenes Entwicklungsteam hat und nicht einmal die Stufe 0 besteht - das ist die Stufe, die die Dinge kontrolliert, die man in jeder Anwendung erwarten würde. Es muss sichergestellt werden, dass alle Klassen und Funktionen vorhanden sind, dass die Dateien keine Parse-Fehler aufweisen und so weiter. Und ja, es gibt solche Anwendungen. Langfristig gesehen verbessert sich die Situation jedoch. Hoffentlich.

Daher verfolge ich bei Codeview einen ähnlichen Ansatz. Ich melde nur Dinge, die objektiv immer falsch sind. Ich stoße oft auf Fehleinschätzungen des Grades - irgendetwas stimmt nicht, aber auch hier ist es kein so großes Problem, dass es angegangen werden muss. Viele Probleme werden durch Konventionen gelöst oder dadurch, dass man erklärt, das Risiko sei gering, so dass es keinen Sinn mache, es zu behandeln.

Ich habe fehlende Datentypen (auch zusammengesetzte) immer als kritischen Fehler betrachtet. Dann habe ich verstanden, dass es Test und Dump gibt.

Ich glaube nicht, dass ich eine konkrete Schlussfolgerung für diesen Teil habe. Ich habe nur eine Menge subjektiver Kommentare dazu, die eher zu gegenseitigen Missverständnissen führen können, deshalb werde ich sie nicht veröffentlichen. Langfristig neige ich zu dem Schluss, dass ich keine Codereview durchführen kann - entweder bin ich zu streng oder zu wohlwollend. Ich kann auf einer allgemeinen Ebene nicht sagen, was wirklich wichtig ist und was weniger wichtig. Es würde mich sehr interessieren, wie andere Leute das machen. Bisher konnte mir niemand eine Antwort geben, die ich auch verwenden kann.

Mit kundenspezifischer Entwicklung ist kein Geld zu verdienen.
---------------------------------

Haben Sie eine Agentur und machen Sie kundenspezifische Webentwicklung? Wenn Sie nicht groß genug sind und große Projekte für andere Unternehmen durchführen, wird es Sie wahrscheinlich eines Tages nicht mehr geben.

Der Grund dafür ist die unzureichende Finanzierung von kundenspezifischen Projekten. Das hat wahrscheinlich mit der Art der Verträge zu tun, die für die Käufer profitabler sind. In der Tat sind die meisten Agenturen brutal unterfinanziert und machen nur für die Eigentümer einen echten Gewinn.

Wenn Sie ein Projekt intern als Unternehmen für sich selbst entwickeln, spielt eine Lieferverzögerung von einem Monat wahrscheinlich keine so große Rolle. Wenn man als Agentur einen Auftrag nicht innerhalb eines Monats abliefert, schläft man garantiert in diesem Monat bei der Arbeit ein, ruiniert ständig seine Gesundheit, der Kunde flucht ins Telefon und in die Tickets, fragt immer, ob es nicht schneller gehen könnte, und zur Belohnung zahlt man noch eine Vertragsstrafe. Und wenn Sie das nicht tun, wird der Kunde Sie als unzuverlässigen Auftragnehmer abstempeln und sich vielleicht überlegen, woanders hinzugehen, wo es ohnehin nicht besser ist.

Aber das sind die Spielregeln, die ich in der Praxis kennengelernt habe, und die ein Loch in mein Budget gerissen haben.

Die Auftragsvergabe ist wahrscheinlich die komplexeste Form des Geschäfts im IT-Bereich. Sie wollen keine Verträge abschließen. Ein Vertrag wird Sie zerstören. Sie rufen Sie am Wochenende, nachts und an Weihnachten an. Für die Hälfte der Arbeit, die Sie leisten, erhalten Sie keinen Lohn. Sie werden sich für Fehler entschuldigen, die Sie nicht machen können. Sie werden sich auf Instagram die Geschichten von Freunden ansehen, die dieses Jahr schon zum dritten Mal in den Urlaub gefahren sind, während Sie samstags um 3 Uhr morgens in Ihrem Büro sitzen und ein Kundenprojekt fertigstellen, für das Sie am Montag sowieso gescholten werden, weil der Vertrag mehr vorsieht, als Sie erledigen konnten. Die Leute nehmen Urlaub und sind krank, und du arbeitest an ihrer Stelle. Oder Sie werden gekündigt. Und dann sind Sie froh, wenn Sie die Miete zahlen können.

Aber man lernt ja auch viel. Denn wenn man unter Druck steht, ist man gezwungen, zu lernen und effizient zu sein, damit man beim nächsten Mal in der gleichen Zeit etwas mehr erledigen kann. Das Schlimme daran ist, dass man dieses Wissen und diese Erfahrung nicht wirklich anderweitig einsetzen kann, weil die Unternehmen einfach nicht daran interessiert sind (wie ich oben erklärt habe).

Glücklicherweise ist dies eine Geschichte aus dem Jahr 2019, und sie ist noch weit entfernt.

Es wird nie eine ideale Welt geben
-------------------------

Wenn ich das tue, was mir Spaß macht? Und Sie? :D

Ich denke, es ist alles eine Frage der Proportion. Sie werden wahrscheinlich nie eine Arbeit machen, die Sie zu 100 % erfüllt. Wahrscheinlich wird Ihnen immer jemand erklären, dass Sie das nicht können, was Sie seit 10 Jahren mühsam gelernt haben, obwohl Sie innerlich wissen, dass Sie es können.

Andererseits gewinnt man durch eine gewisse Verleugnung der eigenen Persönlichkeit den Raum, etwas mehr zu haben. Zum Beispiel Zeit am Wochenende, bessere Gesundheit, mehr Zeit für sich selbst, ein stabiles Umfeld usw. Dass das auf Dauer nicht durchzuhalten ist, liegt ebenfalls auf der Hand.

Ich mag es, verschiedene Dinge auszuprobieren und aus erster Hand zu erfahren, wie es anderen geht. Die Arbeit in einem Entwicklungsteam ist im Vergleich zu einer kundenspezifischen Arbeit sehr langweilig.

Es geht nicht mehr darum, die beste und schnellste Lösung zu finden, sondern um Zusammenarbeit. Es geht um die Verbesserung der Kommunikation, der Gefühle und vor allem darum, zu lernen, menschlich zu sein. Und auch das ist ein großer Wert.
