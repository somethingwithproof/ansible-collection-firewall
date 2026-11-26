<!-- translations:start -->
**Languages:** [English](../../../README.md) · **Italiano** · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · **Italiano** · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
- **Africa:** [Afrikaans](../af-ZA/README.md) · [አማርኛ](../am-ET/README.md) · [العربية (مصر)](../ar-EG/README.md) · [العربية (المغرب)](../ar-MA/README.md) · [Français (Maroc)](../fr-MA/README.md) · [Français (Sénégal)](../fr-SN/README.md) · [Hausa](../ha-NG/README.md) · [Igbo](../ig-NG/README.md) · [Malagasy](../mg-MG/README.md) · [Kinyarwanda](../rw-RW/README.md) · [Soomaali](../so-SO/README.md) · [Kiswahili](../sw-KE/README.md) · [isiXhosa](../xh-ZA/README.md) · [Yorùbá](../yo-NG/README.md) · [isiZulu](../zu-ZA/README.md)
- **Asia:** [বাংলা](../bn/README.md) · [فارسی](../fa/README.md) · [עברית](../he/README.md) · [मराठी](../mr/README.md) · [தமிழ்](../ta/README.md) · [తెలుగు](../te/README.md) · [ไทย](../th/README.md) · [اردو](../ur/README.md) · [Bahasa Melayu](../ms/README.md) · [Filipino](../fil/README.md)

</details>
<!-- translations:end -->

# thomasvincent.firewall

[![CI](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/somethingwithproof/ansible-collection-firewall/actions/workflows/ci.yml)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=somethingwithproof_ansible-collection-firewall&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=somethingwithproof_ansible-collection-firewall)
[![Release](https://img.shields.io/github/v/release/somethingwithproof/ansible-collection-firewall)](https://github.com/somethingwithproof/ansible-collection-firewall/releases)
[![License](https://img.shields.io/badge/license-MIT-blue)](../../../LICENSE)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/somethingwithproof/ansible-collection-firewall/badge)](https://scorecard.dev/viewer/?uri=github.com/somethingwithproof/ansible-collection-firewall)

Una collection Ansible per descrivere la policy del firewall dell'host e applicare la configurazione di nftables. I gruppi di indirizzi mantengono la policy separata dal ruleset generato.

## Implementazione e limiti

- [firewall](../../../roles/firewall/tasks/main.yml) gestisce i valori predefiniti pubblici e la validazione del backend; [policy](../../../roles/policy/tasks/main.yml) preserva il punto di ingresso legacy importandolo.
- [nftables](../../../roles/nftables/tasks/main.yml) installa nftables, prepara la configurazione, la valida con `nft -c -f` e la applica con `nft -f`.
- Il blocco di applicazione tenta di ripristinare una configurazione precedente in caso di errore, qualora ne esistesse una. La disponibilità di backup e un accesso di ripristino restano requisiti operativi.
- Il [template](../../../roles/nftables/templates/nftables.conf.j2) e i [valori predefiniti](../../../roles/policy/defaults/main.yml) definiscono i campi di policy supportati.

La collection include `firewall`, `policy` e `nftables`; solo `nftables` implementa un backend firewall. I nomi di backend per iptables e firewalld sono accettati dalla validazione della policy, ma i relativi ruoli backend non sono inclusi qui. Una regola SSH nel template non garantisce di evitare il blocco dell'accesso (lockout). La modalità di sola validazione installa comunque i pacchetti ed esegue attività di configurazione e di servizio; non è un'esecuzione simulata (dry run) priva di effetti collaterali.

## Avvio rapido

Compila e installa la collection da questo checkout:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

Il nome del file segue la versione in [galaxy.yml](../../../galaxy.yml). Usa il nome del file prodotto dalla build se tale versione cambia.

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

Esamina la policy generata su un host di test prima di applicarla agli host remoti. Mantieni un percorso di ripristino indipendente quando modifichi la policy del firewall.

## Sviluppo

Il [workflow CI](../../../.github/workflows/ci.yml) e gli [scenari Molecule](../../../molecule) descrivono la validazione configurata. La loro presenza non garantisce un deployment riuscito su ogni distribuzione.

## Conformità e licenza

Il README precedente faceva riferimento a un documento di mappatura della conformità che non è presente in questo checkout. Qui non viene rivendicata alcuna certificazione né conformità a framework. La collection usa la [licenza MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) registra i metadati del pacchetto.

## Supporto dei sistemi operativi

Il backend nftables è testato con Ansible core 2.21 su Ubuntu 22.04,
24.04 e 26.04 LTS, Debian 12 e 13, e Rocky Linux 9 e 10. Usa un
controller supportato con core 2.20 o versione successiva per Python 3.14 su Ubuntu 26.04.

Molecule verifica la convergenza, una seconda esecuzione senza modifiche, il ruleset caricato,
una regola di autorizzazione SSH sulla porta 22, i permessi di configurazione e il rollback dopo un errore di applicazione
iniettato. Enterprise Linux utilizza `/etc/sysconfig/nftables.conf`; Debian e
Ubuntu utilizzano `/etc/nftables.conf`. `nftables_conf_path` rimane configurabile.
Configura il servizio nftables nativo per caricare eventuali percorsi personalizzati prima di utilizzare
tale override: il ruolo riavvia il servizio, che altrimenti carica il proprio file
predefinito e può sostituire il ruleset previsto.

Su Enterprise Linux, un file legacy `/etc/nftables.conf` diverso interrompe il ruolo
prima della promozione. Esegui il backup di entrambi i file, quindi sposta la configurazione
precedente validata nel percorso nativo del servizio. In alternativa, mantieni esplicitamente
il percorso legacy e configura il servizio di conseguenza. Non lasciare una copia
obsoleta in `/etc/nftables.conf`: un file diverso in quella posizione attiva il controllo di sicurezza
nelle esecuzioni successive. Le regole precedenti rimangono disponibili nel percorso selezionato come
sorgente di rollback durante un aggiornamento.

Rocky Linux esercita il percorso dei pacchetti e dei servizi di Enterprise Linux. Questi
test in container non certificano sottoscrizioni RHEL né un kernel di produzione.
Ubuntu 20.04 senza supporto esteso, Debian 11 e la versione precedente di Python su EL8 sono
al di fuori dell'attuale matrice di test.
