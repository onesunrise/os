---
title: "Restaurer une mailbox"
date: 2023-06-02T13:10:12+02:00
# à tester :
tags: ["powershell", "o365"]
---
Restaurer une mailbox o365 (sous un autre nom) et l'affecter à un utilisateur.  

Prérequis :   

Compte Admin office 365 avec Role Exchange Online Administrator  
la cmdlet ExchangeOnlineManagement installée sur votre machine  

Lancer en powershell admin :  

```
Install-Module PowershellGet -Force  
Update-Module PowershellGet  
Install-Module -Name ExchangeOnlineManagement  
```

Connexion à ExchangeOnlineManagement  
En powershell admin :   

Connect-ExchangeOnline -UserPrincipalName adm365-xxxxxxx@*.onmicrosoft.com  

Identifier le GUID de la mailbox supprimée 
Si vous avez l'ID :  

get-mailbox -SoftDeletedMailbox | where-Object {_.Alias -eq "ID"}  
et pour avoir le GUID  

Get-Mailbox -SoftDeletedMailbox |  Where-Object {$_.Alias -eq "Matricule" } | select ExchangeGuid  

Identifier le GUID de la mailbox de destination  
Même action pour la destination, ici pas d'attribut SoftDeletedMailbox  car la destination doit être disponible.  
Au besoin, donc, Création shared mailbox pour accueillir les datas  
Ca se fait sur o365, pas besoin de licence  

Recupérer le guid de cette shared mailbox  
Get-Mailbox  -identity "MATRICULE ou adresse mailbox" | select ExchangeGuid  

Lancer la restauration et la suivre  
Syntaxe:  
New-MailboxRestoreRequest -SourceMailbox <Source ExchangeGUID> -TargetMailbox <destination mailbox ExchangeGUID> -AllowLegacyDNMismatch  

Le statut Queued indique que ce n'est pas immédiat  
Pour suivre la progression  

version courte   
get-mailboxrestoreRequest  

Version détaillée  
get-mailboxrestoreRequest | select *  

Si on se trompe et qu on restaure sur la mailbox principale, utiliser les commandes  
Suspend-MailboxRestoreRequest -identity RequestGuid   
Et  
Remove-MailboxRestoreRequest -identity RequestGuid
