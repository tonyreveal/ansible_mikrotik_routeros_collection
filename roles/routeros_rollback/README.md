# routeros_rollback

Installs an explicitly selected RouterOS package version rather than the
latest available version, then installs the RouterBOARD firmware bundled with
that RouterOS package when firmware reports `upgrade-firmware=yes`.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection 3.21.0.
- RouterOS CLI inventory configuration using
  `ansible_connection: ansible.netcommon.network_cli` and
  `ansible_network_os: community.routeros.routeros`.
- An HTTPS-accessible package URL that matches the device architecture and the
  requested RouterOS version.
- An approved maintenance window and out-of-band recovery access.

## Example

```yaml
---
- name: Roll back RouterOS devices
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: mikrotik.routeros.routeros_rollback
      vars:
        routeros_rollback_package_url: https://packages.example.net/routeros-7.22.1-arm64.npk
        routeros_rollback_package_filename: routeros-7.22.1-arm64.npk
        routeros_rollback_package_version: 7.22.1
        routeros_rollback_package_checksum: 0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef
        routeros_rollback_routerboard_upgrade: true
...
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_rollback_package_url` | `''` | URL for the exact package to install. Required. |
| `routeros_rollback_package_filename` | `''` | Destination filename on the router. Required. |
| `routeros_rollback_package_version` | `''` | Exact target RouterOS version. Required. |
| `routeros_rollback_package_checksum` | `''` | Expected package checksum. Required. |
| `routeros_rollback_download_mode` | `https` | RouterOS `/tool fetch` mode. |
| `routeros_rollback_routerboard_upgrade` | `true` | Install the firmware bundled with the target package when required. |
| `routeros_rollback_reboot_timeout` | `900` | Maximum seconds to wait for each reboot. |
| `routeros_rollback_connect_timeout` | `30` | Per-connection timeout. |
| `routeros_rollback_wait_sleep` | `10` | Seconds between connection attempts. |

## Operational notes

RouterOS does not provide a general “install any historical version” command
through the update channel. This role therefore requires an exact package URL,
typically from an internal package repository, and installs that package from
the router using `/system/package/install`.

The RouterBOARD firmware is not independently downloaded by this role. RouterOS
packages contain the matching RouterBOARD firmware; after the package reboot,
the role runs `/system/routerboard/upgrade` when RouterOS reports that firmware
upgrade is required. This is the supported way to align RouterBOARD firmware
with the selected RouterOS package. An older RouterBOARD firmware cannot be
selected independently from the firmware bundled with the target package.

The package URL must match the router architecture. Verify the package checksum
before approving the change, and test the downgrade path on the exact hardware
model. Downgrades can remove features, alter configuration compatibility, or
make the device inaccessible. Check mode is deliberately rejected.

## Quality checks

```text
ansible-lint --profile production roles/routeros_rollback
ansible-playbook --syntax-check tests/test.yml
```
