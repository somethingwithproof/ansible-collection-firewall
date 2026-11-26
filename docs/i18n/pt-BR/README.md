<!-- translations:start -->
**Languages:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · **Português (BR)** · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · [Español](../es/README.md) · [Français](../fr/README.md) · [Italiano](../it/README.md) · **Português (BR)** · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

Uma coleção Ansible para descrever políticas de firewall de host e aplicar configurações do nftables. Grupos de endereços mantêm a política separada do conjunto de regras gerado.

## Implementação e limites

- [firewall](../../../roles/firewall/tasks/main.yml) define os valores padrão públicos e a validação de backend; [policy](../../../roles/policy/tasks/main.yml) preserva o ponto de entrada legado importando-a.
- [nftables](../../../roles/nftables/tasks/main.yml) instala o nftables, prepara a configuração em área temporária, valida-a com `nft -c -f` e aplica-a com `nft -f`.
- **`nftables_backup: true`** — O bloco de aplicação tenta restaurar uma configuração anterior após uma falha, quando ela existir. A disponibilidade de backups e o acesso de recuperação continuam sendo requisitos operacionais.
- O [template](../../../roles/nftables/templates/nftables.conf.j2) e os [valores padrão](../../../roles/firewall/defaults/main.yml) definem os campos de política suportados.

A coleção empacota `firewall`, `policy` e `nftables`; apenas `nftables` implementa um backend de firewall. Os nomes de backend para iptables e firewalld são aceitos pela validação de política, mas nenhuma role de backend correspondente é distribuída aqui. Uma regra SSH no template não garante proteção contra bloqueio de acesso. O modo somente validação ainda instala pacotes e executa tarefas de configuração/serviço; não se trata de uma simulação (dry run) livre de efeitos colaterais.

## Início rápido

Compile e instale a coleção a partir deste checkout:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

O nome do arquivo segue a versão definida em [galaxy.yml](../../../galaxy.yml). Use o nome do arquivo produzido pelo build caso essa versão mude.

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
        office: ["203.0.113.0/24"]  # Substitua pela rede do seu escritório (CIDR)
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

Revise a política gerada em um host de teste antes de aplicá-la a hosts remotos. Mantenha um caminho de recuperação independente ao alterar a política de firewall.

## Desenvolvimento

O [workflow de CI](../../../.github/workflows/ci.yml) e os [cenários do Molecule](../../../molecule) descrevem a validação configurada. A presença deles não garante uma implantação bem-sucedida em todas as distribuições.

## Conformidade e licença

O README anterior referenciava um documento de mapeamento de conformidade que não está presente neste checkout. Nenhuma certificação ou conformidade com frameworks é reivindicada aqui. A coleção utiliza a [licença MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) registra os metadados do pacote.

## Suporte a sistemas operacionais

O backend nftables é testado com o Ansible core 2.21 no Ubuntu 22.04,
24.04 e 26.04 LTS, Debian 12 e 13 e Rocky Linux 9 e 10. Use um
controlador mantido com core 2.20 ou mais recente para o Python 3.14 do Ubuntu 26.04.

O Molecule verifica a convergência, uma segunda execução sem alterações, o conjunto de regras carregado,
uma regra de permissão para a porta SSH 22, as permissões de configuração e o rollback após uma falha de aplicação
injetada. O Enterprise Linux usa `/etc/sysconfig/nftables.conf`; o Debian e
o Ubuntu usam `/etc/nftables.conf`. `nftables_conf_path` permanece configurável.
Configure o serviço nativo do nftables para carregar qualquer caminho personalizado antes de usar
essa substituição: a role reinicia o serviço, que caso contrário carrega seu arquivo
padrão e pode substituir o conjunto de regras pretendido.

No Enterprise Linux, um arquivo legado `/etc/nftables.conf` divergente interrompe a role
antes da promoção. Faça backup de ambos os arquivos e mova a configuração anterior
validada para o caminho nativo do serviço. Alternativamente, mantenha explicitamente
o caminho legado e configure o serviço de acordo. Não deixe uma cópia obsoleta
em `/etc/nftables.conf`: um arquivo divergente nesse local aciona a trava de segurança nas
execuções seguintes. As regras anteriores permanecem disponíveis no caminho selecionado como
origem de rollback durante uma atualização.

O Rocky Linux exercita o caminho de pacotes e serviços do Enterprise Linux. Esses
testes em contêiner não certificam assinaturas RHEL nem um kernel de produção.
O Ubuntu 20.04 sem suporte estendido, o Debian 11 e o Python mais antigo do EL8 estão
fora da matriz de testes atual.
