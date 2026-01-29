Dove risiede il cloud lo abbiamo visto nel documento. Ora è il momento di capire _cosa_ si acquista esattamente. I modelli di servizio definiscono il livello di controllo e di responsabilità che si ha rispetto all'infrastruttura. Esistono tre modelli principali — IaaS, PaaS e SaaS — più una variante emergente chiamata Serverless (FaaS). Per ognuno, la divisione delle responsabilità tra provider e cliente cambia in modo significativo.

## Infrastructure as a Service (IaaS)

L'IaaS fornisce risorse di infrastruttura virtualizzate via Internet. Si tratta del livello più "basico" del cloud, dove il provider mette a disposizione server virtuali, storage e networking, ma il cliente mantiene il controllo completo su ciò che viene eseguito sopra.

**Caratteristiche principali:**

-   **Controllo totale del sistema operativo**: Il cliente sceglie, installa e gestisce Windows o Linux
    
-   **Responsabilità completa dello stack software**: Il cliente gestisce patch del SO, middleware, runtime e applicazioni
    
-   **Flessibilità massima**: Possibilità di configurare l'ambiente esattamente come necessario
    
-   **Modello di pagamento**: Generalmente per risorse allocate (compute hours), non solo per utilizzo effettivo
    

**Esempi di servizi Azure:**

-   Azure Virtual Machines
    
-   Azure Virtual Network
    
-   Azure Storage (Blob, Files)
    
-   Azure Load Balancer
    

**Casi d'uso tipici:**

-   Migrazione "lift and shift" di server fisici esistenti nel cloud senza modificare le applicazioni
    
-   Esecuzione di software legacy che richiede configurazioni specifiche del sistema operativo
    
-   Ambienti di sviluppo/test che necessitano di isolamento completo
    
-   Database self-managed (es. SQL Server installato su VM)
    

**Esempio pratico**: Un'azienda ha un'applicazione ERP che gira su Windows Server 2019 con SQL Server. Per migrare al cloud senza riscrivere codice, crea una Virtual Machine su Azure, installa Windows Server, configura SQL Server e sposta l'applicazione. Il provider gestisce l'hardware fisico e la rete, ma l'azienda deve occuparsi di tutti gli aggiornamenti di sicurezza di Windows e SQL Server.

## Platform as a Service (PaaS)

Il PaaS offre una piattaforma completa per sviluppare, distribuire e gestire applicazioni senza doversi occupare dell'infrastruttura sottostante. Il provider gestisce non solo l'hardware, ma anche il sistema operativo, il middleware e i runtime.

**Caratteristiche principali:**

-   **Focus sul codice**: Gli sviluppatori si concentrano sull'applicazione, non sulla gestione del server
    
-   **Ambiente pre-configurato**: Il runtime (Node.js, .NET, Python, Java) è già installato e mantenuto dal provider
    
-   **Scalabilità integrata**: Possibilità di configurare scaling automatico in base al carico
    
-   **Deployment semplificato**: Push del codice tramite Git, CI/CD integrato
    

**Esempi di servizi Azure:**

-   Azure App Service (Web Apps, API Apps)
    
-   Azure SQL Database (managed)
    
-   Azure Cosmos DB
    
-   Azure Functions (anche se tecnicamente Serverless/FaaS)
    
-   Azure Logic Apps
    

**Casi d'uso tipici:**

-   Sviluppo di nuove applicazioni web o API
    
-   Database gestiti dove non si vuole occuparsi di patching e backup
    
-   Microservizi e architetture moderne
    
-   Elaborazione di flussi di lavoro e integrazione tra sistemi
    

