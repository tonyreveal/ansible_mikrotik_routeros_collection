# routeros_ospf

Configures RouterOS OSPF instances.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` 3.21.0 and RouterOS CLI access.

## Example

```yaml
---
- name: Configure OSPF
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: mikrotik.routeros.routeros_ospf
      vars:
        routeros_ospf_instances:
          - name: main
            options:
              router-id: 192.0.2.2
...
```

## Role variables

`routeros_ospf_instances` is a list. Each item requires `name`; RouterOS OSPF
instance properties belong under `options`.

## Notes

Configure the required OSPF areas and interfaces separately according to the
device design. Keep an out-of-band recovery path.

## Quality checks

```text
ansible-lint --profile production roles/routeros_ospf
ansible-playbook --syntax-check tests/test.yml
```
