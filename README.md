# Proxmox-Ajout-Volume-LVM

## Les étapes.


1. Je créé un disque dur SATA dans virtualbox de 80G

![[Ajout-volume-VLM-2.png]](Ressources/Ajout-volume-VLM-2.png)

2. Verification dans la fenetre shell de proxmox que le disque est bien présent avec un "lsblk"

![[Ajout-volume-VLM-3.png]](Ressources/Ajout-volume-VLM-3.png)

3. Dans le shell toujours, je créé le volume logique sdb

![[Ajout-volume-VLM-4.png]](Ressources/Ajout-volume-VLM-4.png)

4. Création du volume physique avec `pvcreate /dev/sdb` :

![[Ajout-volume-VLM-5.png]](Ressources/Ajout-volume-VLM-5.png)

5. Vérification de la création du volume avec `pvdisplay /dev/sdb` :

![[Ajout-volume-VLM-6.png]](Ressources/Ajout-volume-VLM-6.png)

6. Création du volume group `vg1` avec la commande `vgcreate vg1 /dev/sdb` :

![[Ajout-volume-VLM-7.png]](Ressources/Ajout-volume-VLM-7.png)

7. Création du volume logique `vl-dd2` avec la commande `lvcreate -n vl-dd2 -L 70G vg1` de 70G

![[Ajout-volume-VLM-8.png]](Ressources/Ajout-volume-VLM-8.png)

8. Formatage en ext4 du volume logique avec la commande `mkfs.ext4` :

![[Ajout-volume-VLM-9.png]](Ressources/Ajout-volume-VLM-9.png)

9. Conversion du volume logique au format **thin-pool** avec la commande `lvconvert --type thin-pool vg1/vl-dd2`

![[Ajout-volume-VLM-10.png]](Ressources/Ajout-volume-VLM-10.png)

10. Configuration dans proxmox du volume dans pve -> disk :

Je l'ajoute en selectionnant "LVM-thin"

![[Ajout-volume-VLM-11.png]](Ressources/Ajout-volume-VLM-11.png)

11. Il est bien présent dans "pve" -> "datacenter" -> "Storage"

![[Ajout-volume-VLM-12.png]](Ressources/Ajout-volume-VLM-12.png)

