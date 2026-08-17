# routeros_ntp

Configures RouterOS NTP client settings and NTP servers from structured variables.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- RouterOS version and NTP command paths validated for the target device.

## Example

```yaml
---
- name: Configure RouterOS NTP
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_ntp
      vars:
        routeros_ntp_client_settings:
          enabled: true
          mode: unicast
        routeros_ntp_servers:
          - address: time-a.example.net
          - address: time-b.example.net
            disabled: false
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_ntp_client_settings` | `{}` | Option/value mapping passed to `/system ntp client set`. |
| `routeros_ntp_servers` | `[]` | Server mappings requiring `address`; `disabled` is optional. |

## Variable structure

Client settings use exact RouterOS option names:

```yaml
routeros_ntp_client_settings:
  enabled: true
  mode: unicast
```

Each server is a mapping with an NTP hostname or IP address:

```yaml
routeros_ntp_servers:
  - address: 192.0.2.123
    disabled: false
```

## Notes

The role constructs `/system ntp client set` and `/system ntp client servers add` commands. It adds server entries and does not reconcile duplicates. Confirm the exact NTP command paths supported by the target RouterOS major version before production deployment.

## Quality checks

```text
ansible-lint --profile production routeros_ntp
ansible-playbook --syntax-check tests/test.yml
```
