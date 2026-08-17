# routeros_incident_response

Applies explicitly supplied emergency actions such as disabling a compromised account or adding a temporary block.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- An approved incident, audit trail, and out-of-band recovery path.

## Example

```yaml
---
- name: Apply approved RouterOS containment action
  hosts: affected_routeros
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_incident_response
      vars:
        routeros_incident_response_block_addresses:
          - address: 203.0.113.10
            list: incident-block
            comment: Approved containment IOC
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_incident_response_block_addresses` | `[]` | Address mappings to add to the incident block list. |
| `routeros_incident_response_disable_users` | `[]` | Usernames to disable during response. |

## Variable structure

The current interface uses two structured lists. Address entries require
`address`; `list` and `comment` are optional:

```yaml
routeros_incident_response_block_addresses:
  - address: 203.0.113.10
    list: incident-block
    comment: Approved containment IOC
```

Users to disable are supplied as usernames:

```yaml
routeros_incident_response_disable_users:
  - compromised-user
  - former-admin
```

## Notes

Use human approval and change tracking. Review commands before execution, limit scope, and define a rollback or expiry action for temporary controls.

## Quality checks

```text
ansible-lint --profile production routeros_incident_response
ansible-playbook --syntax-check tests/test.yml
```
