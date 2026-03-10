---
title: "Using ipv6 in cloud/fr"
url: "https://docs.alliancecan.ca/wiki/Using_ipv6_in_cloud/fr"
category: "General"
last_modified: "2023-02-27T21:10:01Z"
page_id: 20577
display_title: "Utiliser IPv6 dans un nuage"
language: "fr"
---

== IPv6 avec Arbutus ==
Les adresses LLA (Link-Local) et GUA (Global Unicast Addresses) sont généralement disponibles dans l'environnement infonuagique d'Arbutus.
Les adresses GUA peuvent être configurées via une interface distincte, qui à son tour ne s'occupe que du trafic IPv6.
Les adresses sont configurées par SLAAC (Stateless Address Auto Configuration), qui configure automatiquement l'IP sur l'interface de l'instance. Par défaut, les règles des groupes de sécurité permettent le trafic sortant de l'instance via le GUA de IPv6, mais rien en provenance de l'extérieur de l'instance ne sera permis jusqu'à ce que des règles spécifiques soient définies.  Ceci est le même comportement qu'avec IPv4.

=== Exemple d'une configuration openstack cli ===

Obtenez l'ID de l'instance pour attacher l'interface réseau.

openstack server list
+--------------------------------------+-----------------+---------+-----------------------------------------------+----------------------------------+----------+
| ID                                   | Name            | Status  | Networks                                      | Image                            | Flavor   |
+--------------------------------------+-----------------+---------+-----------------------------------------------+----------------------------------+----------+
| 74be352d-19ca-46cc-9661-7088d2652e34 | test            | ACTIVE  | def-bott-network=192.168.27.140, 206.12.93.29 | Debian-10.9.2-Buster-x64-2021-05 | p1-1.5gb |
+--------------------------------------+-----------------+---------+-----------------------------------------------+----------------------------------+----------+

Assignez une nouvelle interface réseau à l'instance avec IPv6 comme réseau.

openstack server add network 74be352d-19ca-46cc-9661-7088d2652e34  IPv6-GUA

Vérifiez l'état de l'assignation.

openstack server list
+--------------------------------------+-----------------+---------+------------------------------------------------------------------------------------------------+----------------------------------+----------+
| ID                                   | Name            | Status  | Networks                                                                                       | Image                            | Flavor   |
+--------------------------------------+-----------------+---------+------------------------------------------------------------------------------------------------+----------------------------------+----------+
| 74be352d-19ca-46cc-9661-7088d2652e34 | test            | ACTIVE  | IPv6-GUA=2607:f8f0:c11:7004:f816:3eff:fef1:8cee; def-bott-network=192.168.27.140, 206.12.93.29 | Debian-10.9.2-Buster-x64-2021-05 | p1-1.5gb |
+--------------------------------------+-----------------+---------+------------------------------------------------------------------------------------------------+----------------------------------+----------+

=== Exemple d'une configuration Webinterface ===
Connectez-vous au tableau de bord. Dans le menu Instances, cliquez sur Attacher l'interface pour faire afficher la boîte de dialogue.
Dans le champ Réseau, sélectionnez IPv6-GUA (2607:f8f0:c11:7004::/64) et cliquez sur Attacher l'interface.

Instancemenu.png|1. Tableau de bord montrant les instances
Interface menu attach.png.png|2. Menu déroulant pour attacher interface
netlist.png|3. Réseaux disponibles
show_attached.png| 4. Sélectionner l'interface IPv6

L'adresse IPv6 restera disponible jusqu'à ce qu'elle soit détachée. Chaque fois que l'interface est détachée, l'adresse GUA est libérée et elle retourne dans le pool; elle peut ainsi être utilisée par les autres. Toutefois, le fait de reconstruire ou de redémarrer l'instance ne libère pas l'adresse GUA.

L'accès à partir d'une adresse GUA peut être autorisé par la fonction Groupes de sécurité de OpenStack, à l'exception de  CIDR qui détecte autpomatiquement le type d'une adresse.

== Exemple d'une configuration Linux ==

Le réseau OpenStack que vous avez configuré ci-dessus apparaîtra sous Linux comme une interface de type eth. Dans la plupart des cas, votre interface courante sera /dev/eth0 et votre nouvelle interface IPv6 activée sera /dev/eth1. Le moyen le plus facile d'attraper votre nouveau périphérique est de redémarrer l'ordinateur. Mais auparavant, vérifiez que IPv6 est activée avec la commande
sudo sysctl -a | grep ipv6.*disable

Les résultats devraient tous se terminer par des zéros. L'IPv6 activée est l'interface par défaut pour toutes les images récentes.  Les paramètres qui doivent être changés pour des zéros devraient être ajoutés à /etc/sysctl.conf.

Ajoutez aussi les paramètres de noyau suivants dans /etc/sysctl.conf.

net.ipv6.conf.eth1.forwarding=0
net.ipv6.conf.eth1.accept_ra=1

Redémarrez votre système et confirmez que IPv6 est activé et que /dev/eth1 existe.

Ajoutez à /etc/sysconfig/network-scripts/ifcfg-eth1  les configurations suivantes :

IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no

Redémarrez votre système à nouveau; l'interface /dev/eth1 devrait être configurée et prête à être utilisée.

Vous pouvez maintenant confirmer la configuration IPv6 avec la commande

$ ip -6 address

Confirmez que IPv6 fonctionne avec la commande

$ ping6 -c 1 www.google.com

Votre système est maintenant configuré pour utiliser IPv6.

==Plus d'information==
* de RedHat, What you need to know about IPv6
* de RedHat, Configuring an IPv6 address in Red Hat Enterprise Linux 7 and 8
* de OpenStack, IPv6