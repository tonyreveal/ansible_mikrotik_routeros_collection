# routeros_snmp

Configures RouterOS SNMP settings from a structured option mapping.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- Approved SNMP manager addresses and credentials.

## Example

```yaml
---
- name: Configure RouterOS SNMP
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_snmp
      vars:
        routeros_snmp_settings:
          enabled: true
          contact: noc@example.net
          location: branch-01
          trap-community: "{{ vault_snmp_community }}"
          trap-version: 3
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_snmp_settings` | `{}` | Exact RouterOS `/snmp set` option/value mapping. |

## Notes

Store communities, usernames, and authentication values in Vault or approved controller credentials. Restrict SNMP access to known management networks and validate the target RouterOS version’s SNMP options.

## Quality checks

```text
ansible-lint --profile production routeros_snmp
ansible-playbook --syntax-check tests/test.yml
```
