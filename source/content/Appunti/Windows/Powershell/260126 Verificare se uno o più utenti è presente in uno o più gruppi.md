## Note:
Questo è il codice per ottenere la lista dei domain controller di una foresta AD:

## Uso:
Non sono necessarie modifiche al codice

## Codice:
```
#(Get-ADForest).Domains | %{ Get-ADDomainController -Filter * -Server $_ }| Format-Table -Property Name,ComputerObjectDN,Domain,Forest,IPv4Address,OperatingSystem,OperatingSystemVersio
```