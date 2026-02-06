## Networking in Azure

### Virtual Networks (VNet)

Una **Virtual Network (VNet)** è una rappresentazione logica della vostra rete in Azure. Definisce un confine di sicurezza dove le risorse possono comunicare tra loro.

**Analogia**: Una VNet è come una città. All'interno di questa città ci sono strade (connessioni di rete) che collegano edifici (risorse). La città è isolata dal resto del mondo a meno che non costruiate ponti o tunnel per collegarla altrove.

**Dettaglio tecnico**:

-   Ogni VNet ha uno spazio di indirizzi IP privato (es. 10.0.0.0/16)
    
-   Le risorse nella stessa VNet comunicano direttamente senza bisogno di routing aggiuntivo
    
-   Le VNet sono isolate per default: una VM in VNet-A non vede una VM in VNet-B
    

***

### Subnet

Le **Subnet** sono suddivisioni di una VNet. Segmentano lo spazio di indirizzi IP in blocchi più piccoli.

**Analogia**: Se la VNet è una città, le subnet sono i quartieri. Il quartiere residenziale ha le sue strade e regole, il quartiere industriale ha le sue. Ogni quartiere ha un range di indirizzi civici distinto.

**Esempio pratico**:

-   VNet: 10.0.0.0/16 (65.534 indirizzi)
    
-   Subnet Web: 10.0.1.0/24 (254 indirizzi per i server web)
    
-   Subnet Database: 10.0.2.0/24 (254 indirizzi per i database)
    

**Dettaglio tecnico**:

-   Ogni subnet appartiene a una sola VNet
    
-   Le subnet possono avere Network Security Group (NSG) diverse per controllare il traffico
    
-   Il routing tra subnet nella stessa VNet è automatico
    

***

### VNet Peering

Il **VNet Peering** collega due VNet in modo che le risorse possano comunicare usando i loro indirizzi IP privati.

**Analogia**: Avete due uffici in città diverse. Normalmente per andare dall'uno all'altro dovete uscire, prendere l'autostrada pubblica (Internet), e rientrare. Il Peering è come tirare un cavo diretto tra i due uffici: il traffico passa privatamente, senza uscire su strade pubbliche.

**Dettaglio tecnico**:

-   Il traffico tra VNet peerate rimane sulla rete backbone di Azure (non esce su Internet)
    
-   Bassa latenza e alta larghezza di banda
    
-   Supporta peering tra regioni diverse (Global VNet Peering)
    
-   Le VNet peerate devono avere spazi di indirizzi non sovrapposti
    

***

### DNS in Azure

**DNS (Domain Name System)** traduce nomi di dominio leggibili (es. `server01.contoso.local`) in indirizzi IP numerici (es. `10.0.1.5`).

**Analogia**: Il DNS è come una rubrica telefonica. Invece di memorizzare numeri (IP), memorizzate nomi (domini). Quando volete chiamare "Mario", la rubrica trova il numero. Il DNS fa lo stesso: quando una VM cerca `database.contoso.local`, il DNS risponde con l'IP corrispondente.

**Opzioni DNS in Azure**:

-   **Azure DNS**: Servizio di hosting DNS pubblico per domini Internet
    
-   **Azure Private DNS**: Risoluzione nomi all'interno delle VNet (zone private)
    
-   **DNS personalizzato**: Usare i propri server DNS (es. Active Directory Domain Services)
    

***

### Connettersi al Cloud

#### VPN Gateway

Una **VPN Gateway** crea una connessione criptata tra la vostra rete on-premise e Azure attraverso Internet pubblico.

**Analogia**: Immaginate di dover inviare documenti riservati da un ufficio all'altro. Invece di spedirli per posta normale (Internet aperto), usate un corriere blindato che percorre le strade pubbliche ma il carico è protetto. La VPN Gateway è quel tunnel criptato che attraversa Internet ma mantiene i dati al sicuro.

**Dettaglio tecnico**:

-   Protocolli supportati: IPsec/IKE
    
-   Throughput: da 100 Mbps a 10 Gbps (in base allo SKU)
    
-   Tipi: Site-to-Site (rete a rete), Point-to-Site (client a rete), VNet-to-VNet
    
-   Il traffico passa su Internet, quindi latenza e larghezza di banda dipendono dalla connessione Internet
    

***

#### ExpressRoute

**ExpressRoute** è una connessione privata e dedicata tra la vostra rete on-premise e Azure. Non passa mai per Internet pubblico.

**Analogia**: Tirate un cavo in fibra ottica diretto dal vostro data center aziendale a un data center Microsoft. Questo cavo è vostro esclusivamente: nessun altro traffico, nessuna strada pubblica. Velocità garantita, latenza prevedibile, massima sicurezza.

**Dettaglio tecnico**:

-   Connessione tramite provider di telecomunicazioni partner
    
-   Velocità: da 50 Mbps a 100 Gbps
    
-   Latenza minore e più stabile rispetto alla VPN
    
-   Supporta peering privato (risorse Azure) e peering Microsoft (servizi SaaS come Office 365)
    
-   Costo significativamente superiore alla VPN
    

**Confronto VPN vs ExpressRoute**:

Table

Copy

| Caratteristica | VPN Gateway | ExpressRoute |
| :--- | :--- | :--- |
| Percorso | Internet pubblico | Connessione privata dedicata |
| Velocità | Fino a 10 Gbps | Fino a 100 Gbps |
| Latenza | Variabile | Bassa e stabile |
| Costo | Basso | Alto |
| Tempo di attivazione | Ore | Settimane/Mesi |
| Caso d'uso | Piccole/medie aziende, backup | Enterprise, carichi critici |

***

### Endpoint

#### Endpoint Pubblico

Un **Endpoint Pubblico** è un indirizzo IP pubblico e una porta esposti su Internet. Chiunque su Internet può tentare di raggiungerlo.

**Analogia**: L'indirizzo pubblico di un negozio in centro città. Chiunque può entrare dalla porta principale. Dovete mettere guardie (firewall) e sistemi di sicurezza per controllare chi entra.

**Dettaglio tecnico**:

-   Indirizzo IP pubblico statico o dinamico
    
-   Esposto su Internet
    
-   Richiede controlli di sicurezza rigorosi (NSG, Firewall)
    
-   Usato per: siti web pubblici, API esterne, bastion host
    

***

#### Endpoint Privato

Un **Endpoint Privato** assegna un indirizzo IP privato da una VNet a un servizio Azure. Il servizio è accessibile solo dalla VNet, non da Internet.

**Analogia**: Una porta sul retro del negozio accessibile solo dal cortile interno. Nessuno dalla strada pubblica può arrivarci. Solo chi è già dentro l'edificio (la VNet) può usare quella porta.

**Dettaglio tecnico**:

-   Indirizzo IP privato dalla VNet (es. 10.0.x.x)
    
-   Il traffico rimane sulla rete backbone di Azure
    
-   Nessun accesso da Internet pubblico
    
-   Supportato da: Storage Account, SQL Database, Cosmos DB, e molti altri servizi PaaS
    

**Confronto Endpoint Pubblico vs Privato**:

Table

Copy

| Caratteristica | Endpoint Pubblico | Endpoint Privato |
| :--- | :--- | :--- |
| Accesso | Da Internet | Solo dalla VNet |
| Indirizzo IP | Pubblico | Privato (RFC 1918) |
| Sicurezza | Richiede protezioni esterne | Isolato per design |
| Caso d'uso | Servizi pubblici, API esterne | Dati sensibili, compliance |