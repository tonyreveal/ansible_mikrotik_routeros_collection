# RouterOS roles

This collection provides focused roles for common RouterOS automation functions:

- `routeros_backup` — configuration exports
- `routeros_baseline` — organization baseline commands
- `routeros_firewall` — firewall, address-list, and NAT policy
- `routeros_vpn` — VPN and tunnel configuration
- `routeros_routing` — static and dynamic routing configuration
- `routeros_vlan` — bridges, VLANs, trunks, and access ports
- `routeros_access` — administrative users and services
- `routeros_snmp` — SNMP configuration
- `routeros_netwatch` — Netwatch monitoring entries
- `routeros_traffic_flow` — traffic-flow export configuration
- `routeros_alert_destinations` — email alert delivery configuration
- `routeros_compliance` — compliance evidence collection
- `routeros_inventory` — platform and version inventory
- `routeros_incident_response` — explicit emergency actions
- `routeros_maintenance` — controlled maintenance and reboot actions
- `routeros_firewall_config` — structured firewall and NAT configuration
- `routeros_wireguard` — WireGuard interface and peer configuration
- `routeros_dhcp_relay` — DHCP relay configuration
- `routeros_dhcp_server` — DHCP pools, networks, and server configuration
- `routeros_dns` — DNS resolver and static entry configuration
- `routeros_ntp` — NTP client and server configuration
- `routeros_logging` — remote syslog and CEF logging configuration

The mutating roles intentionally accept reviewed RouterOS command lists through
namespaced variables. This keeps device-specific desired state in inventory or
group variables, avoids secrets in role code, and gives consuming playbooks a
stable functional interface. Because arbitrary command lists cannot be made
idempotent by a wrapper role, production repositories should evolve high-value
commands into resource-specific tasks with state checks and tests.

All roles require the `community.routeros` collection and a configured
`ansible.netcommon.network_cli` connection.
