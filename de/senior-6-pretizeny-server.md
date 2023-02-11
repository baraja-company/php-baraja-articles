Dringende Reparatur eines überlasteten Servers
==============================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	de: dringende-reparatur-eines-ueberlasteten-servers
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

Ein externes Überwachungstool meldet Ihnen, dass sich die durchschnittliche Antwortzeit der 5 überwachten URLs in den letzten 30 Minuten verdoppelt hat. Das Projekt läuft auf einem einzelnen physischen Server, der nicht von Ihnen verwaltet wird und sich irgendwo in einem Rechenzentrum befindet. Sie verbinden sich über SSH, starten htop und sehen, dass die CPU-Last 95% beträgt und der Speicher längst überläuft.

Laut git wissen Sie, dass sie vor etwa einer Woche eine Datenbankmigration auf eine neue Tabellenstruktur durchgeführt haben, und ein Kollege schreibt im Chat, dass er die Migration über Nacht durchführen musste, weil die Neuberechnung der Spalten und Indizes etwa 5 Stunden dauerte, während derer fast die gesamte Datenbank gesperrt war und weder INSERT noch SELECT funktionierten.

Die Leistungsprobleme sind also wahrscheinlich auf unsachgemäß gestaltete Indizes, schlecht umgestaltete SQL-Abfragen oder ein großes Verbindungspooling zurückzuführen. Laut Google Analytics gibt es 7.000 Nutzer auf der Website, und ein Ausfall von 5 Stunden würde für den Kunden ein Reputationsrisiko und einen Verlust von Zehn- bis Hunderttausenden von Kronen in dieser Zeit bedeuten (es ist schwer zu schätzen, die Projektionisten machen genug aus). Sie erkennen, dass es nicht ausreicht, nur die Funktionalität in einer Testumgebung zu testen, sondern dass Sie auch einen Lasttest durchführen müssen.

Da es sich um einen wichtigen E-Commerce-Shop Ihres größten Kunden handelt und Sie davon ausgehen, dass sich die Situation verschlimmern könnte, haben Sie 30 Sekunden Zeit, eine Entscheidung zu treffen.

Wie gehen Sie vor?

1. Sie tun nichts. Die Website wird langsamer sein, aber solange der Server damit umgehen kann, ist das wohl okay...
2. Sie werden versuchen, einen Plan zur Umkehrung der DB-Struktur zu erstellen und ihn so bald wie möglich ausführen.
3. Sie migrieren die Website auf einen leistungsfähigeren Server (was allerdings bedeutet, dass Sie den Preis für den Kunden rasch erhöhen und dann vielleicht 6 Stunden warten müssen, bis die Migration abgeschlossen ist, da Sie Hunderte von GB an Datenbanktabellen haben).
4. Sie finden heraus, welche SQL-Abfragen und welche Seiten die meiste Zeit in Anspruch nehmen, und deaktivieren diese einfach.
5. Sie starten Cloudflare und lassen es statisch einchecken, was es kann.
6. Ein paar andere Zaubereien (ich glaube, da fällt mir nicht viel ein)...
