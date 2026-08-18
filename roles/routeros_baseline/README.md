# routeros_baseline

Applies the core RouterOS system identity baseline. DNS and NTP are intentionally
managed by the dedicated `routeros_dns` and `routeros_ntp` roles.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- Baseline settings maintained in reviewed group variables.

## Example

```yaml
---
- name: Apply RouterOS baseline
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_baseline
      vars:
        routeros_baseline_identity:
          name: branch-router
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_baseline_identity` | `{}` | Identity mapping; currently supports `name`. |

## Variable structure

Each mapping uses RouterOS option names as keys and their desired values as values:

```yaml
routeros_baseline_identity:
  name: branch-router-01
```

`routeros_baseline_identity` expects `name`. Use `routeros_dns` and
`routeros_ntp` for their respective settings.

## Notes

Keep baseline settings separate from role code. The role owns the RouterOS command syntax; expand this interface with resource-specific settings as baseline requirements grow.

## Quality checks

```text
ansible-lint --profile production routeros_baseline
ansible-playbook --syntax-check tests/test.yml
```
