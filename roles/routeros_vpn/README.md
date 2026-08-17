# routeros_vpn

Provisions RouterOS IPsec VPN peer configuration. WireGuard is provided by the separate `routeros_wireguard` role.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- Tunnel keys and pre-shared secrets stored in Vault or controller credentials.

## Example

```yaml
---
- name: Configure RouterOS VPN
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_vpn
      vars:
        routeros_vpn_ipsec_peers: []
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_vpn_ipsec_peers` | `[]` | IPsec peers with address and option mappings. |

## Variable structure

IPsec peers require `address`; additional peer properties are supplied under
`options`:

```yaml
routeros_vpn_ipsec_peers:
  - address: 198.51.100.10/32
    options:
      exchange-mode: ike2
      send-initial-contact: true
```

## Notes

Never store pre-shared secrets in this role or unencrypted variables. Validate IPsec reachability and routing in a lab before production rollout. Use `routeros_wireguard` for all WireGuard interfaces and peers.

## Quality checks

```text
ansible-lint --profile production routeros_vpn
ansible-playbook --syntax-check tests/test.yml
```
