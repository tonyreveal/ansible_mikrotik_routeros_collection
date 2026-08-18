# routeros_bgp

Configures RouterOS BGP connections.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` 3.21.0 and RouterOS CLI access.

## Example

```yaml
---
- name: Configure BGP
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: mikrotik.routeros.routeros_bgp
      vars:
        routeros_bgp_peers:
          - name: upstream-01
            remote_address: 198.51.100.1
            options:
              remote.as: 64501
              local.role: ebgp
...
```

## Role variables

`routeros_bgp_peers` is a list of mappings. Each item requires `name` and
`remote_address`; RouterOS connection options belong under `options`.

## Notes

Validate autonomous-system, address-family, and policy requirements before
deployment. Keep an out-of-band recovery path for routing changes.

## Quality checks

```text
ansible-lint --profile production roles/routeros_bgp
ansible-playbook --syntax-check tests/test.yml
```
