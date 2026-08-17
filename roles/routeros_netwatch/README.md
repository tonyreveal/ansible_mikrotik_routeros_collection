# routeros_netwatch

Configures RouterOS Netwatch entries for reachability monitoring and event actions.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.

## Example

```yaml
---
- name: Configure RouterOS Netwatch
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_netwatch
      vars:
        routeros_netwatch_entries:
          - host: 192.0.2.1
            options:
              interval: 30s
              timeout: 1s
              comment: Upstream gateway
              on-up: ":log info 'gateway reachable'"
              on-down: ":log warning 'gateway unavailable'"
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_netwatch_entries` | `[]` | Entries requiring `host`; Netwatch options are supplied under `options`. |

## Notes

Option names and event scripts are passed to `/tool netwatch add` using RouterOS syntax. Review scripts carefully and test reachability transitions before production use. The role adds entries and does not reconcile duplicates.

## Quality checks

```text
ansible-lint --profile production routeros_netwatch
ansible-playbook --syntax-check tests/test.yml
```
