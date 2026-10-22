[README_Version4.md](https://github.com/user-attachments/files/33828601/README_Version4.md)

# Bacheca ITIS SGM

Bacheca digitale per gli studenti: avvisi, eventi, compiti, club e mercatino.
Un solo file, nessuna installazione.

## Pubblicare su GitHub Pages
1. Crea un repository e carica `index.html` e questo `README.md`.
2. Vai su Settings > Pages.
3. In "Branch" scegli `main` e la cartella `/ (root)`, poi Save.
4. Dopo circa un minuto il sito è online su `https://TUO-UTENTE.github.io/NOME-REPO/`.

I fogli sono salvati nel browser di chi li scrive (localStorage).

## Avvisi ufficiali (solo staff)
Gli avvisi in alto vengono letti dal file `avvisi.json`. Solo chi ha accesso in scrittura al repository può cambiarli, quindi il controllo è garantito da GitHub.

- Per dare accesso a poche persone: Settings > Collaborators > Add people.
- Per modificare: apri `avvisi.json` su GitHub, clicca la matita, cambia il testo, Commit changes. Il sito si aggiorna in circa un minuto.
- Il pulsante "Area staff" in fondo alla pagina aiuta a preparare il testo giusto da incollare. Non pubblica nulla da solo.
- Ogni avviso ha: `titolo`, `testo`, `data` (AAAA-MM-GG) e `urgente` (true o false).

## Fogli condivisi tra tutti gli studenti (Firebase, gratis)
Senza questa configurazione i fogli restano sul dispositivo di chi li scrive. Per renderli visibili a tutti:

1. Vai su console.firebase.google.com e crea un progetto (puoi disattivare Google Analytics).
2. Build > Authentication > Get started > Sign-in method: abilita Anonymous.
3. Build > Firestore Database > Create database (modalità produzione, regione europea).
4. Nella scheda Rules incolla il contenuto di `firestore.rules` e premi Publish.
5. Impostazioni progetto (icona ingranaggio) > Your apps > icona web `</>` > registra l'app e copia `apiKey`, `authDomain`, `projectId` e `appId` dentro `config.js`.
6. In Authentication > Settings > Authorized domains aggiungi `TUO-UTENTE.github.io`.
7. Carica i file aggiornati su GitHub.

Come funziona:
- Tutti leggono i fogli. Ognuno può eliminare o segnare come importante solo i propri, dallo stesso browser con cui li ha scritti.
- Per togliere un foglio inappropriato, lo staff lo elimina dalla console Firebase, nella raccolta `fogli`.
- Le chiavi in `config.js` non sono segrete: la protezione è data dalle regole in `firestore.rules`.

## Nota importante
Se `config.js` resta vuoto, la bacheca continua a funzionare in modalità locale: i fogli restano sul dispositivo di chi li scrive.
