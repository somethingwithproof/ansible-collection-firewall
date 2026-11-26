<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Français (Sénégal)** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · **Français (Sénégal)** · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Une collection Ansible permettant de décrire la politique de pare-feu d'un hôte et d'appliquer une configuration nftables. Les groupes d'adresses permettent de dissocier la politique du jeu de règles généré.

## Implémentation et limites

- [firewall](../../../roles/firewall/tasks/main.yml) gère les valeurs par défaut publiques et la validation du backend ; [policy](../../../roles/policy/tasks/main.yml) préserve l'ancien point d'entrée en l'important.
- [nftables](../../../roles/nftables/tasks/main.yml) installe nftables, prépare la configuration, la valide avec `nft -c -f` et l'applique avec `nft -f`.
- Le bloc d'application tente de restaurer une configuration précédente après un échec lorsqu'il en existait une. La disponibilité des sauvegardes et d'un accès de récupération demeure une exigence opérationnelle.
- Le [modèle](../../../roles/nftables/templates/nftables.conf.j2) et les [valeurs par défaut](../../../roles/policy/defaults/main.yml) définissent les champs de politique pris en charge.

La collection intègre `firewall`, `policy` et `nftables` ; seul `nftables` implémente un backend de pare-feu. Les noms de backend pour iptables et firewalld sont acceptés lors de la validation de la politique, mais aucun rôle backend correspondant n'est fourni ici. Une règle SSH dans le modèle ne garantit pas l'absence de verrouillage d'accès. Le mode validation uniquement installe tout de même les paquets et exécute des tâches de configuration et de service ; il ne s'agit pas d'une exécution à blanc sans effets de bord.

## Démarrage rapide

Compilez et installez la collection à partir de cette copie locale :

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Le nom du fichier suit la version indiquée dans [galaxy.yml](../../../galaxy.yml). Utilisez le nom de fichier généré par la compilation si cette version change.

```yaml
- hosts: firewall_test_hosts
  become: true
  vars:
    firewall_backend: nftables
    firewall_defaults:
      policy_v4: drop
      policy_v6: drop
      allow_loopback: true
      allow_established: true
      drop_invalid: true
      log_drops: true
      ssh_guard: true
      ssh_ports: [22]
    firewall_objects:
      address_groups:
        office: ["203.0.113.0/24"]  # TEST-NET-3 placeholder; replace with your office CIDR
      port_groups: {}
    firewall_rules:
      - name: office-https
        direction: inbound
        source_group: office
        dest_port: 443
        action: accept
  roles:
    - thomasvincent.firewall.policy
    - thomasvincent.firewall.nftables
```

Passez en revue la politique générée sur un hôte de test avant de l'appliquer à des hôtes distants. Conservez une voie de récupération indépendante lors de toute modification de la politique de pare-feu.

## Développement

Le [workflow CI](../../../.github/workflows/ci.yml) et les [scénarios Molecule](../../../molecule) décrivent la validation configurée. Leur présence ne garantit pas un déploiement réussi sur chaque distribution.

## Conformité et licence

La version précédente du README faisait référence à un document de correspondance de conformité qui n'est pas présent dans ce dépôt. Aucune certification ni conformité à un référentiel n'est revendiquée ici. La collection est distribuée sous la [licence MIT](../../../LICENSE) ; le fichier [galaxy.yml](../../../galaxy.yml) consigne les métadonnées du paquet.

## Systèmes d'exploitation pris en charge

Le backend nftables est testé avec Ansible core 2.21 sur Ubuntu 22.04, 24.04 et 26.04 LTS, Debian 12 et 13, ainsi que Rocky Linux 9 et 10. Utilisez un contrôleur maintenu avec core 2.20 ou une version plus récente pour Python 3.14 sous Ubuntu 26.04.

Molecule vérifie la convergence, une seconde exécution sans changement, le jeu de règles chargé, une règle autorisant SSH sur le port 22, les permissions de configuration et le retour arrière après l'injection d'un échec d'application. Enterprise Linux utilise `/etc/sysconfig/nftables.conf` ; Debian et Ubuntu utilisent `/etc/nftables.conf`. `nftables_conf_path` reste configurable. Configurez le service nftables natif pour charger tout chemin personnalisé avant d'utiliser cette surcharge : le rôle redémarre le service, qui charge autrement son fichier standard et peut remplacer le jeu de règles souhaité.

Sur Enterprise Linux, un ancien fichier `/etc/nftables.conf` différent interrompt le rôle avant la mise en service. Sauvegardez les deux fichiers, puis déplacez la configuration précédente validée vers le chemin du service natif. Vous pouvez également conserver explicitement l'ancien chemin et configurer le service en conséquence. Ne laissez pas de copie obsolète dans `/etc/nftables.conf` : un fichier différent à cet emplacement déclenche le garde-fou lors des exécutions ultérieures. Les règles précédentes restent disponibles à l'emplacement sélectionné comme source de restauration lors d'une mise à niveau.

Rocky Linux permet de tester le chemin des paquets et des services d'Enterprise Linux. Ces tests en conteneur ne certifient ni les abonnements RHEL ni un noyau de production. Ubuntu 20.04 sans support étendu, Debian 11 et la version plus ancienne de Python sous EL8 sont exclus de la matrice de test actuelle.
