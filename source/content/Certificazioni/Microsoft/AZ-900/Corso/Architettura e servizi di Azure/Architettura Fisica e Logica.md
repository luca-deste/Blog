# Componenti Principali dell'Architettura di Azure

## 1. Struttura Fisica dell'Infrastruttura Azure

### 1.1 Regioni di Azure (Azure Regions)

**Definizione**: Una regione di Azure è un'area geografica contenente uno o più data center connessi tramite rete a bassa latenza. Ogni regione è identificata da un nome (es. "West Europe", "East US").

**Caratteristiche Tecniche**:

-   Ogni regione è isolata dalle altre per garantire tolleranza ai guasti
    
-   La latenza tra data center nella stessa regione è inferiore ai 2ms
    
-   Ogni regione appartiene a una specifica località geografica (sovranità dei dati)
    
-   Disponibilità di servizi specifici per regione (non tutti i servizi sono disponibili in tutte le regioni)
    

**Identificazione**:

-   Nome visualizzato: "West Europe"
    
-   Nome interno: "westeurope"
    
-   Paese di localizzazione: Paesi Bassi
    

**Casi d'Uso**:

-   Distribuzione di applicazioni vicino agli utenti finali (latenza ridotta)
    
-   Conformità normativa per residenza dei dati
    
-   Ridondanza geografica per disaster recovery
    

***

### 1.2 Coppie di Regioni (Region Pairs)

**Definizione**: Ogni regione Azure è accoppiata con un'altra regione nella stessa area geografica (stesso paese o continente), separata da almeno 300 miglia (480 km).

**Caratteristiche Tecniche**:

-   Replica automatica della piattaforma (non dei dati utente) tra regioni accoppiate
    
-   Priorità di ripristino in caso di interruzione su vasta area
    
-   Aggiornamenti della piattaforma sequenziali (non simultanei) tra regioni accoppiate
    
-   Coppie fisse definite da Microsoft, non modificabili dall'utente
    

**Esempi di Coppie**:

-   West Europe (Paesi Bassi) ↔ North Europe (Irlanda)
    
-   East US (Virginia) ↔ West US (California)
    
-   Southeast Asia (Singapore) ↔ East Asia (Hong Kong)
    

**Meccanismo di Failover**:

-   In caso di interruzione di una regione, Azure priorizza il ripristino della regione accoppiata
    
-   La replica dei dati utente deve essere configurata esplicitamente (non automatica)
    

**Casi d'Uso**:

-   Disaster recovery geografico
    
-   Business continuity planning
    
-   Protezione contro eventi catastrofici a livello regionale
    

***

### 1.3 Zone di Disponibilità (Availability Zones)

**Definizione**: Zone di disponibilità sono data center fisicamente separati all'interno della stessa regione Azure. Ogni zona è un'unità di disponibilità indipendente con alimentazione, raffreddamento e networking dedicati.

**Caratteristiche Tecniche**:

-   Ogni regione supportante le Availability Zones ne contiene almeno 3 (tipicamente 3)
    
-   Distanza fisica tra zone: decine di chilometri (minimo 10 km)
    
-   Latenza tra zone: inferiore a 2ms
    
-   Connessione tramite rete fibra ottica privata ad alta velocità
    

**Servizi Supportati**:

-   **Servizi Zonal**: Risorse vincolate a una zona specifica (VM, Dischi gestiti, IP pubblici standard)
    
-   **Servizi Zone-Redundant**: Risorse replicate automaticamente tra zone (SQL Database, Storage, Service Bus)
    

**Identificazione**:

-   Numerazione: Zona 1, Zona 2, Zona 3
    
-   Ogni zona è un data center fisico distinto con ID univoco
    

**SLA (Service Level Agreement)**:

-   VM in singola zona: 99.9% uptime
    
-   VM distribuite tra due zone con Load Balancer: 99.99% uptime
    

**Casi d'Uso**:

-   Alta disponibilità per applicazioni mission-critical
    
-   Protezione contro guasti a livello di data center
    
-   Distribuzione di VM in configurazione active-active o active-passive
    

***

## 2. Struttura Logica di Gestione

### 2.1 Risorse (Resources)

**Definizione**: Una risorsa è un'istanza gestibile di un servizio Azure. Rappresenta qualsiasi elemento creato, distribuito e gestito all'interno di Azure.

**Tipologie di Risorse**:

-   **Compute**: Macchine virtuali, set di scalabilità, Azure Kubernetes Service
    
-   **Storage**: Account di archiviazione, dischi gestiti, file share
    
-   **Networking**: Reti virtuali, interfacce di rete, indirizzi IP pubblici, load balancer
    
-   **Database**: SQL Database, Cosmos DB, Azure Database for PostgreSQL
    
