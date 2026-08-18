# routeros_mlag

Configures Multi-chassis Link Aggregation Group (MLAG) bridge peer settings on
MikroTik RouterOS 7.22 and newer. Bond interfaces are managed separately by
`routeros_bonding`, using its optional `mlag_id` variable.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS 7.22 or newer. This role does not support the pre-7.22 MLAG menu
  and property names.
- Hosts configured with `ansible_connection: ansible.netcommon.network_cli` and
  `ansible_network_os: community.routeros.routeros`.
- Two RouterOS devices with a directly connected MLAG peer port and coordinated
  configuration.

## Example

```yaml
---
- name: Configure RouterOS MLAG peers
  hosts: mlag_switches
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_mlag
      vars:
        routeros_mlag_bridges:
          - name: bridge-lan
            mlag_peer_port: bond-peer
            mlag_heartbeat: 1s
            mlag_priority: 128
            protocol_mode: rstp
```

Create the corresponding bond with `routeros_bonding` and the same `mlag_id` on
both MLAG devices. The peer-port must be directly connected between the devices.

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_mlag_bridges` | `[]` | Bridge mappings requiring `name` and `mlag_peer_port`; heartbeat, priority, and protocol mode are optional. |

## Variable structure

Bridge definitions use the RouterOS 7.22+ MLAG properties:

```yaml
routeros_mlag_bridges:
  - name: bridge-lan
    mlag_peer_port: bond-peer
    mlag_heartbeat: 1s
    mlag_priority: 128
    protocol_mode: rstp
```

MLAG bonds are defined in `routeros_bonding` with a stable ID shared by both
peers:

```yaml
routeros_bonding_interfaces:
  - name: bond-server01
    slaves:
      - ether3
      - ether4
    mlag_id: 101
    mode: 802.3ad
    transmit_hash_policy: layer-2-and-3
```

## Notes

RouterOS 7.22 moved MLAG configuration from the dedicated MLAG menu to the
normal bridge menu and added the `mlag-` property prefix. This role deliberately
uses only the RouterOS 7.22+ syntax.

MLAG requires matching, coordinated configuration on both devices. Confirm
peer-port VLAN handling, STP settings, MLAG IDs, and switch-side LACP settings
before deployment. MLAG is hardware offloaded only on supported MikroTik
platforms; otherwise it runs in software. MLAG is incompatible with L3 hardware
offloading and MVRP, and large host tables can consume significant CPU and
memory.

The role updates existing bridge records; it does not create bonds or reconcile
bridge ports, VLAN tables, or peer configuration.
Use an out-of-band recovery path and test failover before production use.

## Quality checks

```text
ansible-lint --profile production routeros_mlag
ansible-playbook --syntax-check tests/test.yml
```
