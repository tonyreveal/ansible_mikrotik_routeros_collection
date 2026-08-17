# routeros_inventory

Collects RouterOS version, board, package, and resource information as the `routeros_inventory` host fact.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- An approved destination if results are persisted outside Ansible.

## Example

```yaml
---
- name: Inventory RouterOS devices
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_inventory
      vars:
        routeros_inventory_collect_interfaces: true
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_inventory_collect_interfaces` | `true` | Include interface data in inventory collection. |

## Notes

The resulting output is available as the host fact `routeros_inventory`. Treat collected output as potentially sensitive and do not expose credentials or private configuration in reports.

## Quality checks

```text
ansible-lint --profile production routeros_inventory
ansible-playbook --syntax-check tests/test.yml
```
