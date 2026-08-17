# routeros_compliance

Collects RouterOS evidence for compliance checks.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- Organization-specific assertions in the consuming playbook or CI pipeline.

## Example

```yaml
---
- name: Collect RouterOS compliance evidence
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_compliance
      vars:
        routeros_compliance_check_updates: true
        routeros_compliance_check_services: true
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_compliance_check_updates` | `true` | Collect package update status. |
| `routeros_compliance_check_services` | `true` | Collect RouterOS service status. |

## Notes

The role collects evidence but does not claim compliance by itself. Assert the required values in a consuming playbook, store results through approved logging, and avoid exposing secrets in output.

## Quality checks

```text
ansible-lint --profile production routeros_compliance
ansible-playbook --syntax-check tests/test.yml
```
