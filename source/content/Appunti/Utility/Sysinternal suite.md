# Sysinternals Suite

La **Sysinternals Suite** è una raccolta di utilities avanzate per Windows, create per aiutare i professionisti IT e gli sviluppatori a gestire, risolvere problemi (troubleshooting) e diagnosticare applicazioni e sistemi operativi Windows.

### Come ottenere gli strumenti

Esistono diversi modi per scaricare o utilizzare questi strumenti:

1.  **Download Manuale:**
    
    -   [Pagina Principale Sysinternals](https://learn.microsoft.com/en-us/sysinternals/downloads/)
        
2.  **Download via PowerShell:**
    
    PowerShell
    
    ```
    Download-SysInternalsTools C:\Tools\Sysint
    ```
    
3.  **Sysinternals Live (Esecuzione Web):** È possibile eseguire gli strumenti direttamente dal web senza scaricarli manualmente.
    
    -   **Via Esplora Risorse:** Inserire `\\live.sysinternals.com\tools\<nometool>` nella barra degli indirizzi.
        
    -   **Via Prompt dei Comandi:** Eseguire `\\live.sysinternals.com\tools\<nometool>.exe`.
        

> **Nota Tecnica per Sysinternals Live:** Per far funzionare questo metodo, il servizio **WebClient** deve essere in esecuzione e la network discovery attiva.
> 
> Comando per installare/attivare il supporto necessario (su Windows Server):
> 
> PowerShell
> 
> ```
> Install-WindowsFeature WebDAV-Redirector –Restart
> Get-WindowsFeature WebDAV-Redirector | Format-Table –Autosize
> ```

***

## 1. Analisi dei Processi e del Sistema

### Process Explorer (ProcExp)

Process Explorer è un task manager avanzato. Mostra un elenco dei processi attivi e permette di approfondire cosa sta facendo ciascuno di essi. L'interfaccia è divisa in due finestre:

-   **Finestra Superiore:** Mostra i processi attivi e gli account proprietari.
    
-   **Finestra Inferiore:** Cambia in base alla modalità:
    
    -   _Handle Mode:_ Mostra gli handle aperti dal processo selezionato.
        
    -   _DLL Mode:_ Mostra le DLL e i file mappati in memoria caricati dal processo.
        

> **Codifica a Colori:** I processi in Process Explorer sono colorati in modo diverso per indicarne la natura (es. Servizi, Processi sospesi, Processi .NET).

### Process Monitor (ProcMon)

Uno strumento di monitoraggio avanzato che mostra in tempo reale l'attività del **File System**, del **Registro** e dei **Processi/Thread**. Combina le funzionalità dei vecchi _Filemon_ e _Regmon_.

-   **Caratteristiche chiave:** Filtraggio non distruttivo, proprietà complete degli eventi (Session ID, User Name), e stack dei thread.
    
-   **Utilizzo:** Essenziale per il troubleshooting e la caccia ai malware.
    
-   [Guida alla configurazione di ProcMon](https://adamtheautomator.com/procmon/)
    

### ProcDump

Un'utility da riga di comando progettata per monitorare i picchi di CPU di un'applicazione e generare un **crash dump** (file di dump della memoria) durante il picco. Questo file può essere analizzato successivamente da amministratori o sviluppatori per capire la causa del problema.

-   _Nota:_ Anche Process Explorer può creare file di dump manualmente.
    

***

## 2. Analisi dei File e del Disco

### Sigcheck

Mostra il numero di versione del file, le informazioni sul timestamp e i dettagli della firma digitale (comprese le catene di certificati).

-   **Integrazione VirusTotal:** Include un'opzione per controllare l'hash del file su VirusTotal o caricare il file per la scansione.
    
-   **Caso d'uso:** Verificare la presenza di file non firmati in directory sensibili.
    
    DOS
    
    ```
    sigcheck -u -e C:\Windows\System32
    ```
    

### Streams

Il file system NTFS permette di creare **Alternate Data Streams (ADS)**. Per impostazione predefinita, i dati sono nel flusso principale, ma è possibile nascondere dati in flussi alternativi (sintassi `file:stream`).

-   **Il problema:** Esplora Risorse non mostra nativamente gli ADS. Gli attaccanti li usano per nascondere dati o codice.
    
-   **Uso legittimo:** I browser aggiungono un identificatore "Zone.Identifier" negli ADS per marcare i file scaricati da Internet.
    
-   **Comando per leggere uno stream:** `notepad file.txt:streamfile.txt`
    

### Strings

Scansiona un file alla ricerca di stringhe UNICODE o ASCII (di default lunghe almeno 3 caratteri). Utile per estrarre testo leggibile da file binari o eseguibili sospetti.

***

## 3. Avvio e Persistenza

### Autoruns

Lo strumento più completo per monitorare le posizioni di avvio automatico. Mostra quali programmi sono configurati per essere eseguiti durante il boot o il login.

-   **Cosa controlla:** Cartelle di avvio, chiavi Run/RunOnce del Registro, estensioni della shell, toolbar, oggetti helper del browser, servizi, e molto altro.
    
-   **Utilizzo:** Fondamentale per rilevare meccanismi di **Persistenza** utilizzati dai malware.
    

***

## 4. Networking

### TCPView

Mostra elenchi dettagliati di tutti gli endpoint TCP e UDP sul sistema, inclusi indirizzi locali/remoti e lo stato delle connessioni.

-   **Vantaggio:** Riporta il nome del processo che possiede la connessione.
    
-   **Alternativa nativa:** Windows include `Resource Monitor` (comando: `resmon`) che offre funzionalità simili.
    

***

## 5. Gestione e Informazioni di Sistema

### Sysmon (System Monitor)

Un servizio di sistema e driver che rimane residente dopo il riavvio per monitorare e registrare l'attività del sistema nel registro eventi di Windows.

-   **Cosa traccia:** Creazione di processi, connessioni di rete, modifiche ai file, ecc.
    
-   **Utilizzo:** Fornisce dati granulari essenziali per i SIEM per identificare attività anomale o movimenti di intrusi.
    

### PsExec

Un sostituto leggero di telnet che permette di eseguire processi su altri sistemi con piena interattività per le applicazioni console, senza installare client manualmente. Spesso usato per lanciare prompt dei comandi remoti.

### WinObj

Visualizza lo spazio dei nomi dell'**Object Manager** di Windows NT.

-   **Concetto Chiave: Sessioni.**
    
    -   **Session 0:** Sessione del Sistema Operativo (isolata).
        
    -   **Session 1:** Sessione del primo utente loggato.
        
    -   Ci sono almeno due processi `csrss.exe`, uno per ogni sessione.
        

### BgInfo

Genera automaticamente un'immagine di sfondo del desktop contenente informazioni rilevanti sul sistema (Nome PC, IP, Service Pack, ecc.). Utile per amministratori che gestiscono molti server.

### RegJump

Una piccola applet da riga di comando che apre l'editor del registro (Regedit) direttamente al percorso specificato. Accetta anche abbreviazioni (es. HKLM).

***

## Scenario Reale: Troubleshooting di un Agente

Come Security Engineer, ecco un esempio pratico di come questi strumenti vengono usati insieme per risolvere un problema (es. un agente di sicurezza che non risponde su un endpoint):

1.  **ProcExp (Process Explorer):** Utilizzato per ispezionare il processo dell'agente, verificare le sue proprietà, i thread attivi e gli handle aperti.
    
2.  **ProcMon (Process Monitor):** Utilizzato per indagare se ci sono errori "Access Denied" o file mancanti che impediscono all'agente di funzionare correttamente.
    
3.  **ProcDump:** Utilizzato per creare un dump della memoria del processo bloccato, da inviare al fornitore (vendor) per un'analisi approfondita del codice.