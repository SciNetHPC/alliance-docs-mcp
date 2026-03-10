---
title: "Using cloud vGPUs/fr"
url: "https://docs.alliancecan.ca/wiki/Using_cloud_vGPUs/fr"
category: "General"
last_modified: "2025-01-21T21:16:07Z"
page_id: 17584
display_title: "Utilisation de vGPU dans le cloud"
language: "fr"
---

Cette page décrit comment
* allouer des ressources de GPU virtuel (vGPU) à une machine virtuelle (VM),
* installer les pilotes nécessaires et
* vérifier si le vGPU peut être utilisé.
L'accès aux dépôts de données ainsi qu'aux vGPU n'est actuellement disponible que sur le nuage Arbutus. Veuillez noter que la documentation ci-dessous ne couvre que l'installation du pilote vGPU. La boîte à outils CUDA n'est pas préinstallée mais vous pouvez l'installer directement à partir de NVIDIA ou la charger de la pile logicielle dans CVMFS.
Si vous choisissez d'installer la boîte à outils directement de NVIDIA, assurez-vous que le pilote vGPU n'est pas écrasé par celui de CUDA.

== Gabarits pris en charge ==

Pour utiliser un vGPU dans une machine virtuelle, l'instance doit être déployée sur un des gabarits mentionnés ci-dessous. Le vGPU sera disponible pour le système d'exploitation via le bus PCI.

* g1-8gb-c4-22gb
* g1-16gb-c8-40gb

== Préparer une machine virtuelle sous Almalinux9 ==

Une fois que la machine virtuelle est disponible, assurez-vous de mettre à jour le système d'exploitation avec la dernière version disponible, y compris le noyau (kernel).
Redémarrez ensuite la machine virtuelle pour exécuter le dernier noyau.

Pour avoir accès à DKMS, le dépôt EPEL est requis.

AlmaLinux 9 possède par défaut le pilote nouveau défectueux qui fait planter le noyau dès que le pilote nvidia est monté.
Nous avons besoin de quelques étapes supplémentaires pour empêcher le chargement du pilote nouveau au démarrage du système.

[root@almalinux9]# echo -e "blacklist nouveau\noptions nouveau modeset=0" >/etc/modprobe.d/blacklist-nouveau.conf
[root@almalinux9]# dracut -fv --omit-drivers nouveau
[root@almalinux9]# dnf -y update && dnf -y install epel-release && reboot

Après le redémarrage de la machine virtuelle, le dépôt arbutus-cloud-vgpu doit être installé.

[root@almalinux9]# dnf install http://repo.arbutus.cloud.computecanada.ca/pulp/repos/alma9/Packages/a/arbutus-cloud-vgpu-repo-1.0-1.el9.noarch.rpm

L’étape suivante consiste à installer les paquets vGPU pour installer le pilote requis.

[root@almalinux9]# dnf -y install nvidia-vgpu-gridd.x86_64 nvidia-vgpu-tools.x86_64 nvidia-vgpu-kmod.x86_64

Si votre installation réussit, vous pouvez utiliser nvidia-smi pour vérifier le bon fonctionnement.

[root@almalinux9]# nvidia-smi
Tue Apr 23 16:37:31 2024
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.15              Driver Version: 550.54.15      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  GRID V100D-8C                  On  |   00000000:00:06.0 Off |                    0 |
| N/A   N/A    P0             N/A /  N/A  |       0MiB /   8192MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+

== Préparer une machine virtuelle sous Almalinux9 ==

Une fois que la machine virtuelle est disponible, assurez-vous de mettre à jour le système d'exploitation avec la dernière version disponible, y compris le noyau (kernel). Redémarrez ensuite la machine virtuelle pour exécuter le dernier noyau.
Pour avoir accès à DKMS, le dépôt EPEL est requis.

[root@vgpu almalinux]# dnf -y update && dnf -y install epel-release && reboot

Après le redémarrage de la machine virtuelle, le répertoire arbutus-cloud-vgpu doit être installé.

[root@almalinux8]# dnf install http://repo.arbutus.cloud.computecanada.ca/pulp/repos/alma8/Packages/a/arbutus-cloud-vgpu-repo-1.0-1.el8.noarch.rpm

L’étape suivante consiste à installer les paquets vGPU pour installer le pilote requis.

[root@vgpu almalinux]# dnf -y install nvidia-vgpu-gridd.x86_64 nvidia-vgpu-tools.x86_64 nvidia-vgpu-kmod.x86_64

Si votre installation réussit, vous pouvez utiliser nvidia-smi pour vérifier le bon fonctionnement.

[root@almalinux8]# nvidia-smi
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.15              Driver Version: 550.54.15      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  GRID V100D-8C                  On  |   00000000:00:06.0 Off |                    0 |
| N/A   N/A    P0             N/A /  N/A  |       0MiB /   8192MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+

== Préparer une machine virtuelle sous Debian11 ==
Assurez-vous que les paquets les plus récents sont installés et que le système a été démarré avec le noyau stable le plus récent puisque DKMS exigera le dernier disponible dans les dépôts de données Debian.

root@debian11:~# apt-get update && apt-get -y dist-upgrade && reboot

