## Note:
Questo codice permette di verificare se uno o più utenti sono presenti in un gruppo AD che matcha un nome.
Può essere utilizzabile anche per matchare tutti i gruppi che contengono determinate parole per un utente.
> ES: Verificare se un utente è presente in gruppi che contengono la parola "Server"

## Uso:
- Inserire il nome del/degli utenti al posto di {UTENTE1} e {UTENTE2}.
- Inserire il nome del gruppo (o la parola interessata) al posto di {NOMEGRUPPO}


## Codice:
```
$utenti = @("{UTENTE1}", "{UTENTE2}") # Lista degli username
$word = "{NOME GRUPPO}" # Aggiunti asterischi per la ricerca parziale

foreach ($utente in $utenti) {
    try {
        # Recupera l'utente e i suoi gruppi
        $adUser = Get-ADUser -Identity $utente -Properties MemberOf
        
        # Filtra i gruppi che corrispondono alla parola chiave
        $gruppiFiltrati = $adUser.MemberOf | Where-Object { 
            # Estrae il CN e verifica se contiene la parola
            ($_ -split ',')[0] -replace '^CN=' -like $word 
        }

        if ($gruppiFiltrati) {
            Write-Host "`n[$utente] è membro dei seguenti gruppi che contengono '$word':" -ForegroundColor Green
            foreach ($g in $gruppiFiltrati) {
                # Mostra solo il nome pulito del gruppo invece del DN completo
                Write-Host " - (($g -split ',')[0] -replace '^CN=')"
            }
        } else {
            Write-Host "`n[$utente] NON è membro di alcun gruppo che contiene '$word'." -ForegroundColor Yellow
        }
    } catch {
        Write-Host "`n[$utente] Errore: Utente non trovato o errore di accesso ad AD." -ForegroundColor Red
    }
}

```