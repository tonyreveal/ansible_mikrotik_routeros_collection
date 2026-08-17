# routeros_routing

Manages static, OSPF, BGP, and failover routing configuration.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- Reviewed routing changes and an out-of-band recovery path.

## Example

```yaml
---
- name: Configure RouterOS routing
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_routing
      vars:
        routeros_routing_static_routes: []
        routeros_routing_bgp_peers: []
        routeros_routing_ospf_instances: []
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_routing_static_routes` | `[]` | Routes with destination, gateway, and options. |
| `routeros_routing_bgp_peers` | `[]` | BGP connections with name, remote address, and options. |
| `routeros_routing_ospf_instances` | `[]` | OSPF instances with name and options. |

## Variable structure

Static routes require `dst_address` and `gateway`; optional RouterOS route
options go under `options`:

```yaml
routeros_routing_static_routes:
  - dst_address: 10.20.0.0/16
    gateway: 192.0.2.1
    options:
      distance: 10
      comment: Branch network
```

BGP peers require `name` and `remote_address`; OSPF instances require `name`.
Their `options` mappings are passed to the corresponding RouterOS `add`
command:

```yaml
routeros_routing_bgp_peers:
  - name: upstream-01
    remote_address: 198.51.100.1
    options:
      remote.as: 64501
      local.role: ebgp

routeros_routing_ospf_instances:
  - name: main
    options:
      router-id: 192.0.2.2
```

## Notes

Routing changes can disconnect the device or downstream networks. Test in a lab, stage changes, and keep a recovery path. Resource-specific state logic is recommended for idempotent production use.

## Quality checks

```text
ansible-lint --profile production routeros_routing
ansible-playbook --syntax-check tests/test.yml
```
