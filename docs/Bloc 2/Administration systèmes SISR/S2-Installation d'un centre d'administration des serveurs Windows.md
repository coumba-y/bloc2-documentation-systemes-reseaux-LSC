# BLOC  2 - Administration des systèmes - Situation 2 : Installation d'un centre d'administration des serveurs Windows (WAC)

**Compte rendu rédigé par** : BIDANESSY Coumba
**Formation** : BTS SIO 2ème année - Option SISR  
**Établissement** : Lycée Paul-Louis Courier, Tours  


# Installation d'un serveur Windows Admin Center (WAC)

## Objectif

Déployer un serveur **Windows Admin Center (WAC)** sur une machine virtuelle Proxmox afin d'administrer les serveurs Windows de l'infrastructure depuis une interface web sécurisée.

---

# 1. Création de la machine virtuelle

Créer une machine virtuelle **Windows Server 2025** dans Proxmox avec une configuration adaptée :

| Paramètre | Configuration |
|-----------|---------------|
| Nom de la VM | SRV-WAC |
| Système d'exploitation | Windows Server 2025 x64 |
| BIOS | UEFI (OVMF) |
| Disque | 60 Go minimum |
| Processeur | 2 vCPU minimum |
| Mémoire | 4 Go minimum |
| Carte réseau | VirtIO reliée au bridge du réseau |

Installer Windows Server ainsi que les pilotes **VirtIO** pour le disque et la carte réseau.

---

# 2. Configuration réseau des serveurs

Attribuer une adresse IP fixe à chaque serveur du réseau en suivant un plan d'adressage cohérent.

| Serveur | Rôle | Adresse IP |
|----------|------|------------|
| ServeurADO | Contrôleur de domaine principal (Active Directory / DNS) | **192.168.1.1** |
| AD1 | Contrôleur de domaine secondaire | **192.168.1.2** |
| AD2 | Contrôleur de domaine supplémentaire | **192.168.1.3** |
| DHCP0 | Serveur DHCP principal | **192.168.1.4** |
| DHCP1 | Serveur DHCP secondaire | **192.168.1.5** |
| DHCP2 | Serveur DHCP supplémentaire | **192.168.1.6** |
| SRV-WAC | Serveur Windows Admin Center | **192.168.1.9** |

### Configuration réseau du serveur WAC

| Paramètre | Valeur |
|-----------|--------|
| Adresse IP | 192.168.1.9 |
| Masque | 255.255.255.128 |
| Passerelle | 192.168.1.126 |



---

# 3. Renommage du serveur

Renommer la machine avant son intégration au domaine.

```powershell
Rename-Computer -NewName "SRV-WAC" -Restart
```

Après le redémarrage, vérifier le nom du serveur.

---


# 5. Installation de Windows Admin Center

Télécharger puis lancer le programme d'installation de **Windows Admin Center**.

Choisir les paramètres suivants :

| Option | Configuration |
|--------|---------------|
| Mode d'installation | Gateway |
| Port HTTPS | 443 |
| Certificat | Certificat auto-signé |
| Service Windows | Activé |
| Démarrage du service | Automatique |

Attendre la fin de l'installation.

---

# 6. Vérification du service WAC

Contrôler que le service Windows Admin Center est démarré.

```powershell
Get-Service ServerManagementGateway
```

Si le service est arrêté :

```powershell
Start-Service ServerManagementGateway
```

Le statut attendu est **Running**.

---

# 7. Autorisation du pare-feu

Autoriser le trafic HTTPS vers Windows Admin Center.

```powershell
New-NetFirewallRule `
-DisplayName "Windows Admin Center HTTPS" `
-Direction Inbound `
-Protocol TCP `
-LocalPort 443 `
-Action Allow
```

Vérifier la règle créée.

```powershell
Get-NetFirewallRule -DisplayName "Windows Admin Center HTTPS"
```

---

# 8. Accès à Windows Admin Center

Depuis un navigateur d'un poste du réseau, accéder à l'interface :

```text
https://192.168.1.9
```

Accepter le certificat auto-signé si nécessaire puis se connecter avec un compte administrateur.

---

# 9. Ajout des serveurs dans Windows Admin Center

Ajouter les serveurs de l'infrastructure afin de les administrer depuis WAC.

| Serveur | Adresse IP |
|---------|------------|
| ADO | 192.168.1.1 |
| AD1 | 192.168.1.2 |
| AD2 | 192.168.1.3 |
| DHCP0 | 192.168.1.4 |
| DHCP1 | 192.168.1.5|
| DHCP2 | 192.168.1.6 |

Pour chaque serveur :

1. Cliquer sur **Ajouter**.
2. Sélectionner **Serveur**.
3. Saisir le nom ou l'adresse IP.
4. Utiliser un compte administrateur du domaine.
5. Valider l'ajout.

Les serveurs apparaissent ensuite dans le tableau de bord de Windows Admin Center.

---

# Vérifications finales

| Vérification | Résultat attendu |
|--------------|------------------|
| Ping entre les serveurs | Connectivité réseau fonctionnelle. 
| Service `ServerManagementGateway` | Running. |
| Accès HTTPS | Interface WAC accessible sur `https://192.168.1.9`. |
| Gestion des serveurs | ADO, AD1, AD2, DHCP0, DHCP1 et DHCP2 visibles dans WAC. |

