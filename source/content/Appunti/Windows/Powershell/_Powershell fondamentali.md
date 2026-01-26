I cmdlet seguono la convenzione `Verbo-Nome`. Il verbo definisce l'azione, il nome l'oggetto su cui agisce.

**Comandi di sistema e navigazione:**

-   `Get-Command`: Elenca tutti i cmdlet, funzioni, alias e script disponibili nella sessione corrente.
-   `Get-Help`: Fornisce documentazione dettagliata, parametri ed esempi d'uso per ogni cmdlet.
-   `Get-Alias`: Mostra le scorciatoie per i cmdlet (es. `dir` per `Get-ChildItem`, `cd` per `Set-Location`).
-   `Set-Location`: Cambia la directory di lavoro corrente.
-   `Get-ComputerInfo`: Recupera informazioni complete sul sistema.

**Gestione Moduli e File:**

-   `Find-Module`: Ricerca moduli nelle repository online (es. PowerShell Gallery). Supporta wildcards: `Find-Module -Name "Pattern*"`.
-   `Install-Module`: Installa i moduli trovati sul sistema.
-   `New-Item`: Crea un nuovo oggetto (file o directory). Richiede di specificare `-Path` e `-ItemType`.
-   `Get-Content`: Legge e visualizza il contenuto di un file (equivalente a `cat` o `type`).
-   `Get-FileHash`: Genera l'hash di un file per verificarne l'integrità.

**Manipolazione e Filtro Dati:**

-   `Where-Object`: Filtra gli oggetti in base a condizioni specifiche.
    -   Esempio: `Get-ChildItem | Where-Object -Property "Extension" -eq ".txt"`
-   `Select-Object`: Seleziona proprietà specifiche di un oggetto o limita il numero di risultati.
-   `Select-String`: Ricerca pattern di testo all'interno dei file (equivalente a `grep`).

**Monitoraggio e Troubleshooting:**

-   `Get-Process`: Mostra i processi in esecuzione, inclusi consumi CPU e memoria.
-   `Get-Service`: Recupera lo stato dei servizi di sistema.
-   `Get-NetTCPConnection`: Monitora le connessioni di rete attive.
-   `Invoke-Command`: Esegue comandi su sistemi remoti (fondamentale per l'amministrazione remota).

**Utenti e Networking:**

-   `Get-LocalUser`: Elenca gli account utente locali.
-   `Get-NetIPConfiguration`: Fornisce i dettagli delle interfacce di rete attive.
-   `Get-NetIPAddress`: Mostra tutti i dettagli degli indirizzi IP configurati (anche quelli non attivi).