# routeros_static_routes

Configures RouterOS static routes.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` 3.21.0.
- RouterOS network CLI inventory configuration.

## Example

```yaml
---
- name: Configure static routes
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: mikrotik.routeros.routeros_static_routes
      vars:
        routeros_static_routes:
          - dst_address: 10.20.0.0/16
            gateway: 192.0.2.1
            options:
              distance: 10
              comment: Branch network
...
```

## Role variables

`routeros_static_routes` is a list. Each item requires `dst_address` and
`gateway`; optional RouterOS route properties belong under `options`.

## Notes

Route changes can interrupt connectivity. Use an out-of-band recovery path and
test changes before production deployment.

## Quality checks

```text
ansible-lint --profile production roles/routeros_static_routes
ansible-playbook --syntax-check tests/test.yml
```
