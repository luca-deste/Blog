Dove risiede fisicamente il cloud? La risposta dipende dal modello di deployment scelto. Esistono tre modelli principali: Public Cloud, Private Cloud e Hybrid Cloud. Ognuno ha caratteristiche, vantaggi e casi d'uso specifici.

## Public Cloud

Il Public Cloud è il modello più comune. L'infrastruttura fisica (server, storage, rete) è di proprietà del provider cloud (Azure, AWS, Google Cloud) e viene condivisa tra più clienti. L'accesso avviene via Internet.

**Caratteristiche principali:**

-   **Multi-tenant**: Più organizzazioni utilizzano la stessa infrastruttura fisica, mantenendo isolamento logico dei dati
    
-   **Nessun investimento hardware**: Non si acquista nulla, si pagano solo le risorse consumate
    
-   **Scalabilità immediata**: È possibile aumentare o diminuire le risorse in pochi minuti
    
-   **Manutenzione a carico del provider**: Aggiornamenti, patch di sicurezza e gestione dell'hardware sono gestiti dal provider
    

**Casi d'uso tipici:**

-   Startup in fase di crescita rapida
    
-   Ambienti di sviluppo e testing
    
-   Applicazioni con traffico imprevedibile o stagionale
    
-   Siti web e applicazioni consumer
    

**Esempio pratico**: Un e-commerce che si prepara per il Black Friday può raddoppiare la capacità dei server per pochi giorni, pagando solo per quel periodo, senza dover acquistare hardware che rimarrebbe inutilizzato il resto dell'anno.

## Private Cloud

Nel Private Cloud, l'infrastruttura è dedicata esclusivamente a una singola organizzazione. Può essere ospitata on-premises (nel data center aziendale) o gestita da un provider terzo, ma le risorse non sono mai condivise con altri clienti.

**Caratteristiche principali:**

-   **Single-tenant**: L'hardware è esclusivo
    
-   **Controllo totale**: Si decide la configurazione esatta di hardware, rete e sicurezza
    
-   **Compliance semplificata**: Più facile dimostrare il rispetto di normative rigide sui dati
    
-   **Prestazioni prevedibili**: Nessuna competizione per le risorse con altri tenant
    

**Casi d'uso tipici:**

-   Enti governativi con dati classificati
    
-   Ospedali e strutture sanitarie (dati sanitari sensibili)
    
-   Banche e istituzioni finanziarie con requisiti normativi stringenti
    
-   Aziende con workload legacy che richiedono configurazioni specifiche
    

**Svantaggi:**

-   Costi iniziali elevati (acquisto hardware, costruzione data center)
    
-   Costi operativi fissi indipendentemente dall'utilizzo
    
-   Tempi di provisioning lunghi (ordine, installazione, configurazione hardware)
    
-   Necessità di personale specializzato per la gestione
    

## Hybrid Cloud

L'Hybrid Cloud combina Public Cloud e Private Cloud (o on-premises), permettendo il flusso di dati e applicazioni tra i due ambienti. Non è un compromesso, ma una scelta strategica per ottimizzare i workload.

**Caratteristiche principali:**

-   **Flessibilità**: Si possono posizionare i workload nell'ambiente più adatto
    
-   **Cloud bursting**: Quando il carico supera la capacità del Private Cloud, l'eccesso viene gestito dal Public Cloud
    
-   **Bilanciamento costi/sicurezza**: Dati sensibili nel Private, workload variabili nel Public
    
-   **Integrazione**: I due ambienti sono connessi e comunicano tra loro
    

**Perché una banca sceglie l'Hybrid Cloud?**

Una banca ha esigenze contrastanti che nessun singolo modello può soddisfare completamente:

1.  **Dati sensibili nel Private**: I dati dei conti correnti, le transazioni finanziarie e le informazioni personali dei clienti devono rimanere in un ambiente controllato per rispettare normative come PCI DSS (pagamenti) e GDPR (privacy). Questi dati risiedono nel data center privato della banca, dietro firewall dedicati.
    
2.  **Scalabilità nel Public**: L'applicazione mobile che i clienti usano per controllare il saldo deve sopportare picchi enormi (es. il giorno di paga o durante eventi speciali). Gestire questi picchi con hardware proprio sarebbe economicamente inefficiente. Nel Public Cloud, la banca scala automaticamente durante i picchi e riduce durante i periodi calmi.
    
3.  **Disaster Recovery**: I backup critici possono essere replicati nel Public Cloud, garantendo continuità anche in caso di guasto totale del data center principale.
    

**Esempio pratico**: Durante il Black Friday, una banca lancia una promozione sui prestiti. Il sito pubblico e il modulo di richiesta (nel Public Cloud) ricevono 10 volte il traffico normale, mentre il sistema che valuta il merito creditizio (nel Private Cloud) processa i dati sensibili rimanendo nel perimetro sicuro. I due sistemi comunicano tramite API sicure, ma i dati sensibili non lasciano mai il Private Cloud.

## Community Cloud

Esiste anche una variante meno comune: il Community Cloud. L'infrastruttura è condivisa tra diverse organizzazioni che hanno esigenze comuni (stesso settore, stesse normative). Un esempio è un cloud dedicato a università o a enti governativi di una specifica regione.

Offre un equilibrio tra costo (condiviso) e controllo (dedicato a una community specifica), ma è meno rilevante per l'AZ-900 rispetto ai tre modelli principali.

## Sintesi

Table

Copy

| Modello | Controllo | Costo | Scalabilità | Sicurezza |
| :--- | :--- | :--- | :--- | :--- |
| Public | Limitato | Basso (pay-as-you-go) | Altissima | Standard |
| Private | Totale | Alto (costi fissi) | Limitata | Massima |
| Hybrid | Bilanciato | Variabile | Alta | Configurabile |

La scelta del modello dipende dalle esigenze specifiche: non esiste una risposta giusta universale, ma una risposta giusta per il proprio contesto.
