# routeros_restore

Restores a native RouterOS binary `.backup` file. The binary backup must already
be present on the RouterOS device before this role runs.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection 3.21.0.
- RouterOS hosts configured for `ansible.netcommon.network_cli` with
  `community.routeros.routeros`.
- An approved restore window and out-of-band recovery access.
- A native `.backup` file transferred to the router's files directory.

## Example

```yaml
---
- name: Restore RouterOS configuration
  hosts: routeros_devices
  gather_facts: false
  serial: 1
  roles:
    - role: mikrotik.routeros.routeros_restore
      vars:
        routeros_restore_filename: router-backup-20260817-1354.backup
...
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_restore_filename` | `''` | Exact `.backup` filename already on the router. Required. |
| `routeros_restore_reboot_timeout` | `600` | Maximum seconds to wait for the reboot. |
| `routeros_restore_connect_timeout` | `30` | Per-connection timeout. |
| `routeros_restore_wait_sleep` | `10` | Seconds between connection attempts. |

## Notes

This role restores a native RouterOS binary backup using
`/system/backup/load`. Transfer the file to the router with a controlled,
separately audited file-transfer process before running the role.

Binary backup loading restores device configuration and may reboot the router.
Use serial execution, an out-of-band path, and a tested recovery plan. The role
waits for the device to return after the load operation.

Check mode is deliberately rejected because import and reboot operations are
live changes.

## Quality checks

```text
ansible-lint --profile production roles/routeros_restore
ansible-playbook --syntax-check tests/test.yml
```
