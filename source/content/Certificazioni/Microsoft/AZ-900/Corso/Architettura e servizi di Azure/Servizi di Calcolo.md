### Macchine Virtuali (VM)

Le **Macchine Virtuali Azure** offrono **IaaS (Infrastructure as a Service)** puro. Avete il controllo completo del sistema operativo, delle configurazioni di rete e del software installato. Microsoft gestisce l'hardware fisico, voi gestite tutto il resto.

#### VM Singole

Una **VM singola** è un'istanza isolata di macchina virtuale. Adatta per carichi di lavoro semplici, sviluppo o test. Non offre protezione contro i riavvii pianificati della piattaforma Azure.

#### Availability Sets

Gli **Availability Sets** raggruppano VM in due categorie: **Fault Domain** (domini di guasto) e **Update Domain** (domini di aggiornamento).

-   **Fault Domain**: VM distribuite su rack hardware separati. Se un rack fallisce, le altre VM rimangono operative.
    
-   **Update Domain**: VM aggiornate in sequenza durante la manutenzione della piattaforma. Non tutte le VM vengono riavviate contemporaneamente.
    

**Quando usarli**: Quando avete poche VM che devono resistere a guasti hardware o riavvii pianificati. Non offrono scalabilità automatica.

#### Virtual Machine Scale Sets (VMSS)

I **Virtual Machine Scale Sets** permettono di creare e gestire un gruppo di VM identiche con **scalabilità automatica**. In base a regole (es. CPU > 70%), Azure aggiunge o rimuove istanze automaticamente.

**Quando usarli**: Applicazioni con carico variabile che richiedono scalabilità orizzontale. Il load balancer integrato distribuisce il traffico tra le istanze.

#### Azure Virtual Desktop

**Azure Virtual Desktop** è un servizio di virtualizzazione desktop e applicazioni. Permette di distribuire Windows 10/11 multi-sessione e applicazioni Office in cloud.

**Caso d'uso**: Aziende che vogliono desktop centralizzati, accessibili da qualsiasi dispositivo, senza gestire l'infrastruttura fisica on-premise.

***

### Container

Un **Container** è un'unità di software che impacchetta codice, runtime, librerie e dipendenze in un unico pacchetto leggero. A differenza delle VM, i container condividono il kernel del sistema operativo host: sono più veloci da avviare e consumano meno risorse.

#### Azure Container Instances (ACI)

**ACI** permette di eseguire container senza gestire server, orchestrazione o infrastruttura. Avviate un container con un comando, lo usate, lo eliminate.

**Caratteristiche**:

-   Avvio in secondi
    
-   Pagamento al secondo di utilizzo
    
-   Nessuna gestione di VM sottostanti
    
-   Ideale per task isolati, job batch, scaling event-driven
    

#### Azure Kubernetes Service (AKS)

**AKS** è un servizio Kubernetes gestito per orchestrare container in produzione. Kubernetes gestisce il deployment, la scalabilità, il bilanciamento del carico e la resilienza dei container.

**Caratteristiche**:

-   Orchestrazione complessa con pod, service, ingress
    
-   Auto-scaling dei nodi e dei pod
    
-   Aggiornamenti rolling senza downtime
    
-   Ideale per microservizi, applicazioni enterprise, team DevOps
    

**Confronto secco**:

Table

Copy

| Caratteristica | ACI | AKS |
| :--- | :--- | :--- |
| Complessità | Bassa | Alta |
| Tempo di setup | Minuti | Ore/Giorni |
| Orchestrazione | Nessuna | Kubernetes completo |
| Scaling | Manuale o semplice | Automatico avanzato |
| Caso d'uso | Job isolati, prototipi | Produzione enterprise |

***

### App Service e Funzioni

#### Azure App Service

**Azure App Service** è una piattaforma **PaaS (Platform as a Service)** per ospitare applicazioni web, API REST e backend mobile senza gestire l'infrastruttura sottostante.

**Caratteristiche**:

-   Supporta .NET, Java, Node.js, Python, PHP
    
-   Deployment continuo da GitHub, Azure DevOps, Docker Hub
    
-   Auto-scaling integrato
    
-   SSL, dominio personalizzato, backup integrati
    

**Quando usarlo**: Siti web, API, applicazioni web che non richiedono controllo a livello di sistema operativo.

#### Azure Functions

**Azure Functions** è un servizio **serverless** che esegue codice in risposta a eventi. Non gestite server, non pagate per risorse inattive.

**Event-driven**: Il codice si attiva solo quando si verifica un evento. Esempi di trigger:

-   HTTP request (API chiamata)
    
-   Messaggio in una coda (Azure Queue Storage)
    
-   File caricato in blob storage
    
-   Timer schedulato
    

**Analogia**: Immaginate un interruttore della luce. La lampadina è spenta, non consuma energia. Quando premete l'interruttore (l'evento), la lampadina si accende (il codice gira). Quando rilasciate l'interruttore, la lampadina si spegne (il codice termina, il conteggio si ferma).

**Caratteristiche**:

-   Pagamento solo per il tempo di esecuzione (milioni di esecuzioni gratuite al mese)
    
-   Scaling automatico istantaneo da zero a migliaia di istanze
    
-   Linguaggi supportati: C#, Java, JavaScript, Python, PowerShell
    

***

## Quando Scegliere Cosa

Table

Copy

| Servizio | Modello | Quando usarlo |
| :--- | :--- | :--- |
| **VM Singola** | IaaS | Sviluppo, test, applicazioni legacy che richiedono controllo OS completo |
| **Availability Sets** | IaaS | Poche VM in produzione che devono resistere a guasti/riavvii |
| **VM Scale Sets** | IaaS | Applicazioni con carico variabile che richiedono scalabilità automatica |
| **Azure Virtual Desktop** | IaaS | Desktop virtualizzati per utenti remoti, VDI in cloud |
| **ACI** | Container | Job isolati, task batch, scaling rapido senza orchestrazione |
| **AKS** | Container | Microservizi in produzione, orchestrazione complessa, DevOps |
| **App Service** | PaaS | Siti web, API, applicazioni web senza gestione server |
| **Azure Functions** | Serverless | Codice event-driven, integrazioni, task automatizzati, API semplici |

**Regola pratica**:

-   Serve controllo completo del sistema? → **VM**
    
-   Serve scalabilità automatica di VM? → **VM Scale Sets**
    
-   Serve orchestrazione container complessa? → **AKS**
    
-   Serve un container veloce senza complicazioni? → **ACI**
    
-   Serve ospitare un sito/API senza gestire server? → **App Service**
    
-   Serve codice che gira solo su evento? → **Azure Functions**