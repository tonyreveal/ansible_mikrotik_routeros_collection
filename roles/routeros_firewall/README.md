# routeros_firewall

Applies reviewed RouterOS firewall, address-list, and NAT policy.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS network CLI inventory configuration.
- An out-of-band recovery path and reviewed policy commands.

## Example

```yaml
---
- name: Configure RouterOS firewall
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_firewall
      vars:
        routeros_firewall_address_lists: []
        routeros_firewall_filter_rules: []
        routeros_firewall_nat_rules: []
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_firewall_address_lists` | `[]` | Address-list entries with `list` and `address`. |
| `routeros_firewall_filter_rules` | `[]` | Filter rules with `chain` and option mappings. |
| `routeros_firewall_nat_rules` | `[]` | NAT rules with `chain` and option mappings. |

## Variable structure

Address-list entries require `list` and `address`; `timeout` and `comment` are
optional:

```yaml
routeros_firewall_address_lists:
  - list: trusted-admins
    address: 192.0.2.10
    comment: Admin workstation
    timeout: 1d
```

Filter and NAT rules require `chain` and an `options` mapping. The mapping keys
are passed as RouterOS rule options:

```yaml
routeros_firewall_filter_rules:
  - chain: input
    options:
      action: accept
      protocol: tcp
      dst-port: 22
      src-address-list: trusted-admins
      comment: Allow administration

routeros_firewall_nat_rules:
  - chain: srcnat
    options:
      action: masquerade
      out-interface: ether1
```

## Notes

Test policy changes in a lab and stage changes carefully. Arbitrary command lists are not idempotent; use resource-specific state checks for production policy management. Never store secrets in this role.

## Quality checks

```text
ansible-lint --profile production routeros_firewall
ansible-playbook --syntax-check tests/test.yml
```
