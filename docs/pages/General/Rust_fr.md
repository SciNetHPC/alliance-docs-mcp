---
title: "Rust/fr"
url: "https://docs.alliancecan.ca/wiki/Rust/fr"
category: "General"
last_modified: "2025-09-09T20:17:36Z"
page_id: 22079
display_title: "Rust"
language: "fr"
---

Rust est un langage de programmation multiparadigmes, de haut niveau et à usage général. Rust met l'accent sur les performances, la sécurité des types et la simultanéité. Rust applique la sécurité de la mémoire, c'est-à-dire que toutes les références pointent vers une mémoire valide sans nécessiter l'utilisation d'un ramasse-miettes (garbage collector) ou d'un compteur de références présent dans d'autres langages sécurisés pour la mémoire.

== Module ==
Le compilateur Rust est disponible sous forme de module.

== Installer un crate ==
Un paquet écrit en Rust s'appelle un crate.

=== De Crates.io ===
1. Chargez les modules requis.

2. Construisez et installez le crate localement. Cela doit être fait à partir d'un nœud de connexion.

3. Testez le binaire.

Vous pouvez également ajouter .cargo/bin à votre $PATH avec export PATH="$HOME/.cargo/bin:$PATH".

=== D'un répertoire Git ===
1. Chargez les modules requis.

2. Construisez et installez le crate localement à partir d'un nœud de connexion.

3. Testez le binaire.

Vous pouvez aussi ajouter .cargo/bin à votre $PATH avec export PATH="$HOME/.cargo/bin:$PATH".

== Utiliser le compilateur nightly ==
Puisque certaines fonctionnalités ne sont pas encore stables, elles ne sont pas incluses dans la version stable offerte, mais sont cependant utilisées par certains crates.
Par exemple, si vous voulez utiliser la version nightly du compilateur, vous pouvez l'installer localement.

1. Installez le compilateur en tant que module local.
nightly --disable-enforce-checksums}}

2. Chargez le module local.

== Nettoyer la cache ==
Comme la cache et le registre prennent souvent beaucoup de place, vous pouvez en regagner en supprimant le registre avec