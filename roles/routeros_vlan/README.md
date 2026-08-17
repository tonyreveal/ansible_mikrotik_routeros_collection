# routeros_vlan

Configures RouterOS bridges, VLAN filtering, trunks, and access ports.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- Reviewed VLAN design and an out-of-band recovery path.

## Example

```yaml
---
- name: Configure RouterOS VLANs
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_vlan
      vars:
        routeros_vlan_bridges: []
        routeros_vlan_interfaces: []
        routeros_vlan_vlans: []
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_vlan_bridges` | `[]` | Bridges with names and options. |
| `routeros_vlan_interfaces` | `[]` | Bridge-port assignments. |
| `routeros_vlan_vlans` | `[]` | VLAN interfaces with name, VLAN ID, and parent interface. |

## Variable structure

Bridge entries require `name`; optional bridge properties go under `options`:

```yaml
routeros_vlan_bridges:
  - name: bridge-lan
    options:
      vlan-filtering: true
```

VLAN interface entries require `name`, `vlan_id`, and `interface`:

```yaml
routeros_vlan_vlans:
  - name: vlan10
    vlan_id: 10
    interface: bridge-lan
```

Bridge-port entries require the bridge and physical/interface names:

```yaml
routeros_vlan_interfaces:
  - bridge: bridge-lan
    interface: ether2
  - bridge: bridge-lan
    interface: ether3
```

## Notes

Incorrect VLAN filtering or trunk configuration can cut off management access. Test on representative hardware and use resource-specific state checks when converting command lists into production enforcement.

## Quality checks

```text
ansible-lint --profile production routeros_vlan
ansible-playbook --syntax-check tests/test.yml
```
