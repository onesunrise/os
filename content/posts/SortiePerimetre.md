---
title: "Sortie de périmètre d'un serveur"
date: 2023-05-23
# à tester :
tags: ["tech","AD","Powershell"]
---
- Arrêt de la VM  

- Suppression de la VM  

- Suppression AD group et computer  

Chercher ds l AD et trouver le groupe en question :  
get-adgroup -filter {name -like "*Name_Server*"}

Puis le supprimer :  
get-adgroup -filter {name -like "* Name_Server *"} | Remove-ADGroup  

Pour trouver le serveur en question :  
Get-ADComputer -filter {name -like "* Name_Server *"}

Puis le supprimer :  
Get-ADComputer -filter {name -like "* Name_Server *"} | Remove-ADComputer  

- Suppression du DNS  

- Suppression du WSUS