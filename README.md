# AgentMap

App per la gestione di clienti, visite, mappe e giri settimanali per agenti.

## Pubblicare su GitHub Pages

1. Vai su https://github.com e accedi (o crea un account gratuito).
2. Clicca su **New** (in alto a sinistra) per creare un nuovo repository.
   - Nome: `agentmap` (o quello che preferisci)
   - Visibilita': **Public** (richiesto per GitHub Pages gratuito)
   - **Non** spuntare "Add a README" (ce l'abbiamo gia')
3. Carica i file di questa cartella (`agentmap.html`, `index.html`, `README.md`):
   - Clicca su **uploading an existing file** (o trascinali nella pagina del repo)
   - Scrivi un messaggio di commit (es. "primo commit") e clicca **Commit**.
4. Vai su **Settings** del repo > **Pages** (menu a sinistra).
5. Sotto "Build and deployment" > "Source" seleziona **Deploy from a branch**.
6. Sotto "Branch" seleziona **main** e cartella **/ (root)**, poi **Save**.
7. Aspetta 1-2 minuti. In alto comparira' un riquadro verde con l'URL pubblico, tipo:
   `https://TUONOME.github.io/agentmap/`
8. Apri quell'URL dal telefono e dal PC: e' lo stesso sito.

## Configurare la sincronizzazione tra dispositivi (Firebase)

Senza questo passaggio i dati restano sul singolo dispositivo. Per averli sincronizzati tra PC e telefono devi creare un progetto Firebase **gratuito**.

### 1. Crea il progetto Firebase

1. Vai su https://console.firebase.google.com/ e accedi col tuo account Google.
2. Clicca **Add project** (o **Crea progetto**).
3. Dai un nome (es. `agentmap-sergio`), clicca Continua.
4. Disabilita Google Analytics (non serve), clicca **Crea progetto**.

### 2. Abilita Firestore (il database)

1. Nel menu a sinistra clicca **Build** > **Firestore Database**.
2. Clicca **Crea database**.
3. Scegli modalita' **Production** (sicura), clicca Avanti.
4. Scegli la regione **eur3 (europe-west)** e clicca Abilita.
5. Quando il database e' pronto, vai su **Rules** (in alto) e incolla queste regole:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /agentmap/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

6. Clicca **Pubblica**.

### 3. Abilita il login con Google

1. Menu a sinistra > **Build** > **Authentication**.
2. Clicca **Get started**.
3. Sotto "Sign-in method" clicca su **Google**.
4. Spunta **Enable**, scegli un nome di progetto pubblico (es. AgentMap) e una email di supporto, poi **Save**.

### 4. Aggiungi l'app web e copia la config

1. Nella home del progetto Firebase, clicca sull'icona **</>** ("Web") per aggiungere un'app web.
2. Soprannome dell'app: `agentmap`. **Non** spuntare "Hosting". Clicca **Register app**.
3. Vedrai un blocco di codice tipo:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "agentmap-sergio.firebaseapp.com",
  projectId: "agentmap-sergio",
  storageBucket: "agentmap-sergio.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123def456"
};
```

4. **Copia** quei valori.

### 5. Incolla la config in agentmap.html

1. Apri `agentmap.html` con un editor di testo (Blocco note va bene).
2. Cerca (`Ctrl+F`) la parola `FIREBASE_CONFIG`.
3. Trovi un blocco con scritto `PASTE_YOUR_API_KEY` e simili. Sostituisci ogni valore con quelli copiati da Firebase.
4. Salva il file.
5. Ricarica il file su GitHub (sostituisci `agentmap.html` con la nuova versione).

### 6. Autorizza il dominio GitHub Pages su Firebase

1. Torna su Firebase > **Authentication** > **Settings** > **Authorized domains**.
2. Clicca **Add domain** e aggiungi: `TUONOME.github.io` (sostituisci con il tuo username GitHub).

### 7. Usa l'app

1. Apri il sito (`https://TUONOME.github.io/agentmap/`).
2. Clicca sull'icona "utente" in alto a destra.
3. Clicca **Accedi con Google** > scegli il tuo account.
4. Da quel momento tutti i dati si salvano nel cloud automaticamente.
5. Apri lo stesso sito da un altro dispositivo, accedi con lo stesso account: ritrovi tutto.

## Costi

Tutto **gratis** entro questi limiti (per uso singolo agente Sergio non li raggiungerai mai):

- GitHub Pages: 100 GB/mese di traffico, illimitato per uso personale.
- Firebase Spark plan: 50.000 letture / 20.000 scritture al giorno, 1 GB storage. Sufficiente per migliaia di clienti e visite quotidiane.

## Sicurezza

- Le **regole Firestore** consentono ad ogni utente di leggere/scrivere solo i propri dati. Nessun altro vede le tue informazioni.
- Il login Google e' gestito direttamente da Google (Firebase non vede la tua password).
- I dati sono criptati in transito (HTTPS) e a riposo (Google Cloud).

## Backup manuale

Per un backup esterno, in qualsiasi momento puoi:
1. Aprire la console del browser (F12 > Console)
2. Digitare: `copy(localStorage.getItem('emilV4'))`
3. Incollare il risultato in un file di testo. E' un JSON che contiene tutto.
