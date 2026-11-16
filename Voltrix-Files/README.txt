VOLTRIX — File Manager (nuovo meccanismo)

Cosa è cambiato
- Prima: password "fileadmin" scritta nella pagina (visibile a chiunque con "Ispeziona") e controllo solo nel browser.
- Ora: login con Firebase Authentication (email + password). Le email e la password restano le stesse, ma la password non è più nel codice.
  L'accesso è deciso dal server tramite le regole del database (firebase-rules.json), non dalla pagina.
- Owner: può bannare/riattivare i dev (anche se collegati: vengono buttati fuori subito) e impostare l'immagine del vault.
- Dev: entra, usa i file, può aprire il vault (richiede di reinserire la password).
- Vault: si blocca da solo dopo 90 s, se cambi scheda, con Esc (prima era Ctrl+C, che interferiva con la copia).
- I file restano SOLO nel browser (IndexedDB), non vengono caricati su Firebase, e sono CIFRATI (AES-256-GCM).
  La chiave nasce dalla tua password (PBKDF2, 310.000 giri) e vive solo in memoria: non viene mai salvata.
  Nomi, tipo e dimensione dei file sono cifrati insieme al contenuto. Dopo un ricaricamento della pagina i file
  restano bloccati finché non reinserisci la password. Se cambi la password, i file già salvati non si aprono più.

Configurazione (una volta, dalla console Firebase del progetto ez1234-62da2)
1. Authentication > Sign-in method: abilita "Email/Password"; disabilita "Anonymous" se attivo.
   Authentication > Settings > User actions: disattiva "Enable create (sign-up)" se disponibile.
2. Authentication > Users > Add user: crea le due email con la STESSA password di prima.
   Copia l'UID di ciascun utente.
3. Realtime Database > Dati: importa admins-template.json (sostituisci UID ed email) -> nodi /admins/<UID> con
   email, role ("owner" o "dev"), active true.
4. Realtime Database > Regole: incolla firebase-rules.json e pubblica.
5. Apri index.html (meglio da un hosting, es. Firebase Hosting; Authentication > Settings > Authorized domains
   deve contenere il dominio).
6. Entra come owner e imposta l'URL https dell'immagine del vault.

Note
- La vecchia password era visibile nel sorgente della pagina: consigliato cambiarla dalla console Firebase.
- Non testato contro il tuo progetto Firebase reale; testato con un Firebase simulato.
