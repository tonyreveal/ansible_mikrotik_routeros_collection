# routeros_dhcp_relay

Configures RouterOS DHCP relay instances from structured variables.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- A reachable DHCP server and correctly routed relay interface.

## Example

```yaml
---
- name: Configure DHCP relay
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_dhcp_relay
      vars:
        routeros_dhcp_relay_instances:
          - name: relay-users
            interface: vlan20
            dhcp_server: 10.20.0.10
            local_address: 10.20.0.1
            disabled: false
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_dhcp_relay_instances` | `[]` | Relay mappings. `name`, `interface`, and `dhcp_server` are required; `local_address` and `disabled` are optional. |

## Notes

The role constructs `/ip dhcp-relay add` commands. The DHCP server address may be an IPv4 address or a RouterOS-resolvable server value as supported by the target RouterOS version. Test relay traffic and preserve management access before production rollout.

## Quality checks

```text
ansible-lint --profile production routeros_dhcp_relay
ansible-playbook --syntax-check tests/test.yml
```
