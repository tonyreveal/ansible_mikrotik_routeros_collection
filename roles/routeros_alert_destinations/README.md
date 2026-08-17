# routeros_alert_destinations

Configures RouterOS email delivery settings used by scripts, schedulers, and alerting workflows.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- An SMTP service reachable from the RouterOS device.

## Example

```yaml
---
- name: Configure RouterOS alert email destination
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_alert_destinations
      vars:
        routeros_alert_destinations_email:
          server: smtp.example.net
          port: 587
          from: router@example.net
          user: router@example.net
          password: "{{ vault_smtp_password }}"
          tls: yes
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_alert_destinations_email` | `{}` | `/tool e-mail set` option/value mapping. |

## Notes

The role configures the email destination but does not create alert-producing scripts, schedulers, or Netwatch event handlers. Store SMTP credentials in Vault or approved controller credentials. The task uses `no_log` to protect credentials.

## Quality checks

```text
ansible-lint --profile production routeros_alert_destinations
ansible-playbook --syntax-check tests/test.yml
```
