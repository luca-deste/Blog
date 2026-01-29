Nella prima parte del corso [[Cos'è il Cloud]] abbiamo spiegato la definizione di cloud e abbiamo visto le differenze tra il modello di pagamento in cloud e il classico.

In questa seconda parte andiamo a toccare i seguenti punti:

- **Scalabilità** (Verticale e orizzontale)
-  **Elasticità**
- **Disponibilità**
- **Disaster recovery**
- **Affidabilità e prevedibilità**
- **Sicurezza e governance**
___
Riconosco che sembrano molti argomenti da toccare insieme, ma vi assicuro che in realtà cercheremo di toccare solamente i dettagli fondamentali di ogni argomento.
Iniziamo.
___

## Scalabilità
Quando si parla di cloud il concetto di **scalabilità** è fondamentale.
Questa è la capacità di un sistema di adattarsi a incrementi di carico di lavoro aumentando o diminuendo dinamicamente le risorse disponibili. In altre parole, la scalabilità permette di gestire picchi di traffico o di elaborazione senza che l'utente finale percepisca degrado nelle prestazioni.

Esistono due tipologie principali di scalabilità:

### Scalabilità Verticale (Scaling Up/Down)
La **scalabilità verticale** consiste nell'aumentare o diminuire le risorse di una singola istanza computazionale. Si parla di:
- **Scaling Up**: Aggiungere più potenza a una risorsa esistente (es. aumentare RAM, CPU o storage di una macchina virtuale)
- **Scaling Down**: Ridurre le risorse quando non sono più necessarie

**Esempio pratico su Azure**: Se hai una VM di tipo B2s (2 vCPU, 4 GB RAM) che sta raggiungendo il limite di utilizzo CPU, puoi scalare verticalmente passando a una B4ms (4 vCPU, 16 GB RAM) senza dover creare nuove istanze.

**Limiti**: La scalabilità verticale ha un tetto massimo dettato dall'hardware fisico disponibile nel data center. Inoltre, tipicamente richiede un riavvio della macchina virtuale, causando downtime.

### Scalabilità Orizzontale (Scaling Out/In)
La **scalabilità orizzontale** consiste nell'aggiungere o rimuovere istanze di risorse identiche per distribuire il carico di lavoro. Si parla di:
- **Scaling Out**: Aggiungere più istanze quando il carico aumenta
- **Scaling In**: Rimuovere istanze quando il carico diminuisce

**Esempio pratico su Azure**: Un'applicazione web ospitata su Azure App Service può essere configurata per scalare automaticamente da 2 a 10 istanze in base alla percentuale di utilizzo CPU. Se il traffico aumenta, Azure provisioninga nuove istanze; quando cala, le decommissiona.

**Vantaggi**: Non esiste un limite teorico superiore (oltre ai vincoli di budget), e il processo può avvenire senza interruzione del servizio grazie al bilanciamento del carico.

___

## Elasticità
Se la scalabilità è la *capacità* di crescere, l'**elasticità** è la *capacità di farlo automaticamente e in tempo reale* in risposta alla domanda effettiva.

L'elasticità rappresenta uno dei vantaggi distintivi del cloud rispetto all'infrastruttura on-premises. Mentre in un data center tradizionale devi acquistare hardware per il picco massimo teorico (sprecando risorse nella maggior parte del tempo), nel cloud paghi solo ciò che consumi, quando lo consumi.

### Auto-scaling in Azure
Azure mette a disposizione diverse strategie di auto-scaling:

- **Metric-based scaling**: Le risorse si scalano in base a metriche predefinite (CPU > 70%, lunghezza coda, memoria utilizzata)
- **Time-based scaling**: Scaling programmato in base a pattern prevedibili (es. aumentare istanze alle 9:00 e ridurle alle 18:00)
- **Instance protection**: Protezione di specifiche istanze dal scale-in per garantire disponibilità

**Servizi Azure che supportano l'elasticità**:
- **Virtual Machine Scale Sets (VMSS)**: Gestiscono un gruppo di VM identiche con bilanciamento del carico
- **Azure App Service**: Piattaforma PaaS che scala automaticamente le istanze dell'applicazione
- **Azure Kubernetes Service (AKS)**: Orchestrazione di container con HPA (Horizontal Pod Autoscaler)

___

## Disponibilità
La **disponibilità** (Availability) si riferisce alla capacità di un sistema di erogare servizi in modo continuo e operativo per il tempo richiesto. Si misura tipicamente in "nove" (es. 99.9%, 99.95%, 99.999%).

### Service Level Agreement (SLA)
Azure garantisce determinati livelli di disponibilità attraverso gli **SLA** (Contratti di Livello di Servizio), che definiscono:
- La percentuale di uptime garantita
- Le conseguenze in caso di mancato rispetto (tipicamente crediti di servizio)
- Le condizioni che devono essere soddisfatte per la validità dell'SLA

**Esempi di SLA Azure**:
- Macchine Virtuali: 99.9% (con disco Premium e Availability Zone)
- Azure App Service: 99.95%
- Azure SQL Database (Business Critical): 99.995%

### Strategie per garantire la disponibilità
Azure implementa diversi meccanismi:

**Availability Zones**
Le **Zone di Disponibilità** sono location fisicamente separate all'interno di una regione Azure. Ogni zona è dotata di alimentazione, raffreddamento e networking indipendenti. Distribuire risorse su più zone (zonal redundancy) protegge da guasti a livello di data center singolo.

**Regioni Accoppiate (Paired Regions)**
Ogni regione Azure è accoppiata con un'altra regione all'interno della stessa geografia. Questa accoppiamento garantisce che gli aggiornamenti della piattaforma vengano applicati in modo scaglionato (una regione alla volta) e fornisce un'opzione per il disaster recovery geografico.

**Ridondanza dei Dati**
Azure offre diverse opzioni di replica dello storage:
- **LRS** (Locally Redundant Storage): 3 copie all'interno di un singolo data center
- **ZRS** (Zone-Redundant Storage): 3 copie distribuite su Availability Zone
- **GRS** (Geo-Redundant Storage): 6 copie totali (3 nella regione primaria, 3 nella regione secondaria accoppiata)

___

## Disaster Recovery
Il **Disaster Recovery (DR)** è l'insieme di politiche, strumenti e procedure per il ripristino o la continuità della infrastruttura tecnologica e dei sistemi a seguito di un disastro naturale o causato dall'uomo.

### Differenza tra Backup e Disaster Recovery
È fondamentale distinguere questi due concetti:
- **Backup**: Copia dei dati in un punto specifico nel tempo. Protegge dalla perdita di dati, ma non garantisce la continuità operativa immediata.
- **Disaster Recovery**: Strategia completa che include backup, ma anche procedure per riavviare l'infrastruttura, reindirizzare il traffico e ripristinare le operazioni in tempi definiti.

### Metriche fondamentali del DR
Due parametri definiscono una strategia di DR:

**RTO (Recovery Time Objective)**
Il tempo massimo tollerabile durante il quale un'applicazione può essere offline dopo un evento disastroso. Un RTO di 4 ore significa che l'azienda accetta un'interruzione massima di 4 ore.

**RPO (Recovery Point Objective)**
La quantità massima di perdita di dati tollerabile, misurata nel tempo. Un RPO di 1 ora significa che potresti perdere fino a 1 ora di dati transazionali.

### Strategie di Disaster Recovery
Azure supporta tre approcci principali:

**Cold Site**
Backup dei dati in una regione secondaria, ma nessuna infrastruttura attiva. In caso di disastro, le risorse devono essere provisioningate manualmente. Costo minimo, RTO/RPO elevati.

**Warm Site**
Infrastruttura di base pre-provisionata nella regione secondaria in modalità "standby" (tipicamente dimensioni ridotte). In caso di failover, le risorse vengono scalate orizzontalmente. Bilanciamento costo/tempo di ripristino.

**Hot Site**
Infrastruttura completamente replicata e attiva in entrambe le regioni con bilanciamento del carico geografico. Failover immediato (RTO/RPO minimi), costo massimo.

### Azure Site Recovery
**Azure Site Recovery (ASR)** è il servizio nativo Azure per il DR che permette di:
- Replicare macchine virtuali Azure tra regioni
- Replicare VM on-premises (VMware/Hyper-V) in Azure
- Orchestrare test di failover senza impatto sulla produzione
- Automatizzare il ripristino con runbook personalizzati

___

## Affidabilità e Prevedibilità
Questi due concetti sono correlati ma distinti: l'affidabilità riguarda la capacità di funzionare correttamente, la prevedibilità riguarda la capacità di anticipare comportamenti e costi.

### Affidabilità
L'**affidabilità** (Reliability) è la capacità di un sistema di recuperare da guasti e continuare a funzionare. Include aspetti come:
- **Fault tolerance**: La capacità di operare anche quando componenti falliscono
- **Resilienza**: La capacità di ripristinarsi automaticamente da errori

**Come Azure garantisce affidabilità**:
- **Service Fabric**: Piattaforma di microservizi con meccanismi automatici di failover
- **Circuit Breaker pattern**: Implementato in molti servizi PaaS per isolare guasti a cascata
- **Health probes**: Monitoraggio continuo dello stato delle risorse
- **Self-healing**: Molti servizi Azure riparano automaticamente istanze non healthy

### Prevedibilità
La **prevedibilità** si divide in due dimensioni:

**Prevedibilità delle Performance**
La capacità di garantire che un'applicazione risponda in tempi definiti indipendentemente dal carico. Azure offre:
- **Reserved Instances**: Capacità garantita per carichi di lavoro prevedibili
- **Service Tiers**: Livelli di servizio con performance definite (es. Azure SQL Database DTU/vCore model)
- **Latency-based routing**: Traffico instradato sulla regione con latenza minore

**Prevedibilità dei Costi**
La capacità di stimare e controllare la spesa cloud:
- **Pricing Calculator**: Stima dei costi prima del deployment
- **Azure Cost Management + Billing**: Monitoraggio, allocazione e ottimizzazione dei costi
- **Budgets e Alerts**: Notifiche quando si raggiungono soglie di spesa
- **Reserved Capacity**: Sconti fino al 72% per impegni di 1 o 3 anni
- **Hybrid Benefit**: Utilizzo di licenze Windows Server/SQL Server esistenti in Azure

___

## Sicurezza e Governance
La migrazione al cloud solleva questioni critiche relative alla protezione dei dati e al controllo delle risorse. Azure affronta queste tematiche attraverso un framework completo.

### Modello di Responsabilità Condivisa
In Azure, la responsabilità della sicurezza è distribuita tra Microsoft e il cliente:

**Responsabilità di Microsoft** (gestita dalla piattaforma):
- Sicurezza fisica dei data center
- Sicurezza della rete di infrastruttura
- Patch del sistema operativo host (per servizi PaaS)
- Isolamento tra tenant

**Responsabilità del Cliente** (gestita dal cliente):
- Dati e classificazione
- Endpoint e identità
- Account e accessi
- Configurazione di rete (NSG, Firewall)
- Patch del sistema operativo guest (per IaaS)

### Sicurezza Fisica e Logica
**Sicurezza Fisica**:
- Data center con accesso biometrico multi-fattore
- Sorveglianza 24/7 con guardie di sicurezza
- Isolamento fisico dei server
- Procedure di distruzione sicura dei supporti

**Sicurezza Logica**:
- **Encryption at rest**: Crittografia dei dati memorizzati (AES-256 di default)
- **Encryption in transit**: TLS 1.2+ per i dati in transito
- **Key Management**: Azure Key Vault per la gestione centralizzata di chiavi, segreti e certificati
- **Network Security Groups**: Firewall virtuali a livello di subnet/interfaccia di rete

### Governance
La **governance** cloud definisce le regole e le politiche per l'utilizzo appropriato delle risorse.

**Azure Policy**
Servizio che permette di creare, assegnare e gestire policy per l'applicazione di standard aziendali:
- Limitare le regioni dove possono essere create risorse
- Imporre l'uso di tag obbligatori
- Vietare SKU specifici (es. non permettere VM di fascia alta in ambienti di test)
- Auditing continuo della compliance

**Role-Based Access Control (RBAC)**
Sistema di autorizzazione granulare basato sui ruoli:
- **Owner**: Controllo completo inclusa la gestione degli accessi
- **Contributor**: Può creare/gestire risorse ma non modificare i permessi
- **Reader**: Accesso in sola lettura
- Ruoli personalizzabili per esigenze specifiche

**Azure Blueprints**
Permettono di definire "modelli" ripetibili per la creazione di ambienti conformi:
- Assegnazione di RBAC
- Applicazione di Azure Policy
- Deployment di risorse ARM (Azure Resource Manager)
- Definizione di parametri e vincoli

### Compliance e Certificazioni
Azure mantiene il portfolio di compliance più ampio del settore:
- **Globali**: ISO 27001, ISO 27018, ISO 22301, SOC 1/2/3
- **Europee**: GDPR, EU Model Clauses
- **Settoriali**: HIPAA (sanità), PCI DSS (pagamenti), FedRAMP (governo USA)
- **Nazionali**: ITAR, NIST, CSA STAR

Il **Trust Center** e il **Service Trust Portal** forniscono documentazione dettagliata, audit report e strumenti di valutazione della compliance per ogni servizio Azure.

___

## Conclusione
In questa seconda parte abbiamo esplorato i concetti fondamentali che caratterizzano il cloud computing moderno e che costituiscono la base per l'architettura di soluzioni su Azure:

- La **scalabilità** (verticale e orizzontale) permette di adattare le risorse al carico
- L'**elasticità** automatizza questo adattamento in tempo reale
- La **disponibilità** garantisce l'erogazione continua del servizio attraverso ridondanza e SLA
- Il **disaster recovery** protegge dalla perdita di dati e dalla interruzione prolungata dei servizi
- L'**affidabilità e la prevedibilità** assicurano performance costanti e costi controllati
- La **sicurezza e la governance** forniscono il framework per proteggere asset e garantire la compliance

___
Prossima parte [[Modelli di deployment]]