-   **Web**: App Service, Azure Functions, Logic Apps
    
-   **Identità**: Azure AD tenant, app registrations, service principals
    

**Proprietà delle Risorse**:

-   **Resource ID**: Identificatore univoco globale `/subscriptions/{sub-id}/resourceGroups/{rg-name}/providers/{provider}/{type}/{name}`
    
-   **Tipo di risorsa**: Definito dal provider di risorse (es. `Microsoft.Compute/virtualMachines`)
    
-   **API Version**: Ogni tipo di risorsa ha versioni API specifiche
    
-   **Location**: Regione geografica dove la risorsa risiede fisicamente
    
-   **Tags**: Metadati chiave-valore (max 50 tag per risorsa, chiave max 512 caratteri, valore max 256 caratteri)
    
-   **Proprietà**: Configurazione specifica del servizio (dimensioni VM, tier di storage, ecc.)
    

**Stati della Risorsa**:

-   **Running**: Risorsa attiva e funzionante
    
-   **Stopped**: Risorsa arrestata (alcuni costi possono persistere)
    
-   **Deallocated**: Risorsa deallocata (VM non in esecuzione, nessun costo di calcolo)
    

**Casi d'Uso**:

-   Ogni componente distribuito in Azure è una risorsa
    
-   Gestione del ciclo di vita dei servizi cloud
    
-   Monitoraggio e configurazione granulari
    

***

### 2.2 Gruppi di Risorse (Resource Groups)

**Definizione**: Contenitori logici che raggruppano risorse correlate per un'applicazione o un progetto. Ogni risorsa deve appartenere a uno e un solo gruppo di risorse.

**Caratteristiche Tecniche**:

-   **Ambito di gestione**: Gruppo di risorse è un livello di gestione, non di isolamento di rete
    
-   **Metadati condivisi**: Tutte le risorse nel gruppo condividono tag (ereditarietà opzionale), policy e lock
    
-   **Ciclo di vita comune**: Eliminando un gruppo di risorse, tutte le risorse al suo interno vengono eliminate
    
-   **Regione del gruppo**: Ogni gruppo di risorse ha una regione di metadati (dove vengono memorizzati i metadati del gruppo), che può differire dalla regione delle risorse contenute
    

**Limiti**:

-   Massimo 980 gruppi di risorse per sottoscrizione
    
-   Una risorsa può appartenere a un solo gruppo di risorse
    
-   Le risorse possono essere spostate tra gruppi di risorse (con alcune limitazioni)
    

**Strategie di Organizzazione**:

-   **Per ambiente**: RG-Production, RG-Development, RG-Testing
    
-   **Per applicazione**: RG-App1, RG-App2
    
-   **Per tier**: RG-WebTier, RG-DataTier, RG-MiddleTier
    
-   **Per reparto**: RG-Finance, RG-Marketing, RG-IT
    
-   **Per ciclo di vita**: RG-Temporary, RG-Persistent
    

**Strumenti di Gestione**:

-   **Role-Based Access Control (RBAC)**: Assegnazione di permessi a livello di gruppo di risorse
    
-   **Resource Locks**: Prevenzione eliminazione o modifica accidentale (CanNotDelete, ReadOnly)
    
-   **Policy di Azure**: Applicazione di regole a livello di gruppo
    
-   **Tagging**: Organizzazione e categorizzazione
    

**Casi d'Uso**:

-   Organizzazione logica di risorse correlate
    
-   Gestione dei permessi per team
    
-   Deployment e eliminazione coordinati
    
-   Monitoraggio e cost tracking aggregato
    

***

### 2.3 Sottoscrizioni (Subscriptions)

**Definizione**: Una sottoscrizione è un contenitore logico che funge da limite di fatturazione e controllo degli accessi. Rappresenta un accordo con Microsoft per utilizzare i servizi Azure.

**Caratteristiche Tecniche**:

-   **Limite di fatturazione**: Tutti i costi delle risorse sono aggregati a livello di sottoscrizione
    
-   **Limite di quota**: Ogni sottoscrizione ha limiti predefiniti per tipo di risorsa (es. 2500 CPU core per regione)
    
-   **Tenant association**: Ogni sottoscrizione è associata a un tenant Azure AD
    
-   **Account di fatturazione**: Una sottoscrizione è collegata a un metodo di pagamento (credit card, enterprise agreement, CSP)
    

**Tipi di Sottoscrizione**:

-   **Free Trial**: 30 giorni, $200 di credito, limitata a 1 per account
    
-   **Pay-As-You-Go**: Pagamento in base al consumo, nessun impegno
    
-   **Enterprise Agreement (EA)**: Contratto aziendale con sconti basati sul commit di spesa
    
-   **Cloud Solution Provider (CSP)**: Acquisto tramite partner Microsoft
    
