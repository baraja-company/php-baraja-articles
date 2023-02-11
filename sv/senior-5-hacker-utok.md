Hackerattack mot byrån
======================

> id: '7d5edd94-cce4-4a32-9ec5-51260dcd1653'
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
> 	sv: hackerattack-mot-byran
> 
> publicationDate: '2023-02-11 14:20:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '18511f7b3ff80124720244a93e140932'

En berättelse från 2017: Du arbetar som huvudutvecklare på en byrå och hanterar cirka 300 projekt av olika storlek som företaget har utvecklat under den tiden. De flesta av dem är enkla Nette-program med upp till 10 mallar, några formulär och databastabeller. Inget märkvärdigt. Du vet inte så mycket om projekten, eftersom varje projekt har utvecklats av en lite annorlunda leverantör, folk roterar in och ut ur företaget, och du hoppas att du ska få det gjort på något sätt. Eftersom företaget arbetar mycket med kostnadsoptimering har de kanske 50 projekt på en server åt gången.

Plötsligt kommer en projektledare springande till dig och säger att en av de viktiga kundwebbplatserna har slutat fungera. Istället för den riktiga webbplatsen får du en svart skärm och en massa text som säger att webbplatsen har hackats och har tillgång till hela servern.

Återigen, du vet inte (och kan inte veta) så mycket om projektens arkitektur och innehåll, och många projekt körs bara på servern. Projektionisten säger till dig att webbplatsen måste fungera, men du vet inte hur omfattande attacken är och vart angriparen har tagit vägen, eller om det finns bakdörrar från det förflutna.

Hur bestämmer du dig?

1. Du gör ingenting och försöker först analysera händelsen.
2. Du tar servern offline. Du vet inte hur stor skadan är och angriparen kan fortsätta att tränga in i servern och stjäla data. Samtidigt innebär det att många webbplatser är otillgängliga.
3. Du gör en återställning av en specifik webbplats från en säkerhetskopia för en dag sedan, och under tiden tar du reda på vad som hände. Men angriparen kan återfå tillgång och fortsätta att stjäla data.
4. En annan lösning...
