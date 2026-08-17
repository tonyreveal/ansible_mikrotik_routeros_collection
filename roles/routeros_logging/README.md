# routeros_logging

Configures RouterOS to send selected log topics to an external log aggregator through a remote logging action.

## Requirements

- Ansible 2.16 or newer.
- `community.routeros` collection version 3.21.0.
- RouterOS network CLI inventory configuration.
- A reachable syslog or CEF log collector.
- Network policy allowing the selected remote logging protocol and port.

## Example: syslog aggregator

```yaml
---
- name: Configure RouterOS remote syslog
  hosts: routeros_devices
  gather_facts: false
  roles:
    - role: routeros_logging
      vars:
        routeros_logging_remote_action:
          name: remote
          remote: 192.0.2.50
          remote_port: 514
          remote_log_format: syslog
          syslog_facility: local0
          syslog_severity: auto
          syslog_time_format: bsd-syslog
          remote_protocol: udp
        routeros_logging_topic_rules:
          - topics: error
          - topics: warning
          - topics: critical
          - topics: firewall
          - topics: dhcp
```

## Example: CEF aggregator

```yaml
routeros_logging_remote_action:
  name: remote
  remote: 192.0.2.60
  remote_port: 6514
  remote_log_format: cef
  remote_protocol: tls
  check_certificate: true
```

## Role variables

| Variable | Default | Purpose |
| --- | --- | --- |
| `routeros_logging_remote_action` | Remote syslog defaults | Mapping for the RouterOS remote action. `name` and `remote` are required. |
| `routeros_logging_topic_rules` | `[]` | Topic mappings. Each item requires `topics`; `action`, `prefix`, and `regex` are optional. |

## Variable structure

The remote action uses exact RouterOS action option names expressed as Ansible
snake_case keys. The role converts underscores to hyphens when constructing
the command. Common keys include `remote`, `remote_port`,
`remote_log_format`, `remote_protocol`, `syslog_facility`,
`syslog_severity`, `syslog_time_format`, `src_address`, `vrf`, and
`check_certificate`.

Topic rules use RouterOS topic names. Multiple topics can be supplied in the
RouterOS format accepted by the target version, for example `route,bgp,error`.

## Notes

RouterOS sends syslog-formatted messages using UDP. TCP and TLS are supported
for CEF remote logging; select the format and protocol supported by the target
RouterOS release and aggregator. The role configures the built-in remote action
with `/system logging action set` and adds `/system logging` topic rules. It
does not remove existing topic rules or reconcile duplicates.

Store certificates and other sensitive values through Vault or approved
controller credentials. Test delivery, parsing, timestamps, and aggregator
retention in a lab before production rollout.

## Quality checks

```text
ansible-lint --profile production routeros_logging
ansible-playbook --syntax-check tests/test.yml
```