**Esempio pratico**: Un team di sviluppo deve creare un'API REST per un'applicazione mobile. Invece di configurare server, installare IIS o Nginx, gestire certificati SSL e patch di sicurezza, utilizza Azure App Service. Carica il codice (in C#, Node.js o Python), e la piattaforma gestisce automaticamente il runtime, il bilanciamento del carico, gli aggiornamenti di sicurezza del sistema operativo e lo scaling.

## Software as a Service (SaaS)

Il SaaS fornisce applicazioni software complete, pronte all'uso, accessibili tramite Internet. Il provider gestisce l'intero stack — dall'hardware all'applicazione — e il cliente utilizza semplicemente il software.

**Caratteristiche principali:**

-   **Zero gestione infrastrutturale**: Nessun server da configurare, nessun aggiornamento da installare
    
-   **Accesso immediato**: Dopo la sottoscrizione, il software è immediatamente disponibile
    
-   **Configurazione limitata**: Il cliente può personalizzare solo parametri e impostazioni dell'applicazione, non il codice sottostante
    
-   **Modello di pagamento**: Generalmente per utente/mese (subscription)
    

**Esempi comuni:**

-   Microsoft 365 (Outlook, Word, Excel online)
    
-   Salesforce
    
-   Dropbox
    
-   Google Workspace
    

**Casi d'uso tipici:**

-   Posta elettronica aziendale
    
-   Strumenti di produttività e collaborazione
    
-   CRM e gestione clienti
    
-   Storage e condivisione file
    

**Esempio pratico**: Un'azienda ha bisogno di un sistema di posta elettronica per 50 dipendenti. Invece di comprare un server Exchange, installarlo, configurarlo e mantenerlo aggiornato, sottoscrive Microsoft 365. I dipendenti accedono a Outlook via web o applicazione desktop, e Microsoft gestisce tutto: hardware, storage, sicurezza, aggiornamenti, backup.

## Serverless (Function as a Service - FaaS)

Il Serverless rappresenta un'evoluzione del PaaS dove si arriva a gestire singole funzioni invece di applicazioni complete. Nonostante il nome, i server esistono, ma sono completamente invisibili al cliente.

**Caratteristiche principali:**

-   **Esecuzione event-driven**: Il codice viene eseguito solo in risposta a specifici trigger (HTTP request, messaggio in coda, timer, upload file)
    
-   **Scaling automatico estremo**: Da zero a migliaia di istanze in base alla richiesta
    
-   **Billing per esecuzione**: Si paga solo per il tempo di esecuzione effettivo (frazioni di secondo) e non per server allocati
    
-   **Statelessness**: Ogni esecuzione è indipendente, senza stato persistente (a meno di non usare servizi aggiuntivi)
    

**Esempi di servizi Azure:**

-   Azure Functions
    
-   Azure Logic Apps (workflow serverless)
    
-   Azure Event Grid
    

**Casi d'uso tipici:**

-   Elaborazione di file (es. resize immagini caricate su storage)
    
-   API leggere e sporadiche
    
-   Automazione e task schedulati
    
-   Integrazione tra sistemi (ETL leggeri)
    
-   Elaborazione di stream di dati IoT
    

**Esempio pratico**: Un'applicazione permette agli utenti di caricare foto. Ogni volta che un'immagine viene caricata su Azure Blob Storage, un Azure Function si attiva automaticamente, crea una versione ridotta (thumbnail) e la salva in un altro container. Se non ci sono upload, la funzione non consuma risorse né genera costi. Durante un picco di traffico, Azure scala automaticamente le istanze della funzione per gestire tutte le richieste simultanee.

## Il Modello di Responsabilità Condivisa

Un concetto fondamentale per comprendere i modelli di servizio è il **Shared Responsibility Model**. Questo framework definisce chi è responsabile di cosa nella gestione e sicurezza dell'ambiente cloud. La linea di demarcazione si sposta in base al modello scelto.

### On-Premises (riferimento)

In un data center tradizionale, l'organizzazione è responsabile di tutto:

-   Sicurezza fisica dell'edificio
    
-   Hardware (server, storage, rete)
    
-   Virtualizzazione
    
-   Sistema operativo
    
-   Middleware e runtime
    
-   Applicazioni
    
-   Dati e accessi
    

### IaaS - Responsabilità

**Provider (Microsoft):**

-   Sicurezza fisica dei data center
    
-   Rete fisica e host fisici
    
-   Hypervisor (livello di virtualizzazione)
    

**Cliente:**

-   Sistema operativo (patching, configurazione, hardening)
    
-   Middleware e runtime
    
-   Applicazioni
    
-   Dati
    
-   Identità e accessi
    
-   Configurazione della rete virtuale e firewall
    

### PaaS - Responsabilità

**Provider (Microsoft):**

-   Tutto ciò che riguarda l'IaaS, più:
    
-   Sistema operativo (patching automatico)
    
-   Middleware (web server, database engine)
    
-   Runtime (Node.js, .NET, Java)
    

**Cliente:**

-   Applicazioni (codice e configurazione)
    
-   Dati
    
-   Identità e accessi
    
-   Impostazioni di sicurezza dell'applicazione
    

### SaaS - Responsabilità

**Provider (Microsoft):**

-   L'intero stack tecnologico
    
-   Applicazione software
    
-   Tutti gli aggiornamenti e patch
    

**Cliente:**

-   Dati inseriti nell'applicazione
    
-   Gestione degli utenti e dei permessi
    
-   Configurazione delle impostazioni di sicurezza disponibili
    
-   Dispositivi endpoint utilizzati per accedere al servizio
    

### Sintesi visiva delle responsabilità

Table

Copy

| Componente | On-Prem | IaaS | PaaS | SaaS |
| :--- | :--- | :--- | :--- | :--- |
| Dati | Cliente | Cliente | Cliente | Cliente |
| Applicazioni | Cliente | Cliente | Cliente | Provider |
| Runtime/Middleware | Cliente | Cliente | Provider | Provider |
| Sistema Operativo | Cliente | Cliente | Provider | Provider |
| Virtualizzazione | Cliente | Provider | Provider | Provider |
| Hardware fisico | Cliente | Provider | Provider | Provider |
| Rete fisica | Cliente | Provider | Provider | Provider |

**Esempio pratico di responsabilità**: In un'applicazione PaaS su Azure App Service, Microsoft si occupa di patchare il sistema operativo Windows sottostante e di mantenere aggiornato IIS (il web server). Il cliente deve però assicurarsi che il proprio codice non abbia vulnerabilità (SQL injection, XSS), che i dati sensibili siano crittografati, e che le credenziali di accesso siano gestite correttamente. Se il cliente scegliesse IaaS (VM), sarebbe responsabile anche degli aggiornamenti di Windows e della configurazione di IIS.

## Sintesi e confronto dei modelli

Table

Copy

| Aspetto | IaaS | PaaS | SaaS | Serverless |
| :--- | :--- | :--- | :--- | :--- |
| **Controllo** | Massimo | Medio | Minimo | Basso (solo codice) |
| **Gestione richiesta** | Alta (OS, middleware) | Media (solo app) | Bassa (configurazione) | Minima |
| **Flessibilità** | Massima | Buona | Limitata | Limitata a casi specifici |
| **Tempo di deployment** | Giorni/Ore | Ore/Minuti | Minuti | Secondi |
| **Modello di costo** | Risorse allocate | Risorse allocate + features | Per utente/mese | Per esecuzione |
| **Casi d'uso** | Legacy, lift-shift | Nuove app, microservizi | Produttività, standard | Eventi, automazione, API leggere |

La scelta del modello dipende dal livello di controllo necessario, dalle competenze interne disponibili e dal tipo di workload. Generalmente, se si migra un sistema esistente senza modifiche, si tende verso IaaS. Se si sviluppa nuovo codice, PaaS offre il miglior bilanciamento. Per software standard (posta, office automation), SaaS è la scelta ovvia. Il Serverless è ideale per workload event-driven e sporadici.