-   **Azure for Students**: $100 di credito annuale, no carta di credito richiesta
    
-   **Visual Studio Subscription**: Crediti mensili per sviluppatori
    

**Limiti e Quote**:

-   Massimo 2000 sottoscrizioni per tenant Azure AD
    
-   Limiti di core CPU per regione e per sottoscrizione
    
-   Limiti di risorse per tipo (es. 1000 VM per regione)
    

**Strategie di Organizzazione**:

-   **Per ambiente**: Subscription-Prod, Subscription-Dev, Subscription-Test
    
-   **Per reparto**: Subscription-IT, Subscription-HR, Subscription-Finance
    
-   **Per progetto**: Subscription-Project1, Subscription-Project2
    
-   **Per geografia**: Subscription-US, Subscription-EU, Subscription-APAC
    

**Gestione Accessi**:

-   **Azure AD**: Autenticazione e autorizzazione
    
-   **RBAC**: Ruoli predefiniti (Owner, Contributor, Reader) e ruoli personalizzati
    
-   **Azure AD Privileged Identity Management (PIM)**: Gestione degli accessi con privilegi
    

**Casi d'Uso**:

-   Separazione dei costi per reparto o progetto
    
-   Isolamento degli ambienti (produzione vs sviluppo)
    
-   Gestione delle quote e limiti di risorse
    
-   Controllo degli accessi a livello aziendale
    

***

### 2.4 Gruppi di Gestione (Management Groups)

**Definizione**: Contenitori che consentono di gestire l'accesso, le policy e la conformità per più sottoscrizioni. Facilitano la gestione gerarchica a livello enterprise.

**Caratteristiche Tecniche**:

-   **Gerarchia**: Supportano fino a 6 livelli di profondità (escluso il livello root e il livello sottoscrizione)
    
-   **Ereditarietà**: Le policy RBAC e le policy di Azure si propagano gerarchicamente verso il basso
    
