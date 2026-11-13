[Sommaire](Readme.md) · [Chapitre suivant →](02-Installation-et-post-installation.md)

# Présentation de Windows Server

## 🎯 Objectifs du chapitre

- Définir Windows Server et ce qui le distingue de Windows client
- Connaître les principaux rôles qu'un serveur Windows peut remplir
- Comparer les éditions de Windows Server et leur modèle de licence

---

## 1. Généralités

Un serveur Windows est un système équipé de **Windows Server**, conçu pour
fournir des services à l'ensemble d'un réseau. Contrairement aux éditions
destinées aux postes de travail, comme Windows 11, Windows Server est pensé
pour les environnements serveur : il offre une large gamme de rôles et de
services, et se déploie aussi bien dans une petite structure que dans un
grand datacenter.

---

## 2. Les rôles d'un serveur Windows

Chaque **rôle** permet au serveur de rendre un service aux clients de
l'infrastructure. Liste non exhaustive :

| Rôle | Service rendu |
|---|---|
| **DNS** (*Domain Name System*) | traduction des noms de domaine en adresses IP, et inversement |
| **DHCP** (*Dynamic Host Configuration Protocol*) | attribution automatique des configurations réseau aux clients |
| **Contrôleur de domaine Active Directory** (AD DS) | gestion centralisée des comptes, des stratégies de groupe, de l'authentification et des autorisations |
| **Serveur de fichiers** | stockage et partage de fichiers et de dossiers |
| **Serveur d'impression** | gestion centralisée des impressions |
| **Serveur web IIS** (*Internet Information Services*) | hébergement de sites et d'applications web, en HTTP ou HTTPS |
| **Messagerie — Exchange Server** | messagerie interne et externe |
| **Bases de données — SQL Server** | système de gestion de bases de données relationnelles |
| **Virtualisation — Hyper-V** | hébergement de machines virtuelles |
| **Autorité de certification — AD CS** (*Active Directory Certificate Services*) | émission et gestion de certificats, pour sécuriser les communications |
| **Fédération — AD FS** (*Active Directory Federation Services*) | authentification unique (**SSO**) et fédération d'identité : un seul identifiant pour plusieurs applications |

> 💡 Exchange et SQL Server sont des produits distincts, installés sur
> Windows Server ; les autres sont des rôles intégrés au système.

---

## 3. Les éditions

| Édition | Destination |
|---|---|
| **Essentials** | petites structures : 25 utilisateurs, 50 appareils, 1 processeur de 10 cœurs au plus |
| **Standard** | environnements physiques ou peu virtualisés |
| **Datacenter** | centres de données fortement virtualisés |
| **Datacenter : Azure Edition** | exécution exclusive dans Azure (ou Azure Stack HCI) ; apporte notamment le **Hotpatching**, l'application de mises à jour sans redémarrage |

### Les licences

- **Standard** et **Datacenter** : licence **par cœur** de processeur (vendue par paquet de 16 cœurs), plus une **CAL** (*Client Access License*) par utilisateur ou par appareil qui accède au serveur.
- **Essentials** : une licence unique par serveur, **sans CAL**.

Prix publics indicatifs, Windows Server 2022 :

| Édition | Modèle | CAL nécessaires | Prix public indicatif |
|---|---|---|---|
| Essentials | par serveur | non | ≈ 500 $ |
| Standard | par cœur (16 cœurs) | oui | ≈ 1 070 $ |
| Datacenter | par cœur (16 cœurs) | oui | ≈ 6 150 $ |

### Les fonctionnalités

| Fonctionnalité | Standard / Essentials | Datacenter | Datacenter : Azure Edition |
|---|---|---|---|
| Fonctionnalités de base de Windows Server | ✔ | ✔ | ✔ |
| Machines virtuelles Windows Server couvertes par la licence | 2 | illimitées | illimitées |
| Réseau défini par logiciel (SDN) | ✘ | ✔ | ✔ |
| Réplica de stockage | limité | ✔ | ✔ |
| Espaces de stockage direct (S2D) | ✘ | ✔ | ✔ |
| Service Guardian hôte, machines virtuelles protégées | ✘ | ✔ | ✔ |
| Hotpatching | ✘ | ✘ | ✔ |
| SMB over QUIC | ✘ | ✘ | ✔ |
| Réseau étendu Azure | ✘ | ✘ | ✔ |

> 💡 Windows Server 2025 reprend ces éditions et étend certaines
> fonctionnalités (SMB over QUIC, Hotpatch via Azure Arc) aux éditions
> Standard et Datacenter. Vérifiez toujours la matrice de la version que vous
> déployez.

### Bien choisir

Évaluez les fonctionnalités de chaque édition au regard des besoins réels de
l'organisation : le bon choix optimise l'efficacité de l'infrastructure,
limite les coûts et préserve l'évolutivité. Règle pratique : au-delà d'une
dizaine de VM Windows Server par hôte, l'édition Datacenter devient
généralement plus économique que l'empilement de licences Standard.

---

## ✅ Vérifier ses acquis

1. Quelle édition choisir pour une PME de 15 salariés avec un seul serveur physique ?
2. Que couvre une CAL ?
3. Quel rôle met en place l'authentification unique vers plusieurs applications ?

<details>
<summary>Voir la correction</summary>

1. Essentials : moins de 25 utilisateurs et 50 appareils, un seul processeur, pas de CAL à acheter.
2. Le droit, pour un utilisateur ou un appareil, d'accéder à un serveur Windows Server (Standard ou Datacenter).
3. AD FS (*Active Directory Federation Services*).

</details>

---

[Sommaire](Readme.md) · [Chapitre suivant →](02-Installation-et-post-installation.md)
