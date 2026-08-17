# routeros_dhcp_server

Configures RouterOS DHCP address pools, DHCP networks, and DHCP server instances from structured variables.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- Interfaces and addresses required by the DHCP design already provisioned.

## Example

```yaml
---
- name: Configure DHCP server
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_dhcp_server
      vars:
        routeros_dhcp_server_pools:
          - name: pool-users
            ranges: 10.20.0.100-10.20.0.200
        routeros_dhcp_server_networks:
          - address: 10.20.0.0/24
            gateway: 10.20.0.1
            dns_server: 10.20.0.1
            domain: users.example.net
        routeros_dhcp_server_instances:
          - name: dhcp-users
            interface: vlan20
            address_pool: pool-users
            lease_time: 1d
            disabled: false
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_dhcp_server_pools` | `[]` | Pool mappings requiring `name` and `ranges`. |
| `routeros_dhcp_server_networks` | `[]` | Network mappings requiring `address`; gateway, DNS, domain, and comment are optional. |
| `routeros_dhcp_server_instances` | `[]` | Server mappings requiring `name`, `interface`, and `address_pool`; lease time and disabled state are optional. |

## Notes

The role creates pools before networks and server instances. It adds resources and does not reconcile duplicate objects. Validate address pools, gateway reachability, and DHCP client behavior in a lab before production use.

## Quality checks

```text
ansible-lint --profile production routeros_dhcp_server
ansible-playbook --syntax-check tests/test.yml
```
