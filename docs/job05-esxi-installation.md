# Job 05 - VMware ESXi 8.0[file:6]

## Spécifications VM Serveur[file:6]

RAM : 8 Go

CPU : 2 x 2 cœurs (virtualisation ✓)

Stockage : 30 Go + 142 Go secondaire


## Installation[file:6]

1. **Télécharger** ESXi depuis Broadcom[file:6]
2. **Problème résolu** : Décocher "Plateforme d'ordinateur virtuel" Windows[file:6]
3. Mot de passe root : `Admin123?`[file:6]
4. IP gestion : `https://192.168.29.131/`[file:6]

## Datastore + VM Debian[file:6]
Stockage → Nouvelle datastore (disque 142 Go)
VM → 2 vCPU, 1 Go RAM, 8 Go disque
DVD → ISO Debian depuis datastore

