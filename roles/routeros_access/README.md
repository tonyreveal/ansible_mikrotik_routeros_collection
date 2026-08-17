# routeros_access

Manages RouterOS users, SSH/API services, and administrative access.

## Requirements

- Ansible 2.16 or newer.
- The pinned `community.routeros` collection version tested by your repository.
- `ansible_connection: ansible.netcommon.network_cli` and `ansible_network_os: community.routeros.routeros`.
- Credentials and keys supplied through Vault or an Automation Controller credential.

## Example

```yaml
---
- name: Configure RouterOS access
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_access
      vars:
        routeros_access_users:
          - username: automation
            password: "{{ vault_routeros_automation_password }}"
            group: full
        routeros_access_ssh_settings:
          strong-crypto: true
          forwarding-enabled: false
        routeros_access_api_service_settings:
          - service: api
            options:
              - option: disabled
                value: true
              - option: address
                value: 10.0.0.0/8
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_access_users` | `[]` | Users with `username`, `group`, optional `password`, and optional `disabled`. |
| `routeros_access_ssh_settings` | `{}` | SSH options passed as `option: value` pairs to `/ip ssh set`. |
| `routeros_access_api_service_settings` | `[]` | API service entries with `service` and option/value pairs. |

## Variable structure

Users require `username` and `group`; `password` is required when creating a
new user and `disabled` is optional:

```yaml
routeros_access_users:
  - username: automation
    password: "{{ vault_routeros_automation_password }}"
    group: full
    disabled: false
```

SSH settings are a mapping of RouterOS `/ip ssh set` option names to values:

```yaml
routeros_access_ssh_settings:
  strong-crypto: true
  forwarding-enabled: false
  always-allow-password-login: false
```

API services use a list. Each item requires `service` and an `options` list;
each option requires `option` and `value`:

```yaml
routeros_access_api_service_settings:
  - service: api
    options:
      - option: disabled
        value: false
      - option: address
        value: 10.0.0.0/8
  - service: api-ssl
    options:
      - option: disabled
        value: true
```

## Notes

The role builds the RouterOS commands internally; callers provide user data and settings rather than raw commands. Store passwords in Ansible Vault or controller credentials. User and service tasks are reported changed when applied, so command-list-driven settings should be tested for idempotency against the target RouterOS and collection versions. Test changes with an out-of-band recovery path because incorrect service restrictions can lock out management access.

## Quality checks

```text
ansible-lint --profile production routeros_access
ansible-playbook --syntax-check tests/test.yml
```