Une fois que la machine virtuelle est redémarrée, le système devrait avoir le plus récent  noyau disponible et le dépôt peut être installé en installant le paquet arbutus-cloud-repo. Ce paquet contient la clé gpg qui signe tous les paquets.

root@debian11:~# wget http://repo.arbutus.cloud.computecanada.ca/pulp/deb/deb11/pool/main/arbutus-cloud-repo_0.1_all.deb
root@debian11:~# apt-get install -y ./arbutus-cloud-repo_0.1_all.deb

Faites la mise à jour de la cache apt locale et installez les paquets vGPU.

root@debian11:~# apt-get update && apt-get -y install nvidia-vgpu-kmod nvidia-vgpu-tools nvidia-vgpu-gridd

root@debian11:~# nvidia-smi
Tue Apr 23 18:55:18 2024
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.15              Driver Version: 550.54.15      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  GRID V100D-8C                  On  |   00000000:00:06.0 Off |                    0 |
| N/A   N/A    P0             N/A /  N/A  |       0MiB /   8192MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+

== Préparer une machine virtuelle sous Debian12 ==
Assurez-vous que les paquets les plus récents sont installés et que le système a été démarré avec le noyau stable le plus récent puisque DKMS exigera le dernier disponible dans les dépôts de données Debian.

root@debian12:~# apt-get update && apt-get -y dist-upgrade && reboot

Après un redémarrage réussi, le système devrait fonctionner avec le dernier noyau disponible. Le dépôt de données peut maintenant être installé en installant le paquet arbutus-cloud-repo qui contient aussi la clé gpg pour tous les paquets.

root@debian12:~# wget http://repo.arbutus.cloud.computecanada.ca/pulp/deb/deb12/pool/main/arbutus-cloud-repo_0.1+deb12_all.deb
root@debian12:~# apt-get install -y ./arbutus-cloud-repo_0.1+deb12_all.deb

Faites la mise à jour de la cache apt locale et installez les paquets vGPU.

root@debian12:~# apt-get update && apt-get -y install nvidia-vgpu-kmod nvidia-vgpu-tools nvidia-vgpu-gridd

root@debian12:~# nvidia-smi
Tue Apr 23 18:55:18 2024
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.15              Driver Version: 550.54.15      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  GRID V100D-8C                  On  |   00000000:00:06.0 Off |                    0 |
| N/A   N/A    P0             N/A /  N/A  |       0MiB /   8192MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+

== Préparer une machine virtuelle sous Ubuntu22 ==
Assurez-vous que le système d'exploitation est à jour, que toutes les rustines (patches) les plus récentes sont installées et que le dernier noyau (kernel) stable est en cours d'exécution.

root@ubuntu22:~# apt-get update && apt-get -y dist-upgrade && reboot

Après un redémarrage réussi, le système devrait fonctionner avec le dernier noyau disponible. Le dépôt de données peut maintenant être installé en installant le paquet arbutus-cloud-repo qui contient aussi la clé gpg pour tous les paquets.

root@ubuntu22:~# wget http://repo.arbutus.cloud.computecanada.ca/pulp/deb/ubnt22/pool/main/arbutus-cloud-repo_0.1_all.deb
root@ubuntu22:~# apt-get install ./arbutus-cloud-repo_0.1_all.deb

Faites la mise à jour de la cache apt locale et installez les paquets vGPU.

root@ubuntu22:~# apt-get update && apt-get -y install nvidia-vgpu-kmod nvidia-vgpu-tools nvidia-vgpu-gridd

Si l'installation est réussie, le vGPU sera accessible et la licence sera validée.

root@ubuntu22:~# nvidia-smi
Wed Apr 24 14:37:52 2024
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.15              Driver Version: 550.54.15      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  GRID V100D-8C                  On  |   00000000:00:06.0 Off |                    0 |
| N/A   N/A    P0             N/A /  N/A  |       0MiB /   8192MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+

== Préparer une machine virtuelle sous Ubuntu20 ==
Assurez-vous que le système d'exploitation est à jour, que toutes les rustines (patches) les plus récentes sont installées et que le dernier noyau (kernel) stable est en cours d'exécution.

root@ubuntu20:~# apt-get update && apt-get -y dist-upgrade && reboot

Après un redémarrage réussi, le système devrait fonctionner avec le dernier noyau disponible. Le dépôt de données peut maintenant être installé en installant le paquet arbutus-cloud-repo qui contient aussi la clé gpg pour tous les paquets.

root@ubuntu20:~# wget http://repo.arbutus.cloud.computecanada.ca/pulp/deb/ubnt20/pool/main/arbutus-cloud-repo_0.1ubuntu20_all.deb
root@ubuntu20:~# apt-get install ./arbutus-cloud-repo_0.1ubuntu20_all.deb

Faites la mise à jour de la cache apt locale et installez les paquets vGPU.

root@ubuntu20:~# apt-get update && apt-get -y install nvidia-vgpu-kmod nvidia-vgpu-tools nvidia-vgpu-gridd

Si l'installation est réussie, le vGPU sera accessible et la licence sera validée.

root@ubuntu20:~# nvidia-smi
Wed Apr 24 14:37:52 2024
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.54.15              Driver Version: 550.54.15      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  GRID V100D-8C                  On  |   00000000:00:06.0 Off |                    0 |
| N/A   N/A    P0             N/A /  N/A  |       0MiB /   8192MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+