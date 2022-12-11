Eines Tages werden Hacker Ihre Website angreifen
================================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	de: eines-tages-werden-hacker-ihre-website-angreifen
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '0f0ac4c091115e9c1f0833d318f34f5d'

Das wird Ihnen nicht passieren? :DDDDDDD

Bei der Verwaltung von mehr als 300 Websites erlebe ich von Zeit zu Zeit verschiedene Notfälle. Manchmal sind sie ziemlich hitzig, aber oft ist es eine völlige Banalität. Wenn Sie, wie ich, schon einmal in Versuchung geraten sind, zu programmieren, und wenn Sie auch wissen, dass [Programmierung eine Qual ist] (http://borisovo.cz/programming-sucks-cz.html), werden Sie mir sicher zustimmen.

In der Gewalt von bösen Hackern
----------------------

Wenn eine Webanwendung populär wird, wird sie zu einem verlockenden Ziel für Hacker. Ihre Motivation ist in der Regel nicht, die gesamte Website zum Absturz zu bringen, ganz im Gegenteil. Die **Hacker wollen nämlich, dass Sie als Serveradministrator nichts von ihnen wissen**. Ein guter Hacker wartet monatelang, beobachtet den Webserver und holt sich das Wertvollste - Ihre Daten.

Um Ihre Benutzer zu schützen, müssen Sie alle Daten verschlüsseln und mehrere Schutzschichten einrichten. Verwenden Sie für Passwörter eine der langsamen Funktionen wie [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 oder Scrypt. Michal Spacek hat bereits über [Sicherheitseinstufung] geschrieben (https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), und ich danke ihm für die Ergänzung des Artikels. Als Website-Besitzer müssen Sie bei Gesprächen mit einem Programmierer auf eine ordnungsgemäße Passwortverschlüsselung bestehen. Andernfalls werden Sie weinen, so wie viele Menschen vor Ihnen, die sich ebenfalls vorgemacht haben, dass es sie nicht betrifft.

Können Sie erkennen, wann Sie angegriffen werden?
---------------------------------

Ich habe festgestellt, dass ein Webserver, sobald er einen bestimmten Schwellenwert an Datenverkehr erreicht (in der Tschechischen Republik sind das etwa 50+ Besucher pro Tag), aus dem Nichts heraus eine Reihe von Angriffen auf ihn startet. Um es nicht einfach zu machen, hat der Angreifer immer einen Vorteil, denn er kann sich aussuchen, welchen Teil der Infrastruktur er angreift, und Sie - als Eigentümer des Webservers - müssen dies rechtzeitig erkennen, sich verteidigen und beim nächsten Mal besser darauf vorbereitet sein.

Wie viele Angriffe wurden zum Beispiel im letzten Monat gegen Sie gerichtet? Wissen Sie das genau? Wie viele davon haben Sie verteidigt? Hat noch jemand Zugang zu Ihrem Server?

Was ist, wenn jemand Sie gerade jetzt angreift? Und jetzt... und jetzt auch...?

Leider gibt es kein Patentrezept, um einen Angriff zu erkennen. Aber es gibt Instrumente, die Ihnen einen erheblichen Vorteil verschaffen können.

Was in letzter Zeit für mich am besten funktioniert hat:

- **Wenn ein Angreifer nicht weiß, wo sich Ihre Server physisch befinden**, hat er eine viel schwierigere Position. Informationen über die physische Architektur können sehr gut mit [Cloudflare](https://www.cloudflare.com/) verschleiert werden, das die gesamte Netzwerkkommunikation vertritt (und bidirektional verschlüsselt). Darüber hinaus bietet es eine Reihe weiterer Vorteile, wie z. B. schnellere Abrufe aus dem Ausland, automatischen Schutz vor DDOS-Angriffen, Asset-Komprimierung und nicht zuletzt die automatische Blockierung von böswilligen Besuchern. Ich verwende Cloudflare für alle meine Websites (und fast jedes Projekt verwendet andere Technologien).
- **Fehlerprotokollierung**. Immer und überall. Wahrscheinlich enthält Ihre Anwendung eine Menge Fehler, die von Ihrem Programmierer begangen wurden. Seien es [ungefangene Ausnahmen] (https://php.baraja.cz/vyjimky), Anwendungsfehler, SQL-Injektion und so weiter. Wenn Sie in PHP programmieren und die Protokollierung nicht verstehen, gibt es genug Probleme, die [Tracy](https://tracy.nette.org/) (und möglicherweise andere Tools wie [Sentry](https://sentry.io/)) und die Zugriffsprotokolle auf dem Server selbst auffangen. Fehlerprotokollierung ist kein "nice to have", sondern ein Muss. Ich protokolliere Fehler auf allen Websites, die ich aktiv betreue, und lasse mir Informationen über neue Fehler per E-Mail zuschicken, damit ich sofort Bescheid weiß.
- **Sichere und aktualisierte Plattform**. Haben Sie WordPress auf Ihrer Website? Wissen Sie, wie man sie richtig aktualisiert? Kennen Sie alle Risiken, denen Sie ausgesetzt sind? Wenn etwas kostenlos ist, geht das fast immer auf Kosten von etwas anderem. Ich persönlich verwende [Nette framework] (https://nette.org/cs/) Version 3 für die Anwendungsentwicklung, die ich wöchentlich aktualisiere und aktiv nach Sicherheitslücken suche, die ich ausbessern kann. Genauso wie Sie Ihr Auto regelmäßig warten lassen, sollten Sie auch Ihre Website regelmäßig, mindestens aber einmal im Monat, warten lassen. Die Wartung der Website umfasst die Aktualisierung aller externen Bibliotheken und die ständige Überwachung der Protokolle. Wenn Sie eine alte Version einer Bibliothek verwenden, ist es sehr wahrscheinlich, dass ein Angreifer von der Sicherheitslücke weiß und versuchen wird, sie auszunutzen.
Die Frage ist, wann
Viele Menschen in meinem Umfeld unterschätzen die Sicherheit in unglaublicher Weise und stellen Inhalte ins Internet, die sehr leicht zu knacken und auszunutzen sind.

In der Tat machen selbst große Unternehmen Fehler, selbst bei so trivialen Dingen wie dem [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (Cross-Site-Scripting)-Angriff.

Die Frage ist nicht, ob jemand Ihre Website jemals zerstören wird. Die Frage ist nur, wann es soweit ist und ob Sie darauf vorbereitet sind. Vielleicht hat ein Hacker schon seit Monaten Zugang zu Ihrem Server und Sie wissen es (noch) nicht.

Was ist zu tun, wenn Probleme auftreten?
-------------------------------

Wenn Sie von der Arbeit des Hackers erfahren, ist es in der Regel schon zu spät, und der Hacker hat alles getan, was er tun musste. Er hat höchstwahrscheinlich Malware auf dem gesamten Server platziert und hat zumindest teilweise Kontrolle über den Rechner.

Es handelt sich um ein **chaotisches Problem, bei dem Sie schnell handeln müssen**.

Da dies die letzte Phase des Angriffs ist, empfehle ich persönlich, den Webserver vollständig vom Internet zu trennen und allmählich herauszufinden, was alles passiert ist. Außerdem werden wir nie genau wissen, ob der Server etwas anderes verbirgt. Der Angreifer hat immer einen großen Vorsprung.

Wenn wir uns entschließen, das letzte funktionierende Backup wieder auf den Server zu legen, helfen wir dem Angreifer sehr. Er weiß bereits, wie er in Ihren Server einbrechen kann, und nichts hält ihn davon ab, es in ein paar Stunden wieder zu tun. Außerdem enthält die Sicherungskopie wahrscheinlich Malware oder eine andere Art von Virus.

Obwohl dies bei meinen Kunden sehr unbeliebt ist, empfehle ich persönlich immer, zuerst **komplette WordPress-Websites** zu löschen, oder alle Systeme, die der Kunde nicht aktualisiert und die viele Sicherheitsbedrohungen aufweisen. Wenn Sie beispielsweise 30 Websites auf einem Server hosten, die mehr als 2 Jahre alt sind, ist es praktisch garantiert, dass mindestens eine davon eine Schwachstelle aufweist.

Wenn Sie das Problem nicht verstehen, sollten Sie sich frühzeitig an einen geeigneten Spezialisten wenden, der sich mit Ihrem Problem auskennt und die Dinge in einem größeren Zusammenhang sieht.

**Ethische Anmerkung:**

Wenn Sie eine Website für einen Kunden verwalten, bei dem ein Sicherheitsvorfall aufgetreten ist, sind Sie dafür verantwortlich, ihn zu informieren. Wenn Daten (z. B. Passwörter, aber oft noch viel mehr) nach außen dringen, müssen Sie auch die Endnutzer darüber informieren, dass ihr Passwort öffentlich bekannt ist und sie es überall ändern sollten. Wenn Sie eine langsame Hash-Funktion verwenden (z. B. **Bcrypt + Salt**), erschweren Sie es einem Angreifer erheblich, Passwörter zu knacken (leider können [Bcrypt-Passwörter geknackt werden](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Wenn Sie regelmäßig Informationen darüber erhalten möchten, ob Ihr Konto geleakt wurde, empfehle ich Ihnen, sich bei [Have i been pwned?](https://haveibeenpwned.com/) zu registrieren. Weitere Informationen über [Sicherheitsverletzung] (https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni) finden Sie auf der OOOO-Website.

Atomare Gewohnheiten
--------------

In den letzten sechs Monaten habe ich das Buch [Atomic Habits] (https://www.melvil.cz/kniha-atomove-navyky/) gelesen, das mir die Augen geöffnet hat. Er beschreibt einen Prozess der schrittweisen Verbesserung um 1 % jeden Tag, der auf lange Sicht viel bewirken wird.

Da ich von Unternehmen und Einzelpersonen angesprochen werde, die sich in verschiedenen Stadien der Technologieverschuldung befinden, möchte ich die schlimmsten Dinge zusammenfassen:

- **FTP für die Serverkommunikation ist nicht sinnvoll**. Es handelt sich um ein unsicheres Protokoll, das passwortgeschützt ist. Wenn Sie jemandem Zugang gewähren wollen, muss dieser das Passwort kennen und kann dasselbe tun wie Sie. Wenn Sie ihm den Zugang entziehen wollen, können Sie das nur, indem Sie das Passwort ändern. Besser ist es, vollständig zu SSH zu wechseln und RSA-Schlüssel zu verwenden. Ich bin oft überrascht, dass selbst Unternehmen dieses Problem heutzutage lösen. Erstellen Sie niemals eine Tabelle mit allen Kennwörtern. Und schon gar nicht eine gemeinsame für das ganze Team!
- Der **Quellcode gehört in Git**, die Daten in die Datenbank und der statische Inhalt (Bilder, PDFs, ...) in Dateien auf der Festplatte. Die Wahl des falschen Datenspeichers verschlechtert das Anwendungsdesign und die Sicherheit. Die URL zu einer PDF-Datei (z. B. mit einer Rechnung) sollte zufällig generierte Inhalte enthalten, die nur der Empfänger kennt. Bei äußerst sensiblen Inhalten (z. B. Kontoauszügen) ist es wichtig, den Inhalt auf der Festplatte zu verschlüsseln und erst nach der Anmeldung bereitzustellen.
- Nicht öffentliche Teile der Website müssen die META-Kopfzeilen `noindex` und `nofollow` enthalten, damit sie nicht von Google indexiert werden. Sensible Inhalte, die Ihre Mitbewerber nicht kennen dürfen, müssen durch ein Passwort geschützt werden.
- Deaktivieren Sie [directory content listing](https://www.simplified.guide/apache/disable-directory-listing) auf Ihrem Server. Bei falscher Einstellung kann die gesamte Website maschinell gecrawlt werden, um sensible Dateien zu finden.
- Projekt Wurzel! Es darf niemals eine direkte URL zu Konfigurationsdateien geben, wie z.B. bei [Nette macht es richtig] (https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) gleich im ersten Tutorial. Dasselbe gilt für [öffentlich zugängliche Git-Repositories] (https://smitka.me/open-git/). Diese Art der Verwundbarkeit wird sehr leicht unterschätzt, und die Folgen sind meist tragisch.
- Der Fehler kann auch durch einen versehentlichen Entwicklungsfehler entstehen. Es ist sehr wichtig, bei jeder Änderung eine Codeüberprüfung durch einen Menschen vorzunehmen. Viele Bugs werden von [PHPStan](https://github.com/phpstan/phpstan) oder ähnlichen Tools entdeckt, bevor ein Mensch den Code sieht.
- Senden Sie niemals [Passwörter in lesbarer Form per E-Mail] (https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), es gibt immer einen anderen Weg, das Problem zu lösen.
- Sicherheitsprobleme in alten Versionen von Bibliotheken und Software. Roboter durchforsten jede Sekunde das Internet und greifen bekannte Sicherheitslücken an (SQL-Injection, XSS, CSRF, ...). Viele bekannte Löcher haben ältere Versionen von WordPress.

Es gibt noch viele weitere Schwachstellen und die Probleme sind von Projekt zu Projekt unterschiedlich. Wenn Sie einen schnellen Server-Audit durchführen müssen, empfehle ich Ihnen, [Maldet] (https://www.rfxn.com/projects/linux-malware-detect/) zu verwenden und dann einen geeigneten Spezialisten zu beauftragen, Ihnen bei einem [Full Site Audit] (https://baraja.cz/audit-webu) zu helfen, und zwar nicht nur aus Sicherheitsgründen.

Sicherheitsingenieure sind wie Plastik im Ozean - einfach für immer. Gewöhnen Sie sich daran.

Das Prinzip von Aktion und Reaktion in der Natur
-------------------------------

Sie werden auch festgestellt haben, dass die Natur fast immer das Reaktionsprinzip anwendet. Das bedeutet, dass etwas in einer bestimmten Umgebung passiert und die Organismen in der Umgebung unterschiedlich darauf reagieren. Dieser Ansatz hat viele Vorteile, aber einen großen Nachteil: Sie werden immer zurückbleiben.

Als denkender Mensch (Designer, Entwickler, Berater) haben Sie einen großen Vorteil, nämlich handlungsfähig zu sein - das heißt, einen gewissen Teil der großen Risiken im Voraus zu kennen und aktiv zu verhindern, dass sie überhaupt eintreten. Man kann vielleicht nie alle Fehler verhindern, aber man kann zumindest die Folgen abmildern oder Kontrollmechanismen einrichten, die Probleme frühzeitig erkennen, so dass man Zeit hat zu reagieren.

Die meisten Angriffe erfolgen über einen längeren Zeitraum, und wenn Sie ein Logbuch führen, haben Sie in der Regel genug Zeit, um das Problem zu erkennen.
