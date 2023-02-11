Come gestire gli improvvisi crash degli script PHP
==================================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	it: come-gestire-gli-improvvisi-crash-degli-script-php
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

Una storia di fine 2016, quando sono stato letteralmente salvato da un collega: in un'applicazione PHP, si decide di effettuare il check-in delle immagini tramite uno script proxy, che, tra le altre cose, può regolare le loro dimensioni e altri parametri in base alla richiesta in arrivo. Come parte dell'ottimizzazione, si salvano anche le varianti generate fisicamente su disco.

Tuttavia, in fase di produzione, si inizia improvvisamente a vedere un carico enorme e migliaia di richieste in coda. Le immagini vengono caricate in sequenza una per ogni utente. L'aggiornamento della pagina e i clic sui link non funzionano. L'applicazione sembra completamente bloccata. Funziona solo se si attende che tutto venga elaborato.

Quale potrebbe essere il problema? Ho elencato 3 indizi principali nel testo per consentire una rapida ricerca del problema. Hotfix ha una soluzione banale.
