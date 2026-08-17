# routeros_baseline

Applies an organization-defined RouterOS baseline supplied through inventory variables.

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
        routeros_baseline_dns:
          servers: 10.0.0.53
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_baseline_identity` | `{}` | Router identity settings. |
| `routeros_baseline_dns` | `{}` | `/ip dns set` options. |
| `routeros_baseline_ntp` | `{}` | NTP client settings. |

## Variable structure

Each mapping uses RouterOS option names as keys and their desired values as values:

```yaml
routeros_baseline_identity:
  name: branch-router-01

routeros_baseline_dns:
  servers: 10.0.0.53,10.0.0.54
  allow-remote-requests: false

routeros_baseline_ntp:
  enabled: true
  mode: unicast
  servers: 10.0.0.123
```

`routeros_baseline_identity` currently expects `name`. DNS keys are passed to
`/ip dns set`; NTP keys are passed to `/system ntp client set`. Use the exact
RouterOS spelling for option names.

## Notes

Keep baseline settings separate from role code. The role owns the RouterOS command syntax; expand this interface with resource-specific settings as baseline requirements grow.

## Quality checks

```text
ansible-lint --profile production routeros_baseline
ansible-playbook --syntax-check tests/test.yml
```
