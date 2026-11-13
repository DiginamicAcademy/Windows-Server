[← Chapitre précédent](01-Presentation-de-Windows-Server.md) · [Sommaire](Readme.md) · [Chapitre suivant →](03-Roles-et-services.md)

# Installation et post-installation

## 🎯 Objectifs du chapitre

- Vérifier les prérequis matériels
- Installer Windows Server, avec ou sans interface graphique
- Configurer le nom d'hôte et le réseau, en graphique et en PowerShell
- Gérer le pare-feu Windows et les mises à jour

---

## 1. Prérequis matériels

| Ressource | Minimum |
|---|---|
| Processeur | 1,4 GHz, 64 bits |
| RAM | 512 Mo (Server Core) — 2 Go avec l'expérience de bureau |
| Disque | 32 Go |
| Réseau | carte Ethernet d'au moins 1 Gbit/s |
| Autres | micrologiciel UEFI avec démarrage sécurisé ; TPM pour certaines fonctionnalités ; affichage Super VGA (1024 × 768) |

> 💡 Ce sont des minimums d'installation. En pratique, comptez 4 Go de RAM et
> plus pour un contrôleur de domaine, selon les rôles hébergés.

---

## 2. Étapes d'installation

| Étape | Actions |
|---|---|
| **Préparation** | récupérer le support (ISO ou clé USB), vérifier les prérequis, choisir la méthode d'installation |
| **Démarrage** | insérer le support, démarrer dessus |
| **Configuration** | choisir la langue, le format horaire et le clavier |
| **Choix de l'édition** | choisir l'édition et le mode d'installation (voir ci-dessous) |
| **Licence** | lire et accepter les termes du contrat |
| **Disque** | choisir l'emplacement d'installation, partitionner si besoin |
| **Installation** | patienter pendant la copie et la configuration |
| **Finalisation** | redémarrer, définir le mot de passe de l'administrateur, se connecter |

### Expérience de bureau ou Server Core ?

| | Expérience de bureau | Server Core |
|---|---|---|
| Interface | graphique (GUI) | ligne de commande (CLI) : PowerShell, `sconfig` |
| Surface d'attaque | plus large | réduite |
| Ressources, mises à jour | plus nombreuses | moins nombreuses |
| Administration | locale ou à distance | à distance (RSAT, Windows Admin Center) ou PowerShell |

---

## 3. Configuration post-installation

Avant d'installer le moindre rôle, on prépare le serveur :

1. le **nom d'hôte** ;
2. les **paramètres réseau** ;
3. les **pilotes** ;
4. les **mises à jour**.

### Le nom d'hôte

Le nom d'hôte (*hostname*) identifie la machine sur le réseau. Suivi du nom
de domaine, il forme le **FQDN** (*Fully Qualified Domain Name*) :
`srv-dc01.entreprise.lan`.

```powershell
Rename-Computer -NewName "SRV-DC01" -Restart
```

> ⚠️ Choisissez le nom **avant** de promouvoir le serveur en contrôleur de
> domaine ou de le joindre au domaine : le renommer ensuite est risqué.

### La configuration réseau

On définit l'adresse IP, le masque, la passerelle par défaut et les serveurs
DNS préférés. Un serveur a **toujours** une adresse IP fixe.

#### En graphique

`ncpa.cpl`, depuis **Exécuter** (`Windows + R`) ou une invite de commande,
ouvre directement la liste des connexions réseau.

#### En PowerShell

```powershell
# Lister les cartes réseau, et repérer leur index (ifIndex)
Get-NetAdapter
Get-NetIPInterface

# Afficher la configuration IP et DNS d'une carte
Get-NetIPConfiguration -InterfaceIndex 4
Get-DnsClientServerAddress -InterfaceIndex 4

# Désactiver le DHCP sur la carte
Set-NetIPInterface -InterfaceIndex 4 -Dhcp Disabled

# Configurer adresse, masque (longueur de préfixe) et passerelle
New-NetIPAddress -InterfaceIndex 4 -IPAddress 192.168.10.10 -PrefixLength 24 -DefaultGateway 192.168.10.254

# Configurer les DNS préférés
Set-DnsClientServerAddress -InterfaceIndex 4 -ServerAddresses 192.168.10.10,192.168.10.11
```

`New-NetIPAddress` **n'écrase pas** une configuration existante. Pour la
modifier, on retire d'abord l'ancienne :

```powershell
Remove-NetIPAddress -InterfaceIndex 4 -IPAddress 192.168.10.10 -PrefixLength 24
Remove-NetRoute -InterfaceIndex 4 -NextHop 192.168.10.254
```

### Le pare-feu Windows

Le pare-feu Windows est un composant de sécurité intégré qui filtre le trafic
entrant et sortant **de la machine elle-même** — à la différence d'un
pare-feu d'infrastructure, qui protège tout un réseau. Il se gère par la
console **Pare-feu Windows Defender avec fonctions avancées de sécurité**
(`wf.msc`) ou en PowerShell.

```powershell
# Autoriser le ping entrant (ICMPv4)
New-NetFirewallRule -DisplayName "Autoriser ICMPv4" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow
```

> ⚠️ Ne **désactivez jamais** le pare-feu Windows, même « pour tester ».
> Créez la règle qui autorise le flux nécessaire.

### Les mises à jour

Après l'installation : mettre à jour le système, les pilotes et les
composants de sécurité (Microsoft Defender).

- **Windows Update** : les mises à jour de sécurité comblent les failles découvertes ; ne jamais les négliger.
- **WSUS** (*Windows Server Update Services*) : dans un parc, un serveur WSUS centralise, approuve et distribue les mises à jour aux autres machines.
- **Pilotes** : la plupart arrivent par Windows Update ; certains doivent être récupérés sur le site du constructeur.

---

## ✅ Exercice

Sur une VM Windows Server fraîchement installée, et uniquement en PowerShell :

1. renommez le serveur `SRV-DC01` ;
2. attribuez-lui l'adresse `192.168.10.10/24`, la passerelle `192.168.10.254`, et lui-même comme DNS ;
3. autorisez le ping entrant ;
4. vérifiez la configuration.

<details>
<summary>Voir la correction</summary>

```powershell
Rename-Computer -NewName "SRV-DC01" -Restart
# après redémarrage
Get-NetAdapter                       # repérer l'ifIndex, ici 4
Set-NetIPInterface -InterfaceIndex 4 -Dhcp Disabled
New-NetIPAddress -InterfaceIndex 4 -IPAddress 192.168.10.10 -PrefixLength 24 -DefaultGateway 192.168.10.254
Set-DnsClientServerAddress -InterfaceIndex 4 -ServerAddresses 192.168.10.10
New-NetFirewallRule -DisplayName "Autoriser ICMPv4" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow
Get-NetIPConfiguration -InterfaceIndex 4
```

</details>

---

[← Chapitre précédent](01-Presentation-de-Windows-Server.md) · [Sommaire](Readme.md) · [Chapitre suivant →](03-Roles-et-services.md)
