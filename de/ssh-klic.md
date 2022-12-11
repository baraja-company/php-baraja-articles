Kommunikation über SSH und RSA2-Schlüssel
=========================================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	de: kommunikation-ueber-ssh-und-rsa2-schluessel
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH ist ein Netzwerkprotokoll für verschlüsselte Datei- und Terminalübertragungen. SSH wird am häufigsten für die Fernsteuerung eines Webservers und die sichere Dateiübertragung verwendet. Im Gegensatz zu FTP bietet es eine verschlüsselte Verbindung. SSH kommuniziert über den Standard-Port `22`. Die Verbindung wird mit dem Benutzernamen und der Adresse des Zielservers initialisiert. Zur Authentifizierung kann ein Passwort (nicht empfohlen) oder ein SSH RSA2-Schlüssel (empfohlen) verwendet werden.

Erlangung (Erzeugung) des Schlüssels
--------------------------

Bevor wir uns mit dem Server verbinden, müssen wir unseren ersten SSH RSA2-Schlüssel erhalten (oder erzeugen). Es ist wichtig, dass es sich um einen "RSA2"-Algorithmus handelt. Das liegt daran, dass es eine Reihe von Schlüsseln gibt, von denen nicht alle verwendet werden können.

Unter Linux wird das Dienstprogramm `ssh-keygen` zur Erzeugung des Schlüssels verwendet, dem wir die Komplexität des Schlüssels (in diesem Fall 4096) und die E-Mail-Adresse des autorisierten Benutzers mitteilen:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Nach dem Ausführen des Befehls werden wir nach dem Pfad zum Speichern des Schlüssels und einem eventuellen `Passwort` (Autorisierungspasswort) gefragt. Geben Sie als Pfad nichts ein (der Standardspeicherort wird automatisch ausgewählt) und geben Sie optional die Passphrase ein (wenn Sie dies tun, müssen Sie jedes Mal dasselbe Passwort eingeben, bevor Sie den Schlüssel verwenden).

Der erzeugte Schlüssel wird automatisch im Standardverzeichnis `~/.ssh` gespeichert, d.h. im Verzeichnis `.ssh` im Heimatverzeichnis des aktuellen Benutzers.

Erzeugen eines SSH-Schlüssels unter Windows
-------------------------------

Leider gibt es in Windows keinen Standardpfad für den SSH-Schlüssel. Es ist daher ideal, z.B. das Dienstprogramm `Putty` und `PuttyGen` zu installieren, um den Schlüssel zu erzeugen. Wählen Sie immer den "RSA2"-Algorithmus. Auch hier wird ein Schlüsselpaar erzeugt, das Sie irgendwo sicher aufbewahren sollten. Bevor Sie den SSH-Schlüssel in `Putty` verwenden, müssen Sie den Festplattenpfad auswählen, von dem der Schlüssel abgerufen werden soll. Unter Linux ist dies nicht erforderlich, es gibt einen Standardpfad für die Festplatte.

Sicherheit der Schlüssel
---------------

Bei der Erzeugung von Schlüsseln werden tatsächlich zwei erzeugt. Ein öffentlicher Schlüssel (den Sie der anderen Partei geben, um die Kommunikation zu ermöglichen) und ein privater Schlüssel (der nur Ihnen gehört, den Sie nie jemandem mitteilen und der zur Entschlüsselung der Kommunikation verwendet wird).

Es ist wichtig, dass Sie den privaten Schlüssel niemals verlieren. Sie zu verlieren bedeutet, die gesamte Kommunikation zu unterbrechen!

Im Allgemeinen wird empfohlen, für jedes Gerät und jeden Benutzer ein eindeutiges öffentliches/privates Schlüsselpaar zu erzeugen, um die Wahrscheinlichkeit von Lecks zu verringern. Wenn Sie den Schlüssel jedoch von einem Gerät zum anderen übertragen möchten, können Sie dies tun. Der SSH-Schlüssel befindet sich auf der Passwortebene, so dass die Verbindung sofort funktioniert, wenn Sie ihn auf einen anderen Rechner übertragen.

Einige Server merken sich, mit welchem Gerät sie zuletzt über SSH kommuniziert haben. Daher ist es möglich, dass die Verbindung nach der Übertragung des Schlüssels auf den neuen Computer nicht mehr funktioniert. In diesem Fall müssen Sie den Schlüssel-Cache auf dem Server löschen.

Autorisierung des Schlüssels zur Verbindung mit dem Server
--------------------------------------

Der Befehl `ssh` wird für die Verbindung zum Server verwendet. Geben Sie einfach den Benutzer und die Domäne ein:

```shell
ssh root@baraja.cz
```

Dann versucht er, eine SSH-Verbindung herzustellen. Wenn Sie einen funktionierenden und korrekt konfigurierten SSH-Schlüssel haben, wird die Verbindung automatisch hergestellt. Wenn nicht, müssen Sie ein Passwort eingeben.

Wenn Sie sich gegenüber Ihrem Server mit einem SSH-Schlüssel statt mit einem Passwort authentifizieren wollen, müssen Sie Ihren **öffentlichen Schlüssel** auf den Server übertragen.

Zeigen Sie sie einfach mit dem Befehl an:

```shell
cat ~/.ssh/id_rsa.pub
```

Kopieren Sie den gesamten Inhalt auf den Zielserver in das Verzeichnis `~/.ssh/authorized_keys`. Wenn Sie mehr als einen Schlüssel haben, sollte jeder in einer eigenen Zeile stehen.