-   **Root Management Group**: Gruppo di gestione predefinito alla radice della gerarchia (ID uguale all'ID del tenant Azure AD)
    
-   **Flessibilità**: Sottoscrizioni e gruppi di gestione possono essere spostati all'interno della gerarchia
    

**Struttura Gerarchica**:

plain

Copy

```plain
Root Management Group (Tenant)
├── Management Group: Corporate
│   ├── Management Group: IT
│   │   ├── Subscription: IT-Prod
│   │   └── Subscription: IT-Dev
│   ├── Management Group: HR
│   │   └── Subscription: HR-Prod
│   └── Management Group: Finance
│       └── Subscription: Finance-Prod
└── Management Group: Subsidiaries
    └── Subscription: Sub1-Prod
```

**Limiti**:

-   Massimo 10,000 gruppi di gestione per tenant
    
-   Massimo 6 livelli di profondità
    
-   Una sottoscrizione può appartenere a un solo gruppo di gestione
    
-   Tutte le sottoscrizioni e gruppi di gestione devono essere all'interno di un singolo tenant
    

**Funzionalità**:

-   **Policy inheritance**: Le policy applicate a un gruppo si applicano a tutte le sottoscrizioni figlie
    
-   **RBAC inheritance**: I ruoli assegnati a un gruppo ereditano a tutte le sottoscrizioni figlie
    
-   **Budget e cost management**: Monitoraggio dei costi aggregati per ramo della gerarchia
    
-   **Compliance monitoring**: Valutazione della conformità a livello di gruppo
    

**Casi d'Uso**:

-   Governance enterprise su larga scala
    
-   Applicazione di policy di sicurezza a livello aziendale
    
-   Gestione centralizzata degli accessi per multiple sottoscrizioni
    
-   Strutturazione gerarchica per conglomerati o multi-subsidiary
    

***

## 3. Relazioni e Gerarchia Completa

### 3.1 Gerarchia di Gestione Azure

plain

Copy

```plain
┌─────────────────────────────────────────────────────────────┐
│                    AZURE AD TENANT                          │
│                  (Identity Boundary)                        │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│              ROOT MANAGEMENT GROUP                          │
│           (Policy & RBAC Inheritance Root)                  │
└──────────────────────┬──────────────────────────────────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
┌───────▼──────┐ ┌────▼─────┐ ┌──────▼──────┐
│  Mgmt Group  │ │ Mgmt Grp │ │  Mgmt Group │
│  Corporate   │ │  IT      │ │    HR       │
└───────┬──────┘ └────┬─────┘ └──────┬──────┘
        │             │              │
   ┌────┴────┐   ┌────┴────┐    ┌────┴────┐
   │         │   │         │    │         │
┌──▼──┐   ┌──▼──┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐
│Sub-1│   │Sub-2│ │Sub-3│ │Sub-4│ │Sub-5│
│Prod │   │ Dev │ │Test │ │Prod │ │Prod │
└──┬──┘   └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘
   │         │       │       │       │
┌──▼─────────▼───────▼───────▼───────▼──┐
│           RESOURCE GROUPS              │
│  (Logical Containers for Resources)    │
└──────────────────┬─────────────────────┘
                   │
        ┌──────────┼──────────┐
        │          │          │
    ┌───▼───┐  ┌───▼───┐  ┌───▼───┐
    │Resource│  │Resource│  │Resource│
    │   A    │  │   B    │  │   C    │
    │ (VM)   │  │(Storage)│  │ (VNet) │
    └───┬───┘  └───┬───┘  └───┬───┘
        │          │          │
        └──────────┴──────────┘
                   │
        ┌──────────┴──────────┐
        │      REGIONS        │
        │  (Geographic Areas) │
        └──────────┬──────────┘
                   │
    ┌──────────────┼──────────────┐
    │              │              │
┌───▼───┐    ┌────▼────┐   ┌────▼────┐
│Region │    │ Region  │   │ Region  │
│ West  │    │  East   │   │  North  │
│Europe │    │   US    │   │ Europe  │
└───┬───┘    └────┬────┘   └────┬────┘
    │             │             │
┌───▼───┐    ┌────▼────┐   ┌────▼────┐
│Zone 1 │    │ Zone 1  │   │ Zone 1  │
│Zone 2 │    │ Zone 2  │   │ Zone 2  │
│Zone 3 │    │ Zone 3  │   │ Zone 3  │
└───────┘    └─────────┘   └─────────┘
```

### 3.2 Ereditarietà delle Policy e RBAC

**Direzione dell'Ereditarietà**: Top-down (dalla radice verso le foglie)

**Meccanismo**:

1.  Policy applicata al Root Management Group → si applica a tutte le sottoscrizioni
    
2.  Policy applicata a un Management Group → si applica a tutte le sottoscrizioni figlie dirette e indirette
    
3.  Policy applicata a una Subscription → si applica a tutti i Resource Groups
    
4.  Policy applicata a un Resource Group → si applica a tutte le risorse nel gruppo
    

**Esempio Pratico**:

-   Policy "Consentire solo VM in West Europe" applicata al Management Group "Corporate"
    
-   Tutte le sottoscrizioni sotto "Corporate" ereditano questa policy
    
-   Qualsiasi tentativo di creare una VM in East US viene bloccato
    

***

## 4. Considerazioni per l'Esame AZ-900

### 4.1 Domande Chiave

**Regioni vs Zone di Disponibilità**:

-   **Regioni**: Separazione geografica a livello di centinaia/migliaia di chilometri (disaster recovery)
    
-   **Zone**: Separazione fisica a livello di decine di chilometri all'interno della stessa regione (alta disponibilità)
    

**Gerarchia di Gestione**:

-   Ordine corretto: Tenant → Management Groups → Subscriptions → Resource Groups → Resources
    
-   Una risorsa non può esistere senza Resource Group
    
-   Una sottoscrizione può esistere senza Management Group (viene collocata sotto Root)
    

**Coppie di Regioni**:

-   La replica tra coppie è automatica per la piattaforma, non per i dati utente
    
-   Le coppie sono fisse e definite da Microsoft
    
-   La priorità di ripristino è data alla regione accoppiata
    

### 4.2 Errori Comuni da Evitare

1.  **Confondere Resource Group con rete virtuale**: Resource Group è un contenitore logico, non un confine di rete
    
2.  **Credere che le risorse in un Resource Group debbano essere nella stessa regione**: Falso, un Resource Group può contenere risorse da multiple regioni (sconsigliato per gestione)
    
3.  **Pensare che l'eliminazione di un Resource Group elimini solo il gruppo**: Elimina anche tutte le risorse contenute
    
4.  **Confondere Subscription con Tenant**: Un tenant può contenere multiple sottoscrizioni; una sottoscrizione appartiene a un solo tenant
    

### 4.3 Scenario Tipico d'Esame

**Scenario**: Un'azienda multinazionale necessita di:

-   Isolare i costi per dipartimento
    
-   Applicare policy di sicurezza a tutte le risorse
    
-   Garantire alta disponibilità per applicazioni critiche
    
-   Rispettare requisiti di sovranità dei dati
    

**Soluzione Corretta**:

1.  Creare un Management Group per ogni dipartimento
    
2.  Creare sottoscrizioni separate per ogni dipartimento sotto i relativi Management Groups
    
3.  Utilizzare Availability Zones per applicazioni mission-critical (99.99% SLA)
    
4.  Selezionare regioni specifiche per conformità normativa (es. "Germany West Central" per dati tedeschi)