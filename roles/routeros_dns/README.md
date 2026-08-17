# routeros_dns

Configures RouterOS DNS resolver settings and static DNS entries from structured variables.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- DNS forwarding and access requirements reviewed before deployment.

## Example

```yaml
---
- name: Configure RouterOS DNS
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_dns
      vars:
        routeros_dns_settings:
          servers: 10.0.0.53,10.0.0.54
          allow-remote-requests: true
          cache-size: 4096KiB
        routeros_dns_static_entries:
          - name: router.example.net
            address: 192.0.2.1
            ttl: 1d
            comment: Router management address
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_dns_settings` | `{}` | RouterOS `/ip dns set` option/value mapping. |
| `routeros_dns_static_entries` | `[]` | Entries requiring `name` and `address`; TTL, comment, and disabled state are optional. |

## Notes

DNS setting keys use the exact RouterOS option names. Consider the security impact of `allow-remote-requests`; restrict management access and avoid creating an unintended open resolver. The role adds static entries and does not reconcile duplicate records.

## Quality checks

```text
ansible-lint --profile production routeros_dns
ansible-playbook --syntax-check tests/test.yml
```
