# Analisi degli Strumenti Comunemente Abusati (Living off the Land)

Gli strumenti di sistema legittimi, spesso chiamati "Living off the Land" binaries (LOLBins), forniscono funzionalità di scripting, gestione, gestione file o pianificazione. Queste capacità coincidono perfettamente con le necessità degli attaccanti, come l'esecuzione di codice, la persistenza, la ricognizione e il movimento laterale.

Ecco i principali esempi trattati:

-   **PowerShell:** Utilizzato per script in memoria, download remoti e automazione.
    
-   **WMIC (o WMI):** Utilizzato per eseguire comandi localmente o su host remoti e per interrogare lo stato del sistema.
    
-   **Certutil:** Utilizzato per scaricare file e codificare o decodificare payload.
    
-   **Mshta:** Utilizzato per eseguire contenuto HTA o script inline tramite documenti o link.
    
-   **Rundll32:** Utilizzato per invocare esportazioni DLL o attivare gestori URL.
    
-   **Scheduled tasks (schtasks):** Utilizzati per eseguire codice al logon o secondo una pianificazione per garantire la persistenza.
    

> **Risorse Utili:**
> 
> -   [LOLBAS](https://lolbas-project.github.io/) (Per Windows)
>     
> -   [GTFOBins](https://gtfobins.github.io/) (Per Linux)
>     

***

### 1. PowerShell

PowerShell è un motore di scripting utilizzato per l'amministrazione e l'automazione nei sistemi Windows.

**Perché gli attaccanti lo usano:** Gli attaccanti sfruttano PowerShell perché può eseguire script direttamente in memoria (fileless) senza toccare il disco, automatizzare azioni di sistema, interagire con la rete e bypassare alcune policy di esecuzione. Gli scopi comuni includono il download di payload, la raccolta di informazioni e la modifica delle impostazioni di sistema in modo furtivo.


### 2. WMIC

WMIC (Windows Management Instrumentation Command-line) permette agli amministratori di interrogare e gestire sistemi Windows locali o remoti.

**Perché gli attaccanti lo usano:** È comunemente usato dagli attori delle minacce per eseguire comandi o creare processi da remoto, raccogliere informazioni sul sistema o stabilire la persistenza senza utilizzare binari esterni. Poiché si mescola con il normale comportamento amministrativo, è spesso consentito negli ambienti limitati.

### 3. Certutil

Certutil è uno strumento Microsoft destinato alla gestione dei certificati. Tuttavia, possiede funzionalità per scaricare file (con `-urlcache`) e decodificare dati.

**Perché gli attaccanti lo usano:** Gli attaccanti lo utilizzano perché è firmato da Microsoft ed è comune nei flussi di lavoro amministrativi. La sua capacità di trasformare blocchi di testo in binari (decodifica Base64) e di scaricare file permette di piazzare malware bypassando strumenti come `curl` e superando semplici regole di blocco. Viene spesso usato per lo "staging" dei payload.


### 4. MSHTA

Mshta esegue file HTA (HTML Application), i quali possono contenere ed eseguire codice VBScript o JavaScript con privilegi elevati rispetto a un normale browser.


### 5. Rundll32

Rundll32 viene utilizzato per caricare ed eseguire funzioni specifiche esportate da file DLL (Dynamic Link Library).

### 6. Scheduled Tasks (Task Scheduler / schtasks)

L'Utilità di pianificazione è una funzionalità integrata di Windows che permette agli amministratori di eseguire programmi o script in momenti specifici, al verificarsi di eventi (come il logon) o secondo una pianificazione ripetuta.

**Perché gli attaccanti lo usano:** È un meccanismo primario per la **persistenza**. Gli attaccanti creano o modificano le attività per rieseguire il codice dannoso dopo un riavvio o al login dell'utente. Spesso scelgono nomi che sembrano benigni (es. "WindowsUpdate" o "Manutenzione") per non attirare l'attenzione e utilizzano account di sistema per eseguire i task.