## Regioni e Zone di Disponibilità

### Regioni

Una **Regione Azure** è una località geografica che contiene uno o più data center equipaggiati con energia, rete e raffreddamento indipendenti. Quando create una risorsa in Azure, dovete selezionare una regione. Questa scelta determina dove fisicamente risiederanno i vostri dati.
Una regione è come una città. All'interno di quella città esistono diversi edifici (data center) che ospitano le vostre risorse. Se i vostri utenti sono in Italia, sceglierete una regione europea per ridurre la latenza.

Ogni regione ha un nome (es. `West Europe`, `East US`, `Southeast Asia`) e offre un set specifico di servizi. Non tutti i servizi Azure sono disponibili in tutte le regioni.

**Dettaglio tecnico**:

-   Ogni regione è isolata dalle altre per resistere a guasti regionali
    
-   La scelta della regione influisce su latenza, costi e conformità normativa
    
-   Alcuni servizi sono globali (Azure AD), altri sono regionali (VM, Storage)
    

***

### Region Pairs

Ogni regione Azure è abbinata a un'altra regione all'interno della stessa area geografica, formando una **Region Pair**. Le due regioni sono separate da almeno 300 miglia (circa 480 km) per garantire che un disastro naturale non le colpisca entrambe.
**Esempio**: Microsoft rilascia aggiornamenti software su larga scala. Quando aggiorna la regione primaria, la regione secondaria rimane operativa. Se qualcosa va storto durante l'aggiornamento, il traffico viene reindirizzato alla regione abbinata. Non aggiornano mai entrambe le regioni contemporaneamente.

Esempi di Region Pairs:

-   **West Europe** (Paesi Bassi) è abbinata a **North Europe** (Irlanda)
    
-   **East US** (Virginia) è abbinata a **West US** (California)
    

**Dettaglio tecnico**:

-   La replica tra regioni abbinate è compresa in alcuni servizi (es. Geo-Redundant Storage)
    
-   In caso di interruzione di vasta portata, una regione della coppia viene ripristinata per prima
    
-   I dati rimangono all'interno della stessa giurisdizione geografica (es. Europa, America)
    

***

### Availability Zones

Le **Availability Zones** sono data center fisicamente separati all'interno della stessa regione. Ogni zona ha alimentazione, raffreddamento e rete indipendenti. Una regione può contenere tre o più zone.
Immaginate tre edifici nello stesso quartiere. Ogni edificio ha la propria centrale elettrica, il proprio sistema antincendio e la propria connessione internet. Se un edificio va a fuoco o perde energia, gli altri due continuano a funzionare. La distanza tra gli edifici è sufficiente a isolare i guasti locali, ma sufficientemente breve per mantenere latenze ridotte.

Quando distribuite una macchina virtuale in un'Availability Zone, scegliete esplicitamente in quale zona collocarla (Zona 1, 2 o 3). Per alta disponibilità, distribuite le istanze su zone diverse.

**Dettaglio tecnico**:

-   Le zone sono identificate da numeri (1, 2, 3) all'interno di una regione
    
-   Non tutte le regioni supportano le Availability Zones
    
-   Lo storage zone-redundant (ZRS) replica i dati automaticamente tra tre zone
    
-   Il failover tra zone è manuale per alcuni servizi, automatico per altri
    

***

## Risorse e Gerarchia

### Risorse

Una **Risorsa** è qualsiasi elemento gestibile disponibile in Azure. Ogni risorsa è un'istanza di un servizio che avete creato e configurato.

Esempi di risorse:

-   Macchina virtuale
    
-   Database SQL
    
-   Account di storage
    
-   Rete virtuale
    
-   App Service
    

Ogni risorsa ha:

-   Un nome univoco nel suo scope
    
-   Un tipo che ne definisce la natura
    
-   Una regione dove fisicamente risiede
    
-   Un Resource Group di appartenenza
    

***

### Resource Groups

Un **Resource Group** è un contenitore logico che raggruppa risorse correlate che condividono lo stesso ciclo di vita. Le risorse all'interno di un gruppo possono essere distribuite, aggiornate ed eliminate insieme.

**Analogia**: Un Resource Group è come una cartella nel vostro computer. All'interno della cartella "Progetto Sito Web" mettete tutti i file correlati: immagini, documenti, codice. Quando il progetto finisce, eliminate l'intera cartella e tutto il suo contenuto sparisce. Allo stesso modo, quando eliminate un Resource Group, tutte le risorse al suo interno vengono eliminate.

Strategie comuni di organizzazione:

-   Per applicazione (tutte le risorse di una singola app)
    
-   Per ambiente (sviluppo, test, produzione)
    
-   Per dipartimento (Marketing, Vendite, IT)
    

**Dettaglio tecnico**:

-   Ogni risorsa può appartenere a un solo Resource Group
    
-   I Resource Group non possono essere annidati
    
-   I metadati del Resource Group risiedono in una regione specifica (scelta alla creazione)
    
-   Le risorse all'interno di un gruppo possono essere distribuite in regioni diverse
    

***

### Subscription

Una **Subscription** è un contenitore logico che rappresenta un confine di fatturazione e sicurezza. Ogni risorsa Azure deve appartenere a una subscription. La fatturazione è aggregata a livello di subscription.

**A cosa serve**:

-   **Fatturazione separata**: Ogni subscription genera una fattura indipendente
    
-   **Limiti di servizio**: Quote massime per numero di risorse (es. 980 VM per subscription)
    
-   **Isolamento di sicurezza**: Gli amministratori di una subscription non hanno accesso alle altre
    
-   **Ambienti distinti**: Subscription separate per sviluppo, test e produzione
    

**Dettaglio tecnico**:

-   Un account Azure può avere multiple subscription
    
-   Ogni subscription è associata a un tenant Azure AD
    
-   I costi sono visualizzabili e analizzabili per singola subscription
    
-   Le subscription possono essere trasferite tra tenant
    

***

### Management Groups

I **Management Groups** sono contenitori di livello superiore che raggruppano una o più subscription. Permettono di applicare policy di governance e controlli di accesso a tutte le subscription contenute in modo centralizzato.

**Scopo**: Quando avete centinaia di subscription, configurare ognuna individualmente è impraticabile. I Management Groups vi permettono di applicare regole una sola volta a livello di gruppo.

Casi d'uso:

-   Applicare Azure Policy a tutte le subscription di un dipartimento
    
-   Gestire il Role-Based Access Control (RBAC) centralizzato
    
-   Organizzare le subscription per regione geografica o business unit
    

**Dettaglio tecnico**:

-   Supportano una gerarchia annidata fino a 6 livelli di profondità
    
-   Una subscription può appartenere a un solo Management Group
    
-   Le policy applicate a un Management Group vengono ereditate da tutti i livelli inferiori
    
-   Un tenant Azure AD supporta fino a 10.000 Management Groups
    

***

## Sintesi

La **struttura fisica** (Regioni, Zone) determina dove risiedono i dati e garantisce resilienza contro guasti. La **struttura logica** (Management Groups, Subscription, Resource Groups) determina chi paga, chi ha accesso e come si organizzano le risorse. Fisico e logico sono indipendenti: una VM può risiedere fisicamente in `West Europe` ma appartenere logicamente a un Resource Group di produzione gestito da un Management Group globale.