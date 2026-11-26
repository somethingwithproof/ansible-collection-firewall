<!-- translations:start -->
**Languages:** [English](../../../README.md) · [Deutsch](../de/README.md) · **Español** · [Français](../fr/README.md) · [Português (BR)](../pt-BR/README.md) · [Русский](../ru/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md)

<details>
<summary>All 45 languages (Tier 1, Africa, Asia)</summary>

- **Tier 1:** [English](../../../README.md) · [Deutsch](../de/README.md) · **Español** · [Français](../fr/README.md) · [Italiano](../it/README.md) · [Português (BR)](../pt-BR/README.md) · [Nederlands](../nl/README.md) · [Русский](../ru/README.md) · [Українська](../uk/README.md) · [日本語](../ja/README.md) · [한국어](../ko/README.md) · [简体中文](../zh-CN/README.md) · [繁體中文](../zh-TW/README.md) · [العربية](../ar/README.md) · [हिन्दी](../hi/README.md) · [Polski](../pl/README.md) · [Türkçe](../tr/README.md) · [Svenska](../sv/README.md) · [Tiếng Việt](../vi/README.md) · [Bahasa Indonesia](../id/README.md)
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

Una colección de Ansible para describir la política de firewall del host y aplicar la configuración de nftables. Los grupos de direcciones mantienen la política separada del conjunto de reglas generado.

## Implementación y límites

- [firewall](../../../roles/firewall/tasks/main.yml) gestiona los valores predeterminados públicos y la validación del backend; [policy](../../../roles/policy/tasks/main.yml) conserva el punto de entrada heredado importándolo.
- [nftables](../../../roles/nftables/tasks/main.yml) instala nftables, prepara la configuración, la valida con `nft -c -f` y la aplica con `nft -f`.
- **`nftables_backup: true`** — El bloque de aplicación intenta restaurar una configuración anterior tras un fallo cuando existía una previa. La disponibilidad de copias de seguridad y el acceso de recuperación siguen siendo requisitos operativos.
- La [plantilla](../../../roles/nftables/templates/nftables.conf.j2) y los [valores predeterminados](../../../roles/firewall/defaults/main.yml) definen los campos de política admitidos.

La colección empaqueta `firewall`, `policy` y `nftables`; solo `nftables` implementa un backend de firewall. La validación de políticas acepta los nombres de backend para iptables y firewalld, pero aquí no se incluyen los roles de backend correspondientes. Una regla SSH en la plantilla no garantiza que no se produzca un bloqueo de acceso. El modo de solo validación sigue instalando paquetes y ejecutando tareas de configuración/servicio; no es una ejecución de prueba sin efectos secundarios.

## Inicio rápido

Compile e instale la colección desde esta copia local:

```bash
ansible-galaxy collection build
ansible-galaxy collection install ./thomasvincent-firewall-0.1.0.tar.gz
```

El nombre del archivo sigue la versión definida en [galaxy.yml](../../../galaxy.yml). Use el nombre de archivo generado por la compilación si dicha versión cambia.

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
        office: ["203.0.113.0/24"]  # Sustituya esta red por la de su oficina (CIDR)
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

Revise la política generada en un host de pruebas antes de aplicarla a hosts remotos. Mantenga una vía de recuperación independiente al modificar la política de firewall.

## Desarrollo

El [flujo de trabajo de CI](../../../.github/workflows/ci.yml) y los [escenarios de Molecule](../../../molecule) describen la validación configurada. Su presencia no garantiza un despliegue exitoso en todas las distribuciones.

## Cumplimiento y licencia

El README anterior hacía referencia a un documento de mapeo de cumplimiento que no está presente en esta copia local. Aquí no se afirma ninguna certificación ni cumplimiento de marcos normativos. La colección utiliza la [licencia MIT](../../../LICENSE); [galaxy.yml](../../../galaxy.yml) registra los metadatos del paquete.

## Compatibilidad con sistemas operativos

El backend de nftables se prueba con Ansible core 2.21 en Ubuntu 22.04,
24.04 y 26.04 LTS, Debian 12 y 13, y Rocky Linux 9 y 10. Use un controlador
mantenido con core 2.20 o superior para Python 3.14 de Ubuntu 26.04.

Molecule comprueba la convergencia, una segunda ejecución sin cambios, el conjunto de reglas cargado,
una regla de permiso para el puerto SSH 22, los permisos de configuración y la reversión tras un fallo de aplicación
inyectado. Enterprise Linux utiliza `/etc/sysconfig/nftables.conf`; Debian y
Ubuntu utilizan `/etc/nftables.conf`. `nftables_conf_path` sigue siendo configurable.
Configure el servicio nativo de nftables para cargar cualquier ruta personalizada antes de usar
esa sobrescritura: el rol reinicia el servicio, que de otro modo carga su archivo
predeterminado y puede reemplazar el conjunto de reglas previsto.

En Enterprise Linux, un archivo heredado `/etc/nftables.conf` diferente detiene el rol
antes de la promoción. Haga una copia de seguridad de ambos archivos y luego mueva la configuración previa
validada a la ruta nativa del servicio. Como alternativa, conserve explícitamente
la ruta heredada y configure el servicio en consecuencia. No deje una copia obsoleta
en `/etc/nftables.conf`: un archivo diferente en esa ubicación activa la protección en
ejecuciones posteriores. Las reglas anteriores permanecen disponibles en la ruta seleccionada como
origen de reversión durante una actualización.

Rocky Linux valida la ruta de paquetes y servicios de Enterprise Linux. Estas
pruebas en contenedores no certifican suscripciones de RHEL ni un kernel de producción.
Ubuntu 20.04 sin soporte extendido, Debian 11 y la versión anterior de Python en EL8 están
fuera de la matriz de pruebas actual.
