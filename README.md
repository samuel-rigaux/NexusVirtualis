# 🚀 Opération Nexus Virtualis

> **Mission d'exploration avancée des hyperviseurs de type 1 : VMware ESXi, Microsoft Hyper-V, Proxmox VE et XCP-ng**

Documentation complète d'un projet pratique d'installation, configuration, migration et sauvegarde de machines virtuelles dans un environnement de virtualisation imbriquée (nested virtualization). 

***

## 📋 Table des matières

- [À propos du projet](#-à-propos-du-projet)
- [Prérequis](#-prérequis)
- [Architecture du projet](#-architecture-du-projet)
- [Jobs réalisés](#-jobs-réalisés)
  - [Job 01 - Concepts fondamentaux](#job-01---concepts-fondamentaux)
  - [Job 02 - Installation VMware Workstation Pro](#job-02---installation-vmware-workstation-pro)
  - [Job 03 - Téléchargement des ISO](#job-03---téléchargement-des-iso)
  - [Job 04 - Hyper-V sur Windows Server 2022](#job-04---hyper-v-sur-windows-server-2022)
  - [Job 05 - VMware ESXi](#job-05---vmware-esxi)
  - [Job 06 - Proxmox VE](#job-06---proxmox-ve)
  - [Job 07 - XCP-ng](#job-07---xcp-ng)
  - [Job 08 - Migration inter-hyperviseurs](#job-08---migration-inter-hyperviseurs)
  - [Job 09 - Proxmox Backup Server](#job-09---proxmox-backup-server)
  - [Job 10 - Solutions de sauvegarde](#job-10---solutions-de-sauvegarde)
- [Problèmes rencontrés et solutions](#-problèmes-rencontrés-et-solutions)
- [Compétences acquises](#-compétences-acquises)
- [Ressources](#-ressources)
- [Licence](#-licence)
- [Auteur](#-auteur)

***

## 🎯 À propos du projet

**Opération Nexus Virtualis** est un projet pédagogique exhaustif visant à maîtriser les technologies d'hyperviseurs de type 1 (bare metal). Le projet simule un environnement datacenter en utilisant la virtualisation imbriquée (Matryoshka - poupées russes virtuelles).

### Objectifs principaux

- Comprendre les différences entre hyperviseurs type 1 et type 2 
- Installer et configurer quatre hyperviseurs majeurs (ESXi, Hyper-V, Proxmox VE, XCP-ng)
- Créer et gérer des machines virtuelles Debian sur chaque plateforme
- Réaliser des migrations de VMs entre différents hyperviseurs 
- Mettre en place des stratégies de sauvegarde avec Proxmox Backup Server

### Compétences visées

- Administrer et sécuriser les infrastructures systèmes 
- Administrer et sécuriser les infrastructures virtualisées
- Concevoir une solution technique répondant à des besoins d'évolution de l'infrastructure
***

## 🔧 Prérequis

### Matériel requis

- **PC hôte** : CPU avec support VT-x/AMD-V, 16 Go RAM minimum (32 Go recommandé), 500 Go d'espace disque
- **Virtualisation imbriquée** activée dans le BIOS 
### Logiciels nécessaires

- [VMware Workstation Pro](https://www.broadcom.com/) (gratuit pour usage personnel depuis rachat par Broadcom) 
- Images ISO des hyperviseurs (voir [Job 03](#job-03---téléchargement-des-iso))

***

## 🏗️ Architecture du projet

```
Hôte physique (VMware Workstation Pro - Type 2)
│
├─── VM Hyper-V (Windows Server 2022)
│    └─── VM Debian 13 (2 vCPU, 1 Go RAM, 8 Go disk)
│
├─── VM ESXi 8.0
│    └─── VM Debian 13 (2 vCPU, 1 Go RAM, 8 Go disk)
│
├─── VM Proxmox VE
│    ├─── VM Debian 13 (2 vCPU, 1 Go RAM, 8 Go disk)
│    └─── Proxmox Backup Server
│
└─── VM XCP-ng
     └─── VM Debian 13 (2 vCPU, 1 Go RAM, 8 Go disk)
```

***

## 📚 Jobs réalisés

### Job 01 - Concepts fondamentaux

#### Hyperviseur Type 1 vs Type 2

**Type 1 (bare metal)**
- S'exécute directement sur le matériel physique
- Exemples : VMware ESXi, Microsoft Hyper-V Server, Xen, KVM
- Performance élevée avec accès direct au CPU, RAM et E/S
- Meilleure sécurité et isolation entre VMs

**Type 2 (hosted)**
- Fonctionne comme application sur un OS hôte 
- Exemples : VirtualBox, VMware Workstation, Parallels Desktop
- Plus simple à installer et configurer

#### Avantages du Type 1

- Performances optimales pour environnements datacenter
- Fonctionnalités entreprise (migration à chaud, haute disponibilité, clustering)
- Surface d'attaque réduite

#### Cas d'usage

- Datacenters et consolidation de serveurs
- Cloud privé/public (IaaS)
- Virtualisation de postes de travail (VDI)

📖 [Documentation complète Job 01](docs/job01-concepts.md)

***

### Job 02 - Installation VMware Workstation Pro

VMware Workstation Pro est désormais **gratuit pour usage personnel** depuis le rachat par Broadcom.

1. Téléchargement depuis [Broadcom](https://www.broadcom.com/)
2. Installation standard avec assistant
3. Activation de la virtualisation imbriquée dans les paramètres VM

📖 [Guide détaillé Job 02](docs/job02-vmware-workstation.md)

***

### Job 03 - Téléchargement des ISO

#### ESXi (VMware vSphere Hypervisor)

- Portail : [support.broadcom.com](https://support.broadcom.com) → My Downloads → vSphere → ESXi
- Versions disponibles : 8.0, 9.0
- ⚠️ Vérifier [prérequis matériels](https://kb.vmware.com/s/article/2107518) avant téléchargement

#### Hyper-V Server

- Version standalone gratuite : Hyper-V Server 2019
- Lien direct ISO : [Microsoft Evaluation Center](https://www.microsoft.com/fr-fr/evalcenter/download-hyper-v-server-2019)

#### Proxmox VE

- Page officielle : [proxmox.com/downloads](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso)
- Dernière version : 9.1 ou 8.4

#### XCP-ng

- Site officiel : [xcp-ng.org](https://xcp-ng.org)
- ISO 8.3 LTS : [mirrors.xcp-ng.org](https://mirrors.xcp-ng.org/isos/8.3/xcp-ng-8.3.0-20250606.iso)

**Bonnes pratiques**
- Vérifier les checksums SHA256 après téléchargement
- Tester en environnement nested virtualization avant production

📖 [Liste complète des liens et versions](docs/job03-iso-download.md)

***

### Job 04 - Hyper-V sur Windows Server 2022

#### Spécifications VM serveur

- **Processeur** : 2 x 2 cœurs avec virtualisation exposée
- **RAM** : 16 Go
- **Stockage** : 30 Go principal + 60 Go secondaire pour ISO
#### Installation Hyper-V

```powershell
# Ouvrir PowerShell en admin
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart

# Vérifier l'installation
Get-WindowsFeature Hyper-V

# Créer un switch réseau
New-VMSwitch -Name "NATSwitch" -SwitchType Internal
```

#### Création VM Debian 13

1. **Action > Nouveau > Ordinateur virtuel** dans Hyper-V Manager 
2. Nom : `Debian13`, Génération : **2** 
3. Mémoire : 1024 Mo, Réseau : NATSwitch
4. Disque : Nouveau VHDX 8 Go 
5. Paramètres processeur : 2 vCPU + **Expose virtualization extensions** 
6. Secure Boot : **Désactivé** (Linux: None) 

#### Activation nested virtualization (prérequis VMware)

Dans paramètres VM Windows Server 2022 :  
**CPU > Expose hardware assisted virtualization** 
📖 [Tutoriel complet avec captures](docs/job04-hyperv-installation.md)

***

### Job 05 - VMware ESXi

#### Spécifications VM serveur

- **Processeur** : 2 x 2 cœurs avec virtualisation activée
- **RAM** : 8 Go 
- **Stockage** : 30 Go principal + 142 Go secondaire

#### Installation ESXi 8.0

1. Créer VM sur VMware Workstation avec spécifications ci-dessus
2. Cocher virtualisation dans paramètres processeur
3. ⚠️ **Problème résolu** : Décocher "Plateforme d'ordinateur virtuel" dans fonctionnalités Windows
4. Installer ESXi depuis ISO, définir mot de passe root
5. Noter l'IP de gestion (ex: `https://192.168.29.131/`)

#### Configuration Datastore et VM Debian

1. Accéder interface web ESXi
2. **Stockage > Nouvelle banque de données** (associer disque secondaire 142 Go)
3. Uploader ISO Debian 13 sur datastore
4. **Machine virtuelle > Créer/Enregistrer une machine virtuelle**
   - 2 vCPU, 1 Go RAM, 8 Go stockage
   - Lecteur DVD : ISO Debian depuis datastore
📖 [Guide pas à pas Job 05](docs/job05-esxi-installation.md)

***

### Job 06 - Proxmox VE

#### Spécifications VM serveur

- **Processeur** : 2 x 2 cœurs
- **RAM** : 16 Go
- **Stockage** : 30 Go principal + 60 Go secondaire pour ISO Debian

#### Création VM Debian

Spécifications VM fille:
- Processeur : 2 x 2 cœurs
- RAM : 1 Go
- Stockage : 8 Go

**Installation**
1. Créer VM Proxmox sur VMware Workstation
2. Installer Proxmox VE depuis ISO
3. Se connecter à l'interface web avec identifiants
4. Créer VM Debian avec specs ci-dessus

📖 [Documentation complète Job 06](docs/job06-proxmox-installation.md)

***

### Job 07 - XCP-ng

#### Spécifications VM serveur

Dimensionnement similaire aux autres hyperviseurs:
- **RAM** : 16 Go
- **Stockage** : 60+ Go (extension nécessaire pour XOA)

#### Création VM Debian

- Processeur : 2 x 2 cœurs
- RAM : 1 Go
- Stockage : 8 Go

#### Problème résolu : Extension stockage XCP-ng

**Contexte** : VM XCP-ng sur VMware avec 50 Go disque → Local storage = 15-18 Go → **Erreur "SR_BACKEND_FAILURE_44 Insufficient space"** lors déploiement XOA.

**Solution : Script automatique**

```bash
#!/bin/bash
# Prérequis : Étendre disque VMware à 60+ Go, shutdown puis boot VM XCP-ng

UUID=$(xe sr-list name-label="Local storage" params=uuid --minimal)
echo "UUID Local: $UUID"

lsblk -f
read -p "Confirme partition Local (ex: /dev/sda3)? [Enter pour continuer]"

parted /dev/sda resizepart 3 100%
partprobe
pvresize /dev/sda3
vgdisplay VG_XenStorage-$UUID

lvextend -l +100%FREE /dev/VG_XenStorage-$UUID/
xe sr-scan uuid=$UUID
xe sr-list uuid=$UUID params=physical-size,free-space
echo "✅ Prêt pour XOA !"
```

**Déploiement XOA**

```bash
bash -c "$(wget -qO- https://xoa.io/deploy)"
# Login : admin@admin.net / admin
```

📖 [Guide détaillé avec solution stockage](docs/job07-xcpng-installation.md)  
📖 [Script complet extension Local Storage](docs/extend-xcp-storage.md)

***

### Job 08 - Migration inter-hyperviseurs

#### Combinaisons testées

Migrations réalisées:
- Hyper-V → ESXi → Proxmox → XCP-ng
- Proxmox → ESXi
- ESXi → Hyper-V

#### Outils et méthodes

- Export/Import OVF/OVA
- Conversion de formats (VHDX, VMDK, qcow2)
- Outils : `qemu-img`, `StarWind V2V Converter`, `ovftool`

📖 [Guide complet migrations](docs/job08-migrations.md)

***

### Job 09 - Proxmox Backup Server

#### Configuration sauvegarde automatique

- **Fréquence** : Toutes les 2 heures
- **Rétention** : Conserver uniquement les 3 dernières sauvegardes
- **Cible** : VM Debian sur Proxmox VE

**Étapes**
1. Installer Proxmox Backup Server (VM dédiée ou conteneur)
2. Configurer datastore PBS
3. Ajouter datastore PBS dans Proxmox VE (Datacenter > Storage)
4. Créer job de sauvegarde avec schedule `*/2 * * * *` (cron)
5. Configurer retention : `keep-last 3`

📖 [Configuration PBS complète](docs/job09-proxmox-backup.md)

***

### Job 10 - Solutions de sauvegarde

#### Solutions tierces analysées

- **Veeam Backup & Replication** : Leader marché entreprise
- **Proxmox Backup Server** : Open source intégré
- **VMware vSphere Data Protection** : Natif ESXi
- **Acronis Cyber Backup** : Multi-plateforme
- **Nakivo Backup & Replication** : Alternatif abordable

#### Critères d'évaluation

- Support multi-hyperviseurs
- Déduplication et compression
- Restauration granulaire
- Coût et licences
- Intégration CI/CD

📖 [Comparatif solutions sauvegarde](docs/job10-backup-solutions.md)

***

## 🛠️ Problèmes rencontrés et solutions

### 1. VMware Workstation - Virtualisation imbriquée

**Problème** : VM ne démarre pas après activation nested virtualization  
**Solution** : Décocher "Plateforme d'ordinateur virtuel" dans fonctionnalités Windows

### 2. XCP-ng - Espace insuffisant pour XOA

**Problème** : `SR_BACKEND_FAILURE_44 Insufficient space` lors déploiement XOA
**Solution** : Script automatique d'extension Local Storage (voir [Job 07](#job-07---xcp-ng))

### 3. Hyper-V - Secure Boot avec Linux

**Problème** : VM Debian ne boot pas  
**Solution** : Désactiver Secure Boot (paramètre "None" pour Linux)

### 4. ESXi - Compatibilité matérielle

**Problème** : ESXi ne détecte pas le matériel  
**Solution** : Vérifier [prérequis matériels](https://kb.vmware.com/s/article/2107518) avant installation

📖 [Base de connaissances complète](docs/troubleshooting.md)

***

## 🎓 Compétences acquises

- ✅ Installation et configuration d'hyperviseurs type 1 en environnement nested
- ✅ Gestion des ressources virtuelles (CPU, RAM, stockage, réseau)
- ✅ Migration de VMs entre plateformes hétérogènes
- ✅ Mise en place de stratégies de sauvegarde automatisées
- ✅ Troubleshooting d'infrastructures virtualisées 
- ✅ Documentation technique exhaustive de projets complexes

***

## 📚 Ressources

### Documentation officielle

- [VMware ESXi](https://docs.vmware.com/en/VMware-vSphere/index.html)
- [Microsoft Hyper-V](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/)
- [Proxmox VE](https://pve.proxmox.com/wiki/Main_Page)
- [XCP-ng](https://docs.xcp-ng.org/)

### Outils mentionnés

- [VMware Workstation Pro](https://www.broadcom.com/)
- [Proxmox Backup Server](https://www.proxmox.com/en/proxmox-backup-server)
- [Xen Orchestra (XOA)](https://xen-orchestra.com/)

### Articles de référence

- [Hyperviseurs Type 1 vs Type 2](https://www.ionos.fr/digitalguide/serveur/know-how/hyperviseurs-de-type-1-et-2/)
- [Nested Virtualization Explained](https://www.nakivo.com/blog/hyper-v-nested-virtualization-explained/)

***

## 📄 Licence

Ce projet est sous licence **MIT**. Voir [LICENSE](LICENSE) pour plus d'informations.

***

## 👤 Auteur

**Votre Nom**

- GitHub : [@votre-username](https://github.com/samuel-rigaux)
- LinkedIn : [Votre Profil](https://linkedin.com/in/samuel-rigaux)
- Email : samuel.rigaux@laplateforme.io

***

## ⭐ Remerciements

- **La Plateforme** pour le projet pédagogique
- Communautés open source (Proxmox, XCP-ng)
- Tous les contributeurs et testeurs

***

<div align="center">

**[⬆ Retour en haut](#-opération-nexus-virtualis)**

Made with ❤️ and ☕ | © 2026

</div>

***


Ce README est accompagné d'une documentation détaillée pour chaque job dans le dossier `/docs/`. Consultez les liens ci-dessus pour guides complets, captures d'écran et scripts.

