# routeros_bonding

Configures RouterOS bonded interfaces from structured interface definitions.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- Switch-side bonding or LAG configuration planned and validated before deployment.

## Example

```yaml
---
- name: Configure RouterOS interface bonding
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_bonding
      vars:
        routeros_bonding_interfaces:
          - name: bond-uplink
            slaves:
              - ether1
              - ether2
            mode: 802.3ad
            mlag_id: 100
            lacp_rate: 1sec
            transmit_hash_policy: layer-2-and-3
            link_monitoring: mii
            mii_monitor_interval: 100ms
            comment: Redundant uplink
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_bonding_interfaces` | `[]` | Bond definitions. Each entry requires `name` and a `slaves` list. |

## Variable structure

Each bonding entry has a RouterOS interface name and member interfaces:

```yaml
routeros_bonding_interfaces:
  - name: bond-storage
    slaves:
      - sfp-sfpplus1
      - sfp-sfpplus2
    mode: active-backup
```

Optional fields map to RouterOS bonding properties:

- `mode`
- `lacp_rate`
- `transmit_hash_policy`
- `link_monitoring`
- `mii_monitor_interval`
- `comment`
- `mlag_id` (use this only for an MLAG bond)

The role converts option names such as `lacp_rate` to RouterOS names such as
`lacp-rate` when constructing the command.

## Notes

The role constructs `/interface bonding add` commands and does not reconcile
existing bonding interfaces or duplicate members. Bonding changes can interrupt
connectivity; coordinate the switch-side LAG configuration, use an out-of-band
recovery path, and test each supported RouterOS version before production use.

For RouterOS 7.22 and newer MLAG, define `mlag_id` here and use
`routeros_mlag` separately to configure the MLAG bridge peer settings.

## Quality checks

```text
ansible-lint --profile production routeros_bonding
ansible-playbook --syntax-check tests/test.yml
```
