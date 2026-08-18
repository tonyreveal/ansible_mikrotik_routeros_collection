# routeros_backup

Creates a native RouterOS binary `.backup` file.

## Requirements

- Ansible 2.16 or newer and a pinned `community.routeros` collection.
- RouterOS hosts configured for `ansible.netcommon.network_cli` with `community.routeros.routeros`.
- Credentials supplied by Vault or an Automation Controller credential.

## Example

```yaml
---
- name: Back up RouterOS configuration
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_backup
      vars:
        routeros_backup_filename_prefix: ansible-backup
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_backup_filename_prefix` | `ansible-backup` | Safe filename prefix. The role appends `-YYYYMMDD-HHMM`. |

## Notes

The binary backup is created on the device with a basename such as
`ansible-backup-20260817-1354`; RouterOS stores it as a `.backup` file. The
basename is generated with the exact format
`{{ routeros_backup_filename_prefix }}-{{ now(utc=true, fmt='%Y%m%d-%H%M') }}`.
The timestamp uses UTC and minute precision. Retrieve the file with a separate
controlled file-transfer process and protect it because backups contain
sensitive configuration. Restore it with `routeros_restore`. This task is
intentionally reported changed when run.

## Quality checks

```text
ansible-lint --profile production routeros_backup
ansible-playbook --syntax-check tests/test.yml
```
