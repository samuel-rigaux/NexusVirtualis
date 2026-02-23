# Job 01 - Concepts fondamentaux

## Hyperviseur Type 1 vs Type 2

**Type 1 (bare metal)**
- S'exécute directement sur le matériel physique[file:9]
- Exemples : VMware ESXi, Microsoft Hyper-V Server, Xen, KVM
- **Performance élevée** : accès direct CPU, RAM, E/S[file:9]
- **Sécurité renforcée** : isolation optimale entre VMs[file:9]

**Type 2 (hosted)**
- Application sur OS hôte (Windows/Linux/macOS)[file:9]
- Exemples : VirtualBox, VMware Workstation, Parallels

## Avantages Type 1[file:9]

| Avantage | Détail |
|----------|--------|
| Performance | Surcoût minimal virtualisation[file:9] |
| Sécurité | Surface d'attaque réduite[file:9] |
| Scalabilité | Clustering, HA, migration à chaud[file:9] |

## Cas d'usage Type 1[file:9]

- **Datacenters** : consolidation serveurs
- **Cloud IaaS** : multi-tenant sécurisé
- **VDI** : virtualisation postes de travail

**Source** : [Job1.txt][file:9]
