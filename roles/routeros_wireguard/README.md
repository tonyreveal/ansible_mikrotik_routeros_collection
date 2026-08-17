# routeros_wireguard

Configures RouterOS WireGuard interfaces and adds WireGuard peers from structured variables. Interface configuration and peer configuration are independent: when `routeros_wireguard_interfaces` is empty, interface tasks are skipped and supplied peers are still added.

## Requirements

- Ansible 2.16 or newer.
- RouterOS with WireGuard support.
- A pinned `community.routeros` collection version tested by the repository.
- Hosts configured with `ansible_connection: ansible.netcommon.network_cli` and `ansible_network_os: community.routeros.routeros`.
- Private keys supplied by Ansible Vault or an Automation Controller credential.

## Example: configure an interface and peers

```yaml
---
- name: Configure RouterOS WireGuard
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_wireguard
      vars:
        routeros_wireguard_interfaces:
          - name: wg-site
            listen_port: 13231
            private_key: "{{ vault_wireguard_private_key }}"
            mtu: 1420
        routeros_wireguard_peers:
          - interface: wg-site
            public_key: "{{ vault_peer_public_key }}"
            allowed_address: 10.50.0.2/32
            endpoint_address: vpn.example.net
            endpoint_port: 13231
            persistent_keepalive: 25s
            comment: Branch peer
```

## Example: add peers only

When the interface already exists, omit the interface list or set it to an
empty list. The interface task is skipped and the peer task still runs:

```yaml
routeros_wireguard_interfaces: []
routeros_wireguard_peers:
  - interface: wg-site
    public_key: "{{ vault_new_peer_public_key }}"
    allowed_address: 10.50.0.3/32
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_wireguard_interfaces` | `[]` | Interface mappings. `name` is required; `listen_port`, `private_key`, and `mtu` are optional. |
| `routeros_wireguard_peers` | `[]` | Peer mappings. `interface`, `public_key`, and `allowed_address` are required; endpoint and keepalive values are optional. |

## Notes

The role constructs `/interface wireguard add` and `/interface wireguard peers add` commands internally. Private keys and public keys are protected with `no_log`; keep them in Vault or controller credentials. The role adds resources and does not reconcile duplicate interfaces or peers, so test repeated execution and use a reviewed deployment process.

## Quality checks

```text
ansible-lint --profile production routeros_wireguard
ansible-playbook --syntax-check tests/test.yml
```
