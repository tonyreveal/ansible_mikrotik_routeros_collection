# mikrotik.routeros

## Description

The `mikrotik.routeros` Ansible collection provides reusable roles for
configuring and maintaining MikroTik devices running RouterOS. It is intended
for network administrators and automation teams managing RouterOS devices
through Ansible network connections.

The collection includes focused roles for access control, backups, baseline
configuration, firewall policy, VPNs, WireGuard, routing, VLANs, DHCP, DNS,
NTP, logging, SNMP, Netwatch, traffic flow, alert destinations, compliance evidence, inventory, and RouterOS
package and RouterBOARD upgrades.

Roles expose structured variables and construct RouterOS commands internally.
Consumers provide resources and settings rather than raw command strings.

## Requirements

- Ansible Core 2.16 or newer.
- Python 3.9 or newer in the execution environment.
- `community.routeros` collection version 3.21.0.
- RouterOS devices configured for Ansible network CLI access.
- Inventory variables including:
  - `ansible_connection: ansible.netcommon.network_cli`
  - `ansible_network_os: community.routeros.routeros`
- Credentials supplied through Ansible Vault, Automation Controller credentials,
  or another approved secret-management system.

The collection uses the `community.routeros.command` module over the RouterOS
CLI. The target RouterOS release must support the command paths used by the
selected role. Test RouterOS 6 and RouterOS 7 configurations separately where
their syntax or feature sets differ.

The collection has been tested against RouterOS versions **7.23.3** and
**7.24**.

## Installation

### Red Hat Ansible Automation Hub

Configure the appropriate Automation Hub server and install the collection
using the approved content source for your organization. The collection
dependency is pinned to `community.routeros` 3.21.0.

### Ansible Galaxy or source repository

Install the collection and its dependency from Galaxy:

```text
ansible-galaxy collection install mikrotik.routeros:==1.0.0
```

For local development, clone the source repository and place it in an Ansible
collection path matching `ansible_collections/mikrotik/routeros`, then install
the dependency:

```text
ansible-galaxy collection install community.routeros:==3.21.0
```

## Usage

Configure RouterOS inventory in a reviewed inventory or group variables file:

```yaml
---
ansible_connection: ansible.netcommon.network_cli
ansible_network_os: community.routeros.routeros
...
```

Example playbook using the firewall role:

```yaml
---
- name: Configure RouterOS firewall
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: mikrotik.routeros.routeros_firewall_config
      vars:
        routeros_firewall_config_address_lists:
          - list: trusted-admins
            address: 192.0.2.10
        routeros_firewall_config_filter_rules:
          - chain: input
            options:
              action: accept
              protocol: tcp
              dst-port: 22
              src-address-list: trusted-admins
...
```

Example playbook using the WireGuard role:

```yaml
---
- name: Configure RouterOS WireGuard peers
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: mikrotik.routeros.routeros_wireguard
      vars:
        routeros_wireguard_interfaces: []
        routeros_wireguard_peers:
          - interface: wg-site
            public_key: "{{ vault_peer_public_key }}"
            allowed_address: 10.50.0.2/32
...
```

Role-specific documentation is available in the source repository:

- [routeros_access](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_access)
- [routeros_alert_destinations](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_alert_destinations)
- [routeros_backup](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_backup)
- [routeros_baseline](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_baseline)
- [routeros_bgp](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_bgp)
- [routeros_bonding](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_bonding)
- [routeros_bridge](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_bridge)
- [routeros_compliance](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_compliance)
- [routeros_dhcp_relay](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_dhcp_relay)
- [routeros_dhcp_server](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_dhcp_server)
- [routeros_dns](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_dns)
- [routeros_export](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_export)
- [routeros_firewall](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_firewall)
- [routeros_incident_response](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_incident_response)
- [routeros_inventory](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_inventory)
- [routeros_logging](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_logging)
- [routeros_mlag](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_mlag)
- [routeros_netwatch](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_netwatch)
- [routeros_ntp](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_ntp)
- [routeros_ospf](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_ospf)
- [routeros_rollback](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_rollback)
- [routeros_restore](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_restore)
- [routeros_snmp](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_snmp)
- [routeros_static_routes](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_static_routes)
- [routeros_traffic_flow](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_traffic_flow)
- [routeros_upgrade](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_upgrade)
- [routeros_vlan](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_vlan)
- [routeros_vpn](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_vpn)
- [routeros_wireguard](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/tree/main/roles/routeros_wireguard)

## Testing

Run collection quality checks from the collection root:

```text
ansible-lint --profile production .
ansible-test sanity --docker default
```

Before production use, run role syntax checks and integration tests against
representative RouterOS devices. Validate RouterOS 6 and RouterOS 7 behavior
where applicable, and test repeated execution for duplicate-resource risks.

## Support

This is a third-party collection maintained by Tony Reveal. Open issues and
feature requests in the [GitHub issue tracker](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/issues).

Users of a certified distribution should use the Automation Hub **Create
issue** workflow when available for the published collection. Community users
may also seek assistance through the [Ansible Forum](https://forum.ansible.com/).

## Release notes

Release history is maintained in the [collection changelog](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/blob/main/CHANGELOG.md) and the [GitHub releases page](https://github.com/tonyreveal/ansible_mikrotik_routeros_collection/releases).

The collection follows semantic versioning. Changes to role interfaces,
RouterOS command behavior, collection dependencies, or supported Ansible
versions are documented in release notes before publication.

## License

This collection is licensed under [GPL-3.0-or-later](https://www.gnu.org/licenses/gpl-3.0.html).

## Author

Tony Reveal — [https://github.com/tonyreveal](https://github.com/tonyreveal)
