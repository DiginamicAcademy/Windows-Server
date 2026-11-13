<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/Logo-diginamic-color-blk.png">
    <img src="assets/Logo-diginamic-color.svg" alt="Diginamic Formation" width="358">
  </picture>
</p>

# Windows Server et Active Directory

Les infrastructures Windows, du serveur seul au domaine Active Directory :
choisir son édition, installer et configurer un serveur, déployer ses rôles,
comprendre l'annuaire et son architecture, puis administrer utilisateurs,
groupes, droits d'accès et stratégies de groupe.

**Auteur :** Nicolas SAHUC <sahuc.nic@gmail.com>

## 🎯 Objectifs pédagogiques

À la fin de ce cours, vous saurez :

1. Présenter Windows Server, ses rôles et ses éditions, et choisir l'édition adaptée à un besoin.
2. Installer Windows Server et réaliser sa configuration post-installation, en graphique et en PowerShell.
3. Installer et situer les rôles AD DS, DNS, DHCP, serveur de fichiers, d'impression et IIS.
4. Expliquer le fonctionnement d'Active Directory : DNS, Kerberos, LDAP, contrôleurs de domaine, réplication.
5. Décrire l'architecture logique : domaines, arbres, forêts, approbations, niveaux fonctionnels, schéma, partitions.
6. Administrer utilisateurs, groupes et ordinateurs, en console et en PowerShell.
7. Appliquer la méthode AGDLP pour gérer les droits d'accès aux partages SMB et NTFS.
8. Créer, lier et dépanner des stratégies de groupe (GPO).

## ❔ Pourquoi ce cours ?

Dans l'immense majorité des entreprises, l'identité des utilisateurs, l'accès
aux fichiers et la configuration des postes reposent sur un domaine Active
Directory hébergé par Windows Server. C'est le cœur du système d'information
— et la première cible des attaquants. L'administrer correctement est une
compétence centrale de tout administrateur systèmes et réseaux.

## Prérequis

- Notions de réseau IP (adressage, masque, passerelle, DNS)
- Un hyperviseur (Hyper-V, VirtualBox, VMware, Proxmox) pour les travaux pratiques, et une image d'évaluation de Windows Server

Le cours « Active Directory, DNS et DHCP » en constitue une introduction plus courte.

## Le parcours

| # | Étape | Durée | Ce qu'on y fait |
|---|---|---|---|
| [01](01-Presentation-de-Windows-Server.md) | Présentation de Windows Server | 1 h | rôles d'un serveur, éditions, licences |
| [02](02-Installation-et-post-installation.md) | Installation et post-installation | 2 h | prérequis, installation, nom d'hôte, réseau en PowerShell, pare-feu, mises à jour |
| [03](03-Roles-et-services.md) | Rôles et services | 1 h 30 | rôles et fonctionnalités, gestionnaire de serveur, AD DS, DNS, DHCP, fichiers, impression, IIS |
| [04](04-Active-Directory-DNS-et-Kerberos.md) | Active Directory, DNS et Kerberos | 2 h | intérêt de l'annuaire, enregistrements DNS du domaine, authentification Kerberos |
| [05](05-LDAP-et-controleurs-de-domaine.md) | L'annuaire LDAP et les contrôleurs de domaine | 1 h 30 | GUID, DN, attributs, rôle du DC, NTDS.dit, réplication, SYSVOL |
| [06](06-Domaines-forets-et-structure.md) | Domaines, forêts et structure de l'annuaire | 1 h 30 | domaine, arbre, forêt, approbations, niveaux fonctionnels, schéma, partitions |
| [07](07-Utilisateurs-groupes-et-ordinateurs.md) | Utilisateurs, groupes et ordinateurs | 3 h | consoles, UO, comptes, modèles, étendues et types de groupes, jonction au domaine |
| [08](08-Gestion-des-ressources-AGDLP.md) | Gestion des ressources : AGDLP, SMB, NTFS | 2 h | méthode AGDLP, convention de nommage, partages, permissions NTFS |
| [09](09-Strategies-de-groupe.md) | Les stratégies de groupe | 2 h 30 | GPC et GPT, stratégies locales et de domaine, LSDOU, Appliqué, actualisation |
