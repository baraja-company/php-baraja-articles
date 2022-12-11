Kommunikation via SSH og RSA2-nøgle
===================================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	da: kommunikation-via-ssh-og-rsa2-nogle
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH er en netværksprotokol til krypteret fil- og terminaloverførsel. SSH bruges oftest til fjernstyring af en webserver og sikker filoverførsel. I modsætning til FTP tilbyder den en indbygget krypteret forbindelse. SSH kommunikerer via standardporten `22`. Forbindelsen initialiseres med brugernavnet og adressen på destinationsserveren. Der kan bruges en adgangskode (anbefales ikke) eller en SSH RSA2-nøgle (anbefales) til godkendelse.

Fremskaffelse (generering) af nøglen
--------------------------

Før vi opretter forbindelse til serveren, skal vi have (eller generere) vores første SSH RSA2-nøgle. Det er vigtigt, at det er en `RSA2`-algoritme. Det skyldes, at der er en række nøgler, og at ikke alle kan bruges.

I Linux bruges værktøjet `ssh-keygen` til at generere den, hvor vi angiver kompleksiteten af nøglen (4096 i dette tilfælde) og e-mailen på den autoriserede bruger:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Når du har kørt kommandoen, bliver du bedt om at angive stien til at gemme nøglen og et eventuelt `password` (autorisationsadgangskode). Indtast ikke noget som sti (standardplaceringen vælges automatisk), og indtast eventuelt adgangskoden (hvis du gør det, skal du indtaste den samme adgangskode hver gang, før du kan bruge nøglen).

Den genererede nøgle gemmes automatisk til standardplaceringen `~/.ssh`, dvs. til mappen `.ssh` i den aktuelle brugers hjemmemappe.

Generering af en SSH-nøgle i Windows
-------------------------------

Desværre har Windows ikke en standardsti til SSH-nøglen. Det er derfor ideelt at installere f.eks. hjælpeprogrammet `Putty` og `PuttyGen` for at generere nøglen. Vælg altid algoritmen `RSA2`. Der genereres igen et par nøgler, så gem dem sikkert et sted. Før du bruger SSH-nøglen i `Putty`, skal du vælge den disksti, hvorfra nøglen skal hentes. I Linux er dette ikke nødvendigt, der er en standarddisk sti.

Nøglesikkerhed
---------------

Når der genereres nøgler, genereres der faktisk to. En offentlig nøgle (den du giver den anden part for at tillade kommunikation) og en privat nøgle (den er kun din egen, du må aldrig fortælle det til nogen, og den bruges til at dekryptere kommunikation).

Det er vigtigt, at du aldrig mister den private nøgle. At miste den betyder, at hele kommunikationen går i stykker!

Det anbefales generelt at generere et unikt offentligt/privat nøglepar for hver enhed og bruger for at reducere sandsynligheden for lækage. Men hvis du vil overføre nøglen mellem enheder, kan du gøre det. SSH-nøglen er på adgangskode-niveau, så når du flytter den til en anden maskine, fungerer forbindelsen med det samme.

Nogle servere husker, hvilken enhed de sidst kommunikerede med via SSH. Det er derfor muligt, at forbindelsen ikke fungerer, når du flytter nøglen til den nye computer. I dette tilfælde skal du rydde nøglecachen på serveren.

Godkendelse af nøglen til at oprette forbindelse til serveren
--------------------------------------

Kommandoen `ssh` bruges til at oprette forbindelse til serveren. Du skal blot indtaste brugeren og domænet:

```shell
ssh root@baraja.cz
```

Derefter vil den forsøge at oprette en SSH-forbindelse. Hvis du har en fungerende og korrekt konfigureret SSH-nøgle, vil forbindelsen blive oprettet automatisk. Hvis ikke, skal du indtaste en adgangskode.

Hvis du vil autentificere dig over for din server ved hjælp af en SSH-nøgle i stedet for en adgangskode, skal du overføre din **offentlige nøgle** til serveren.

Du skal blot vise den med kommandoen:

```shell
cat ~/.ssh/id_rsa.pub
```

Og kopier hele indholdet til målserveren på adressen `~/.ssh/authorized_keys`. Hvis du har mere end én nøgle, skal hver nøgle stå på en separat linje.
