# routeros_upgrade

An Ansible role that checks for RouterOS package updates, installs them when available, waits for the package-update reboot, upgrades RouterBOARD firmware when required, reboots again, and validates the final state.

## Requirements

- Ansible 2.16 or newer.
- The `community.routeros` collection.
- Inventory hosts configured with `ansible_connection: ansible.netcommon.network_cli` and `ansible_network_os: community.routeros.routeros`.
- Credentials supplied by inventory, Ansible Vault, or an Automation Controller credential; do not store them in this role.

Install the collection with a pinned version appropriate for your environment:

```text
ansible-galaxy collection install community.routeros:<tested-version>
```

Pin the collection to the exact version tested by your repository or execution
environment. The role intentionally does not install collections at runtime.

## Example

```yaml
---
- name: Upgrade RouterOS devices
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: routeros_upgrade
      vars:
        routeros_upgrade_channel: stable
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_upgrade_channel` | `stable` | RouterOS release channel. |
| `routeros_upgrade_reboot_timeout` | `600` | Maximum seconds to wait for a reboot. |
| `routeros_upgrade_connect_timeout` | `30` | Per-connection timeout. |
| `routeros_upgrade_wait_sleep` | `10` | Seconds between connection attempts. |

## Operational notes

The role uses RouterOS CLI output to detect update availability and firmware status. Test the exact RouterOS version and `community.routeros` collection version in a lab first. RouterOS upgrades may be disruptive and this role does not provide automatic rollback.

Check mode is deliberately rejected because package and firmware update commands
cannot provide a reliable dry-run result. Use a lab device and a controlled
promotion process before production execution.

## Quality checks

Run these checks from the repository containing the role:

```text
ansible-lint --profile production routeros_upgrade
ansible-playbook --syntax-check tests/test.yml
```
