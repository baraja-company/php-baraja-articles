Hackerangreb på agenturet
=========================

> id: '7d5edd94-cce4-4a32-9ec5-51260dcd1653'
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
> 	da: hackerangreb-pa-agenturet
> 
> publicationDate: '2023-02-11 14:20:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '18511f7b3ff80124720244a93e140932'

En historie fra 2017: Du arbejder som lead developer i et bureau, og du administrerer omkring 300 projekter af forskellig størrelse, som virksomheden har udviklet i løbet af den tid. De fleste af dem er simple Nette-applikationer med op til 10 skabeloner, et par formularer og databasetabeller. Ikke noget særligt. Du ved ikke så meget om projekterne, fordi de hver især er udviklet af en lidt forskellig leverandør, folk skifter ind og ud af virksomheden, og du håber at få det gjort på en eller anden måde. Da virksomheden arbejder meget med omkostningsoptimering, er de vært for måske 50 projekter på én server ad gangen.

Pludselig kommer en projektleder løbende til dig og fortæller, at et af de vigtige kundewebsteder er holdt op med at fungere. I stedet for det rigtige websted får du en sort skærm, og der er alle mulige tekster, der fortæller, at webstedet er blevet hacket og har adgang til hele serveren.

Igen, du ved ikke (og kan ikke vide så meget om projekternes arkitektur og indhold, og mange projekter kører kun på serveren. Projektionisten presser dig til at tro, at webstedet skal fungere, men du kender ikke omfanget af angrebet, og du ved ikke, hvor angriberen er taget hen, eller om der er bagdøre fra fortiden.

Hvordan beslutter du dig?

1. Du gør ingenting og forsøger først at analysere hændelsen.
2. Du tager serveren offline. Du kender ikke omfanget af skaden, og angriberen kan fortsætte med at trænge ind på serveren og stjæle data. Samtidig betyder det, at mange websteder er utilgængelige.
3. Du udfører en gendannelse af et bestemt websted fra en sikkerhedskopi for en dag siden, og i mellemtiden tager du stilling til, hvad der skete. Men angriberen kan få adgang igen og fortsætte med at stjæle data.
4. En anden løsning...
