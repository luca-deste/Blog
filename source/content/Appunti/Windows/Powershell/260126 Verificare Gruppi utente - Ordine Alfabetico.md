## Note:
Questo è il codice per verificare i gruppi di un utente con powershell, mettendoli in ordine alfabetico:

## Uso:
Sostituire {USERNAME} con il nome dell'utente di dominio.

## Codice:
```
#Get-ADUser -Identity "{USERNAME}" -Properties MemberOf | Select-Object -ExpandProperty MemberOf | ForEach-Object { ($_ -split ',')[0] -replace '^CN=' } | Sort-Object
```