Warum und wie man Frameworks und Bibliotheken verwendet
=======================================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	de: warum-und-wie-man-frameworks-und-bibliotheken-verwendet
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

Es gibt einen berühmten Witz, der besagt, dass Programmierer erst dann anfangen, Frameworks zu verwenden, wenn sie ihre eigenen schreiben und feststellen, dass das nicht weiterhilft. Das Lustige daran ist, dass es wahr ist. Ich habe es selbst erlebt. Zweimal sogar.

Auf der Hauptseite von <a href="https://nette.org">Nette</a> heißt es jedoch:

> Echte Programmierer **benutzen keine Frameworks**. Sie schreiben Webanwendungen über die Befehlszeile direkt auf den Server. Dies ist eine Hommage an sie. Für den Rest von uns wird Nette unsere Arbeit sehr viel einfacher und angenehmer machen.

Die Rolle von Frameworks bei der Anwendungsentwicklung
-----------------------------------

Ich bin sicher, Sie wissen das selbst:

Sie haben eine Idee für ein großartiges Projekt und fangen direkt an, Code in reinem PHP zu schreiben... nach ein paar Stunden stellen Sie fest, dass sich ein Großteil der Arbeit immer noch wiederholt und Sie grundlegende Systemprobleme lösen müssen. Zum Beispiel, wie man eine Verbindung zu einer Datenbank herstellt, wie man ein Formular rendert, wie man Daten validiert, wie man eine E-Mail versendet und vieles mehr.

Sie werden wahrscheinlich Ihre eigenen Funktionen schreiben, die Sie für diese Aufgaben aufrufen. Wenn Sie an mehreren Projekten gleichzeitig arbeiten, werden Sie diese Funktionen in einer großen, unübersichtlichen Datei zwischen den Projekten austauschen und sie bei Bedarf ständig anpassen.

Und wenn die Funktionen so weit entwickelt sind, dass man jedes Mal ein neues Projekt darauf aufbaut und sofort entwickelt, dann kann man davon sprechen, dass man sein eigenes Framework oder zumindest eine Bibliothek geschrieben hat.

Was ist und was bewirkt ein Rahmen
-------------------------

Wie wir bereits an einem praktischen Beispiel aus dem Leben gezeigt haben, ist ein Framework eine Sammlung vieler Funktionen (idealerweise aber Klassen und Objekte), die dem Programmierer die Arbeit abnehmen. Bei der Entwicklung muss er nicht darüber nachdenken, wie er eine Verbindung zu einer Datenbank herstellen kann, sondern er weiß, dass dies bereits irgendwo programmiert ist und "es einfach funktioniert".

Fertige Frameworks sind also die gesammelte Intelligenz von Hunderten von Menschen, die über einen langen Zeitraum Tausende von Anwendungen entwickelt haben, um eine Lösung und ein Ökosystem zu debuggen und neue Projekte in Angriff zu nehmen.

Mit Frameworks erhalten Sie auch eine Reihe von **besten Praktiken**, d. h. Möglichkeiten zur Lösung von Problemen, ohne darüber nachdenken zu müssen. Wenn Sie auf ein Problem stoßen, hat es wahrscheinlich schon jemand vor Ihnen gelöst, und es ist immer besser, eine Lösung in der Dokumentation zu finden, als es selbst auf komplizierte Weise herausfinden zu müssen.

Abstrakte Programmierung und das Kapselungsprinzip
---------------------------------------------

Gut konzipierte Frameworks wie Nette ermöglichen es Ihnen, auf einer **sehr hohen Abstraktionsebene** zu programmieren.

Auf diese Weise muss der Programmierer (der Nutzer des Frameworks) nicht verstehen, was intern vor sich geht und wie genau die Komponenten intern funktionieren, was eine Menge Zeit und Mühe spart. Er kann sich auf die Lösung der tatsächlichen Probleme seiner Anwendung konzentrieren und sehr schnell Ergebnisse erzielen.

David Grudl selbst (der Autor des Nette-Frameworks) hat einmal gesagt, dass er Nette entwickelt hat, um eine Website für einen Kunden in einer Nacht programmieren zu können, wenn er spät aus der Kneipe nach Hause kommt und die Website am Morgen starten muss. Das liegt vor allem daran, dass sich der Entwickler nicht mit der Technik befasst.

Damit dies funktioniert und der Entwickler einfach das fertige Framework als Ganzes nutzen kann, ist es sehr wichtig, das <a href="/encapsulation">Einkapselungsprinzip</a> korrekt anzuwenden.
