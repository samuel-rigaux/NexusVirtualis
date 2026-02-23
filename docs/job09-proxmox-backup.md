# Job 09 - Proxmox Backup Server[file:1]

## Configuration automatisée[file:1]

Fréquence : Toutes les 2h

Rétention : 3 dernières sauvegardes

Cible : VM Debian Proxmox


**Étapes** :
Installer PBS (VM/conteneur)

Datacenter → Storage → Add PBS

Job backup : */2 * * * * (cron)

Retention : keep-last 3


