# Extension Local Storage XCP-ng[file:3]

## Script automatique[file:3]

```bash
#!/bin/bash
UUID=$(xe sr-list name-label="Local storage" params=uuid --minimal)
# ... (copie le script complet de extension-stockage.txt)[5]


### 4. **Dossier `scripts/`**

#### `scripts/extend-xcp-storage.sh`
```bash
#!/bin/bash
# Script auto extension Local Storage XCP-ng[file:3]

UUID=$(xe sr-list name-label="Local storage" params=uuid --minimal)
echo "UUID Local: $UUID"

lsblk -f
read -p "Confirme partition Local (ex: /dev/sda3)? [Enter]"

parted /dev/sda resizepart 3 100%
partprobe
pvresize /dev/sda3
vgdisplay VG_XenStorage-$UUID

lvextend -l +100%FREE /dev/VG_XenStorage-$UUID/
xe sr-scan uuid=$UUID
xe sr-list uuid=$UUID params=physical-size,free-space

echo "✅ Prêt pour XOA !"
bash -c "$(wget -qO- https://xoa.io/deploy)"
