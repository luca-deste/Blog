# Azure Architecture: Logical Governance & Physical Infrastructure

## 1. Gerarchia di Gestione (Logical Layer)

### 1.1 Management Groups

**Definizione**: Contenitori di livello superiore che aggregano sottoscrizioni per applicare governance centralizzata.

**Specifiche Tecniche**:

-   **Profondità massima**: 6 livelli (escluso Root e Subscription)
    
-   **Ereditarietà**: Policy Azure e assegnazioni RBAC si propagano in modo coerente a tutti i discendenti
    
-   **Root Management Group**: Creato automaticamente all'onboarding del tenant; ID identico all'ID Tenant Azure AD
    
-   **Limite**: 10,000 Management Groups per tenant
    

**Caso d'Uso**: Applicazione di policy di sicurezza aziendale (es. "Consentire solo SKU VM approvati") su tutte le sottoscrizioni di un dipartimento senza configurazione individuale.

***

### 1.2 Subscriptions

**Definizione**: Limite amministrativo e di fatturazione per tutte le risorse Azure.

**Specifiche Tecniche**:

-   **Quota**: Limiti predefiniti per tipo di risorsa (es. 2500 vCPU per regione per subscription)
    
-   **Billing Boundary**: Tutti i costi aggregati a livello di subscription; supporta EA, CSP, Pay-As-You-Go
    
-   **Trust Boundary**: Ogni subscription è associata a un singolo Azure AD Tenant
    
-   **Limite**: 2000 subscription per tenant
    

**Caso d'Uso**: Isolamento dei costi per business unit o ambienti (Produzione vs Sviluppo) con budget e controlli di accesso distinti.

***

### 1.3 Resource Groups

**Definizione**: Contenitore logico che raggruppa risorse con ciclo di vita comune.

**Specifiche Tecniche**:

-   **Unicità**: Una risorsa appartiene a uno e un solo Resource Group
    
-   **Metadata Location**: Ogni RG ha una regione di metadati (dove risiedono i metadati del gruppo), indipendente dalla location delle risorse contenute
    
-   **Atomicità**: Eliminazione del RG = eliminazione di tutte le risorse contenute (operazione irreversibile)
    
-   **Limite**: 980 Resource Groups per subscription
    

> **EXAM TIP** ⚠️ **Domanda**: Un Resource Group può contenere risorse distribuite in regioni diverse? **Risposta**: **Sì**. È tecnicamente possibile avere una VM in West Europe e un Storage Account in East US nello stesso RG. Tuttavia, è una pratica sconsigliata per la gestione operativa e la conformità. L'esame testa questa distinzione tra possibilità tecnica e best practice.

***

## 2. Infrastruttura Fisica (Physical Layer)

### 2.1 Regions

**Definizione**: Area geografica contenente uno o più data center connessi con rete a bassa latenza.

**Specifiche Tecniche**:

-   **Isolamento**: Ogni regione è indipendente per fault tolerance
    
-   **Sovranità**: Dati residenti nella geografia specificata (es. "Germany West Central" per dati in Germania)
    
-   **Disponibilità servizi**: Non tutti i servizi sono disponibili in tutte le regioni (consultare Products Available by Region)
    
-   **Identificatore**: Nome visualizzato ("West Europe") vs Nome API ("westeurope")
    

***

### 2.2 Availability Zones

**Definizione**: Data center fisicamente separati all'interno di una regione, con infrastruttura indipendente (power, cooling, networking).

**Specifiche Tecniche**:

-   **Quantità**: Minimo 3 zone per regione abilitata
    
-   **Distanza**: Decine di chilometri (minimo 10 km) tra zone
    
-   **Connettività**: Rete privata fibra ottica con latenza < 2ms
    
-   **SLA**:
    
    -   VM in singola zona: 99.9%
        
    -   VM distribuite su 2+ zone con Load Balancer Standard: 99.99%
        

**Caso d'Uso**: Alta disponibilità per applicazioni mission-critical tramite distribuzione active-active su multiple zone.

***

### 2.3 Region Pairs

**Definizione**: Coppia fissa di regioni nella stessa area geografica con distanza minima di 300 miglia (480 km).

**Specifiche Tecniche**:

-   **Replica piattaforma**: Automatica per servizi di controllo Azure (non per dati utente)
    
-   **Sequenzialità aggiornamenti**: Gli update della piattaforma vengono applicati a una regione alla volta
    
-   **Priorità failover**: In caso di interruzione su vasta area, Azure priorizza il ripristino della regione accoppiata
    
-   **Immutabilità**: Le coppie sono definite da Microsoft e non possono essere modificate
    

> **EXAM TIP** ⚠️ **Domanda**: Chi controlla la sequenza degli aggiornamenti nelle Region Pairs? **Risposta**: **Microsoft**. Gli aggiornamenti della piattaforma vengono applicati sequenzialmente (non simultaneamente) alle regioni accoppiate per garantire che, in caso di problema con l'update, almeno una regione rimanga operativa. L'utente non ha controllo su questa sequenza.

***

## 3. Risorse: Integrazione Logico-Fisica

### 3.1 Caratteristiche di una Risorsa

Ogni risorsa Azure è definita da:

-   **Resource ID**: Percorso univoco globale `/subscriptions/{id}/resourceGroups/{name}/providers/{provider}/{type}/{name}`
    
-   **Resource Type**: Namespace del provider (es. `Microsoft.Compute/virtualMachines`)
    
-   **Location**: Regione fisica di deploy (obbligatorio, immutabile dopo creazione per la maggior parte delle risorse)
    
-   **Tags**: Metadati chiave-valore (max 50 tag, chiave max 512 chars, valore max 256 chars)
    

### 3.2 Modello di Ereditarietà vs Località

-   **Ereditarietà Governance**: Top-down (Management Group → Subscription → Resource Group → Resource)
    
    -   Policy, RBAC, Budgets fluiscono gerarchicamente
        
-   **Località Fisica**: Determinata al momento del deploy della risorsa
    
    -   Una risorsa esiste in una specifica Region e, opzionalmente, in una specifica Availability Zone
        

### 3.3 Esempio di Interazione

Quando si crea una VM:

1.  **Controllo**: Azure AD autentica l'utente (Tenant)
    
2.  **Autorizzazione**: RBAC verifica i permessi sulla Subscription/Resource Group
    
3.  **Deploy**: La VM viene fisicamente allocata in un cluster di data center nella Region specificata (es. West Europe)
    
4.  **Resilienza**: Se specificato, la VM viene vincolata a Availability Zone 1, 2 o 3