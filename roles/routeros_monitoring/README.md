# routeros_monitoring

Configures RouterOS logging, SNMP, Netwatch, traffic flow, and alert destinations.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- Approved monitoring endpoints and any secrets stored in Vault or controller credentials.

## Example

```yaml
---
- name: Configure RouterOS monitoring
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_monitoring
      vars:
        routeros_monitoring_syslog_actions: []
        routeros_monitoring_snmp: {}
        routeros_monitoring_netwatch: []
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_monitoring_syslog_actions` | `[]` | Syslog actions with name, target, and options. |
| `routeros_monitoring_snmp` | `{}` | `/snmp set` options. |
| `routeros_monitoring_netwatch` | `[]` | Netwatch entries with host and options. |

## Variable structure

Syslog actions require `name` and `target`; optional RouterOS action properties
go under `options`:

```yaml
routeros_monitoring_syslog_actions:
  - name: remote-syslog
    target: remote
    options:
      remote: 192.0.2.50
      remote-port: 514
```

SNMP is a mapping passed to `/snmp set`:

```yaml
routeros_monitoring_snmp:
  enabled: true
  trap-community: monitoring
  trap-version: 3
```

Netwatch entries require `host`; probe and event properties go under `options`:

```yaml
routeros_monitoring_netwatch:
  - host: 192.0.2.1
    options:
      interval: 30s
      timeout: 1s
      comment: Upstream gateway
```

## Notes

Validate that monitoring endpoints are reachable and authorized. Avoid placing SNMP communities, tokens, or passwords in role code or unencrypted variables.

## Quality checks

```text
ansible-lint --profile production routeros_monitoring
ansible-playbook --syntax-check tests/test.yml
```
