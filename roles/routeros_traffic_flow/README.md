# routeros_traffic_flow

Configures RouterOS traffic-flow export settings and collectors.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- A compatible NetFlow/IPFIX collector.

## Example

```yaml
---
- name: Configure RouterOS traffic flow
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_traffic_flow
      vars:
        routeros_traffic_flow_settings:
          enabled: true
          interfaces: all
          cache-entries: 4k
        routeros_traffic_flow_targets:
          - address: 192.0.2.70
            port: 2055
            version: 9
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_traffic_flow_settings` | `{}` | Exact `/ip traffic-flow set` option/value mapping. |
| `routeros_traffic_flow_targets` | `[]` | Target mappings requiring `address`; port, version, and source address are optional. |

## Notes

Confirm collector protocol and RouterOS version support before enabling export. Traffic-flow can increase device and network load; begin with a limited scope and monitor resource usage.

## Quality checks

```text
ansible-lint --profile production routeros_traffic_flow
ansible-playbook --syntax-check tests/test.yml
```
