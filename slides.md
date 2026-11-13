---
marp: true
theme: default
style: |
  section {background-color: #121114}
  h1,h2,h3 {color: #8393f0}
  p,ul,li,td,th {color: #ccc}
  section table {font-size: 0.85em}
  section table td {background-color: #121114}
  section table th {background-color: #272133; font-weight: bolder}
  pre {background-color: #17151a; color: #ccc}
  blockquote {border: 2px green solid; border-left-width: 15px; padding: 0.5em 15px;font-style: italic }
paginate: true
header: "Windows Server et Active Directory"
footer: "![height:20px](https://raw.githubusercontent.com/DiginamicInternal/PublicAssets/refs/heads/main/Logo-diginamic-color-blk.png)"
---

# Windows Server et Active Directory

### Infrastructures Windows

Nicolas SAHUC

---

## 🎯 Objectifs pédagogiques

1. Choisir son édition de Windows Server
2. Installer et configurer un serveur
3. Déployer AD DS, DNS, DHCP et les rôles complémentaires
4. Comprendre DNS, Kerberos, LDAP, la réplication
5. Décrire domaines, arbres, forêts, schéma
6. Administrer utilisateurs, groupes, ordinateurs
7. Appliquer AGDLP aux partages
8. Maîtriser les GPO

---

## Le parcours

| #  | Étape                                    | Durée  |
|----|------------------------------------------|--------|
| 01 | Présentation de Windows Server           | 1 h    |
| 02 | Installation et post-installation        | 2 h    |
| 03 | Rôles et services                        | 1 h 30 |
| 04 | Active Directory, DNS et Kerberos        | 2 h    |
| 05 | LDAP et contrôleurs de domaine           | 1 h 30 |
| 06 | Domaines, forêts et structure            | 1 h 30 |
| 07 | Utilisateurs, groupes et ordinateurs     | 3 h    |
| 08 | AGDLP, SMB et NTFS                       | 2 h    |
| 09 | Stratégies de groupe                     | 2 h 30 |

---

# 1. Windows Server

---

## Les rôles d'un serveur

DNS · DHCP · AD DS · fichiers · impression · IIS · Hyper-V · AD CS (certificats) · AD FS (SSO)

+ produits installés dessus : Exchange, SQL Server

---

## Les éditions

| Édition | Pour | Licence |
|---|---|---|
| Essentials | ≤ 25 utilisateurs, 50 appareils | par serveur, sans CAL |
| Standard | peu virtualisé, 2 VM | par cœur + CAL |
| Datacenter | fortement virtualisé, VM illimitées | par cœur + CAL |
| Azure Edition | Azure, hotpatching | abonnement |

---

# 2. Installation

---

## Installer

- 64 bits, 1,4 GHz · 512 Mo (Core) / 2 Go (GUI) · 32 Go · UEFI, TPM
- **Expérience de bureau** (GUI) ou **Server Core** (CLI, surface d'attaque réduite)

## Post-installation

Nom d'hôte → réseau (IP **fixe**) → pilotes → mises à jour

---

## Le réseau en PowerShell

```powershell
Get-NetAdapter
Set-NetIPInterface -InterfaceIndex 4 -Dhcp Disabled
New-NetIPAddress -InterfaceIndex 4 -IPAddress 192.168.10.10 `
    -PrefixLength 24 -DefaultGateway 192.168.10.254
Set-DnsClientServerAddress -InterfaceIndex 4 -ServerAddresses 192.168.10.10
```

> On ne désactive **jamais** le pare-feu Windows : on crée la règle.

---

# 3. Rôles et services

---

## Rôles et fonctionnalités

- **Rôle** : un service rendu au réseau · **Fonctionnalité** : une extension du serveur
- N'installer que le **nécessaire**
- Ordre : **AD DS** → **DNS** → **DHCP** → le reste

| Service | Indispensable au domaine |
|---|---|
| AD DS | oui |
| DNS | oui |
| DHCP | non |

---

# 4. AD, DNS, Kerberos

---

## AD dépend de DNS

- Les clients **trouvent les DC** par DNS (enregistrements SRV)
- Zone du domaine créée et peuplée automatiquement
- Domaine AD ≠ domaine DNS

---

## Kerberos

1. Le client s'authentifie auprès de l'**AS** → **TGT**
2. Il présente son TGT au **TGS** → **ticket de service**
3. Il présente le ticket au serveur → accès

> Le mot de passe ne circule jamais. Plus de KDC = plus d'authentification.

---

# 5. LDAP et DC

---

## Identifier un objet

- **GUID** : 128 bits, unique, immuable
- **DN** : `CN=John,OU=IT,OU=entreprise,DC=test,DC=lab` — change si l'objet bouge
- Attributs : `sAMAccountName`, `userPrincipalName`, `objectSid`, `mail`…

LDAP 389 · LDAPS 636 · catalogue global 3268

---

## Le contrôleur de domaine

- Authentifie, applique les GPO, stocke `NTDS.dit`
- **Deux DC minimum**
- Réplication de l'annuaire et de **SYSVOL** (DFSR)

> `NTDS.dit` contient les condensats de tous les mots de passe.

---

# 6. Domaines et forêts

---

## Domaine, arbre, forêt

- **Domaine** : un triangle, des UO, des objets
- **Arbre** : domaines au nom contigu (`lyon.test.lab`)
- **Forêt** : un ou plusieurs arbres, schéma et catalogue global communs
- **Approbations** : sens, transitivité
- **Niveau fonctionnel** : domaines d'abord, forêt ensuite, pas de retour

---

## Schéma et partitions

| Partition | Unicité |
|---|---|
| Schéma | 1 par forêt |
| Configuration | 1 par forêt |
| Domaine | 1 par domaine |

> Modifier le schéma : irréversible, toute la forêt.

---

# 7. Utilisateurs, groupes, ordinateurs

---

## Administrer

- Consoles : `dsa.msc`, ADAC, PowerShell (RSAT)
- **UO** : déléguer et cibler les GPO
- **Désactiver** avant de supprimer

```powershell
New-ADUser -Name "Jeanne DUPONT" -SamAccountName jdupont `
  -AccountPassword (Read-Host -AsSecureString) -Enabled $true
Add-Computer -DomainName entreprise.lan -Restart
```

---

## Les groupes

| Étendue | Contient | Droits sur |
|---|---|---|
| Domaine local | tout domaine approuvé | son domaine |
| Globale | son domaine | tout domaine approuvé |
| Universelle | toute la forêt | toute la forêt |

**Sécurité** (SID, droits) · **Distribution** (messagerie)

---

# 8. AGDLP, SMB, NTFS

---

## AGDLP

**A**ccount → **G**lobal → **D**omain **L**ocal → **P**ermissions

- `GG-Comptabilite` → `GDL-COMPTA-RW` → Modification sur `COMPTA`
- Partage SMB : Contrôle total aux Utilisateurs authentifiés
- Tout se règle dans **NTFS**

> Droits effectifs = le plus restrictif entre partage et NTFS.

---

# 9. Stratégies de groupe

---

## GPO

- **GPC** dans l'annuaire, **GPT** dans SYSVOL
- Ordre **LSDOU** : Locale, Site, Domaine, UO — la dernière l'emporte
- **Appliqué** = *Enforced* : impose la GPO · **Lien activé** : l'active
- Actualisation : 90 min ± 30

```bat
gpupdate /force
gpresult /r
```

---

# Des questions ?

Pour toute question [adressez-vous au service pédagogie](mailto:pedagogie@diginamic.fr)
