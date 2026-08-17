# routeros_maintenance

Checks for and installs RouterOS package updates, then optionally reboots the device.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- An approved maintenance window and recovery plan for reboot operations.

## Example

```yaml
---
- name: Perform RouterOS maintenance
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_maintenance
      vars:
        routeros_maintenance_update_packages: true
        routeros_maintenance_reboot: false
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_maintenance_update_packages` | `false` | Check for and install RouterOS package updates. |
| `routeros_maintenance_reboot` | `false` | Explicitly enables reboot. |
| `routeros_maintenance_reboot_timeout` | `600` | Maximum reboot wait in seconds. |
| `routeros_maintenance_connect_timeout` | `30` | Per-connection timeout. |

## Notes

Package installation may reboot the device. Schedule disruptive work through an approved maintenance window; use the dedicated upgrade role when RouterBOARD firmware handling is also required.

## Quality checks

```text
ansible-lint --profile production routeros_maintenance
ansible-playbook --syntax-check tests/test.yml
```
