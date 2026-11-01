# BLOC  2 - Administration des systèmes - Situation 1 : Préparation de la maquette et premiers paramétrages du serveur Windows 2025

**Compte rendu rédigé par** : BIDANESSY Coumba
**Formation** : BTS SIO 2ème année - Option SISR  
**Établissement** : Lycée Paul-Louis Courier, Tours  

[toc]

# Listes des commandes qui m'ont aider pour l'installation et le paramétrage d'un serveur Windows 2025 (Machine virtuelle)

## Installation de drivers pour la détection de la carte réseau du PC 

**Get-volume** : vérification des lecteurs de la VM afin d’identifier le lecteur comprenant les VirtIO
**Start-Process** : Installation de VirtIO Guest Tools.
**Set-Service -Name "QEMU-GA" -StartupType Automatic** : Activation du démarrage automatique de l’agent.
**Start-Service QEMU-GA** : Démarrage l’agent
**pnputil** : Installation manuelle du driverVirtIO pour la détection de la carte réseau


## Paramétrages IP

**Get-NetAdapter** : voir les adaptateurs réseaux 

**Disable-NetAdapter -Name "nom du LAN"** : désactiver interface réseau 
    
**Enable-NetAdapter -Name "nom du LAN"** : activer interface réseau
    
**New-NetIPAddress -InterfaceAlias LAN -IPAddress 192.168.14.50 -PrefixLength 24 -DefaultGateway 192.168.14.2 -AddressFamily IPv4** : ajouter une configuration ipv4 
    
**Remove-NetIPAddress -InterfaceAlias LAN -IPAddress 192.168.14.50 New-NetIPAddress -InterfaceAlias LAN -IPAddress 192.168.14.51** : Supprimer et ajouter une nouvelle adresse ipv4

**Get-DnsClientServerAddress** : obtenir la liste des serveurs DNS configurés sur chaque interface réseau de la machine 
    
**Set-DnsClientServerAddress** :  définir une serveur dns sur une interface réseau 

## Renommer un compte utilisateurs / administrateur
    
Rename-LocalUser -Name "nom-actuel" -NewName "nvx-nom" 

## Mise en place des recommandations par l'ANSSI
 
### Synchronisation horaire (Network Time Protocol)

**w32tm /query /configuration** : visualisation de la configuration actuelle du serveur de temps 

**w32tm /config /manualpeerlist:"<serveur NTP>" /syncfromflags:manual /update** : changement de serveur NTP 
> Dans ma situation : **w32tm /config /manualpeerlist:"`0.fr.pool.ntp.org1.fr.pool.ntp.org`" /syncfromflags:manual /update**

**w32tm /resync /force** : forcer la synchronisation d'un serveur NTP
    
### vérification que l'UAC est activé (User Account Control / Contrôle de compte d'utilisateurs)
    
**Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name EnableLUA**
Si valeur = 1 c'est activé Sinon 0 = pas activé
    

### Voir si Windows Defender est activé
    
**Get-NetFirewallProfile | ft Name,Enabled**
    
### Mises à jour
    
**Install-Module -Name PSWindowsUpdate -Force** : installation du module "WindowsUpdate" 
    
**Get-Command -Module PSWindowsUpdate** : visualiser les mises à jours disponibles du module Windows en entier
    
**Get-WindowsUpdate** : visualiser les mises à jour proposées manquantes sur le compte Windows actuel
    
**Install-WindowsUpdate -AcceptAll -AutoReboot** : Installation de toute les mises à jour disponibles du module windowsupdate et redémarrage automatique

    
### Securitiy Identifier (SID)
    
**Get-WmiObject -Class win32_UserAccount | Format-Table Name,SID** : Vérification d'un SID pour tout les utilisateurs :

**(Get-WmiObject -Class win32_UserAccount -Filter "name='Florian' and domain='$env:ComputerName'").SID** : Vérification d'un SID pour un compte spécifique
    
> Informations importantes : renommer un nom de compte n'a pas de consèquences sur le SID
    

### Modification d'un mot de passe 
   
#### Différentes étapes : 
**$NewPwd = Read-Host -AsSecureString** : stockage du mot de passe dans Read-Host (affichage de la console pour mettre le mot de passe)
    
**\$NewPwdText = "My@@Script"** 
**\$NewPwd = $NewPwdText | ConvertTo-SecureString -AsPlainText -Force** : 
1. Mot de passe définit dans la variable $NewPwdText 
2. Transformation du mot de passe en chaîne sécurisé pour être stocké dans $NewPwd 

**Get-LocalUser -Name "admin" | Set-LocalUser -Password \$NewPwd** : 

1. Choix de l'utilisateur
2. Changement définitif du mot de passe