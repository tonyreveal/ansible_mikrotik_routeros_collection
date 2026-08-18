# routeros_bridge

Creates RouterOS bridge interfaces from structured definitions. VLAN and MLAG configuration are handled by separate roles.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.

## Example

```yaml
---
- name: Create RouterOS bridge
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_bridge
      vars:
        routeros_bridge_interfaces:
          - name: bridge-lan
            options:
              vlan-filtering: true
              protocol-mode: rstp
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_bridge_interfaces` | `[]` | Bridge definitions requiring `name`; optional RouterOS properties go under `options`. |

## Notes

The role constructs `/interface bridge add` commands and does not reconcile duplicate bridges. Use `routeros_vlan` to add VLAN interfaces and bridge ports to an existing bridge, and `routeros_mlag` to configure MLAG properties on an existing bridge.

## Quality checks

```text
ansible-lint --profile production routeros_bridge
ansible-playbook --syntax-check tests/test.yml
```
