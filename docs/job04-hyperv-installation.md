# Job 04 - Hyper-V sur Windows Server 2022

## Spécifications VM Serveur
- CPU : 2 x 2 cœurs (nested virt activé)[file:7]
- RAM : 16 Go[file:7]
- Stockage : 30 Go + 60 Go ISO

## Installation Hyper-V[file:7]

```powershell
# PowerShell Admin
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
New-VMSwitch -Name "NATSwitch" -SwitchType Internal

VM Debian 13[file:7]
Génération 2, Secure Boot OFF (Linux: None)

2 vCPU, 1 Go RAM, 8 Go VHDX

Voici un tutoriel détaillé étape par étape pour installer Hyper-V sur Windows Server 2022 (nested dans VMware), puis créer une VM Debian 13 (ou 12.x actuelle, stable). Les captures d'écran illustrent les étapes clés via des tutoriels officiels et guides référencés. Téléchargez ISO Debian 13 netinst depuis [cdimage.debian.org](https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current/amd64/iso-cd/) si disponible, sinon Debian 12.7. [it-connect](https://www.it-connect.fr/chapitres/hyper-v-installer-debian-linux-dans-une-vm/)

## 1. Activer nested virt sur VMware (prérequis)

Éditez settings VM WS2022 : CPU > **Expose hardware assisted virtualization**. Redémarrez VM. [nakivo](https://www.nakivo.com/blog/hyper-v-nested-virtualization-explained/)

## 2. Installer Hyper-V sur WS2022

1. Ouvrez **PowerShell admin** (`Win + X > Windows PowerShell (Admin)`).  
    [aws.amazon](https://aws.amazon.com/fr/compare/the-difference-between-type-1-and-type-2-hypervisors/)
2. Exécutez : `Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart`.  
   Confirmez (Y), redémarrage auto. [learn.microsoft](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/install-hyper-v)
    [ionos](https://www.ionos.fr/digitalguide/serveur/know-how/hyperviseurs-de-type-1-et-2/)
3. Vérifiez post-redémarrage : `Get-WindowsFeature Hyper-V` (Installed: True).  
   Créez switch : `New-VMSwitch -Name "NATSwitch" -SwitchType Internal`. [learn.microsoft](https://learn.microsoft.com/fr-fr/windows-server/virtualization/hyper-v/get-started/install-hyper-v)

## 3. Ouvrir Hyper-V Manager

Recherchez **Hyper-V Manager** dans Start.  
 [it-connect](https://www.it-connect.fr/les-types-dhyperviseurs/)

## 4. Créer VM Debian

1. **Action > Nouveau > Ordinateur virtuel**.  
    [purestorage](https://www.purestorage.com/fr/knowledge/type-1-vs-type-2-hypervisor.html)
   - Nom : `Debian13`. Génération : **2**. Lieu : D:\VMs. [it-connect](https://www.it-connect.fr/chapitres/hyper-v-installer-debian-linux-dans-une-vm/)
2. Mémoire : 1024 Mo. Réseau : NATSwitch.  
3. Disque : Nouveau VHDX 8 Go.  
    [cristeal](https://www.cristeal.com/hyperviseur-type-1-et-2-quelle-est-la-difference.html)
4. Installation : **Installer ultérieurement**.  
5. **Fini**. Éditez settings :  
   - Processeur : 2 vCPU, **Expose virtualization extensions**.  
   - DVD : Ajoutez ISO Debian. Secure Boot : Désactivez (Linux: None).  
    [techlabs](https://techlabs.blog/categories/debian-linux/create-a-debian-linux-virtual-machine-using-hyper-v)

## 5. Installer Debian 13 dans la VM

1. **Démarrer** VM > **Se connecter**.  
    [nutanix](https://www.nutanix.com/fr/info/hypervisor)
2. Boot sur ISO : Écran Debian (Graphique/Installé). Choisissez **Installé en mode texte**.  
   - Langue : Français. Clavier : Français.  
   - Réseau : Auto DHCP.  
   - Miroir : http://deb.debian.org/debian (France).  
    [mentor-communication](https://www.mentor-communication.com/hyperviseur-type-1-et-2-choix-strategique-pour-le-marketing-digital/)
3. Partition : **Guidé - utiliser tout le disque**. Écrire changements.  
4. Paquets : **Standard system utilities** + SSH server.  
5. GRUB > Continuer. Redémarrage.  
    [youtube](https://www.youtube.com/watch?v=YlrdJK6EbEM)

## 6. Post-install et vérifs

1. Login root (mot de passe défini). `ip a` pour IP.  
2. `apt update && apt upgrade -y && apt install openssh-server`.  
3. Depuis hôte WS2022 : `Enter-PSSession -VMName Debian13` ou SSH.  
    [fr.linkedin](https://fr.linkedin.com/advice/3/what-pros-cons-type-1-2-hypervisors-skills-virtual-machines?lang=fr&lang=fr)
4. Test nested : `egrep 'vmx|svm' /proc/cpuinfo` (flags visibles).  [vinchin](https://www.vinchin.com/vm-tips/hyper-v-enable-nested-virtualization.html)

| Étape | Screenshot ref | Problème courant |
|-------|----------------|------------------|
| Hyper-V install |  [ionos](https://www.ionos.fr/digitalguide/serveur/know-how/hyperviseurs-de-type-1-et-2/) | Redémarrage requis  [learn.microsoft](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/install-hyper-v) |
| VM creation |  [purestorage](https://www.purestorage.com/fr/knowledge/type-1-vs-type-2-hypervisor.html) | Gen2 + Secure Boot off  [techlabs](https://techlabs.blog/categories/debian-linux/create-a-debian-linux-virtual-machine-using-hyper-v) |
| Debian boot |  [mentor-communication](https://www.mentor-communication.com/hyperviseur-type-1-et-2-choix-strategique-pour-le-marketing-digital/) | Réseau pour netinst  [it-connect](https://www.it-connect.fr/chapitres/hyper-v-installer-debian-linux-dans-une-vm/) |

**Notes** : Debian 13 (Trixie) en testing ; utilisez 12 (Bookworm) stable. Perf nested ~80% bare metal. Logs : Event Viewer > Applications and Services > Hyper-V. [techlabs](https://techlabs.blog/categories/debian-linux/create-a-debian-linux-virtual-machine-using-hyper-v)


Problème résolu : Exposer virtualization extensions VMware[file:7]