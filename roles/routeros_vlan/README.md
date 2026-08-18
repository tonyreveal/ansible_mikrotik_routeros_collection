# routeros_vlan

Configures VLAN interfaces and bridge ports on an existing RouterOS bridge. Bridge creation is handled by `routeros_bridge`.

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
        routeros_vlan_bridge_name: bridge-lan
        routeros_vlan_interfaces: []
        routeros_vlan_vlans: []
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_vlan_bridge_name` | — | Existing bridge that receives the VLAN configuration. Required. |
| `routeros_vlan_interfaces` | `[]` | Bridge-port assignments. |
| `routeros_vlan_vlans` | `[]` | VLAN interfaces with name, VLAN ID, and parent interface. |

## Variable structure

VLAN interface entries require `name` and `vlan_id`. The optional `interface`
overrides `routeros_vlan_bridge_name`; otherwise the existing bridge is used:

```yaml
routeros_vlan_vlans:
  - name: vlan10
    vlan_id: 10
    interface: bridge-lan
```

Bridge-port entries require an interface name. The optional `bridge` overrides
`routeros_vlan_bridge_name`:

```yaml
routeros_vlan_interfaces:
  - interface: ether2
  - bridge: another-existing-bridge
    interface: ether3
```

## Notes

The bridge must already exist; this role does not create it. Use `routeros_bridge` when bridge creation is required. Incorrect VLAN filtering or trunk configuration can cut off management access. Test on representative hardware and use resource-specific state checks for production enforcement.

## Quality checks

```text
ansible-lint --profile production routeros_vlan
ansible-playbook --syntax-check tests/test.yml
```
