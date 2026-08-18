# routeros_export

Creates a plain-text RouterOS configuration export using `/export file=`. The
result is a `.rsc` file and is separate from the binary `.backup` file created
by `routeros_backup`.

## Requirements

- Ansible 2.16 or newer and `community.routeros` 3.21.0.
- RouterOS hosts configured for `ansible.netcommon.network_cli` with
  `community.routeros.routeros`.
- Credentials supplied by Vault or an Automation Controller credential.

## Example

```yaml
---
- name: Export RouterOS configuration
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: mikrotik.routeros.routeros_export
      vars:
        routeros_export_filename_prefix: ansible-export
...
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_export_filename_prefix` | `ansible-export` | Safe filename prefix. The role appends `-YYYYMMDD-HHMM`. |

## Notes

The role creates a file with a basename such as
`ansible-export-20260817-1354`; RouterOS stores it as a `.rsc` file. The exact
basename format is
`{{ routeros_export_filename_prefix }}-{{ now(utc=true, fmt='%Y%m%d-%H%M') }}`.
The timestamp uses UTC and minute precision. Retrieve the file with a separate
controlled file-transfer process and protect it because exports may contain
sensitive configuration. This role creates text exports; use
`routeros_restore` only for native `.backup` files.

## Quality checks

```text
ansible-lint --profile production roles/routeros_export
ansible-playbook --syntax-check tests/test.yml
```
