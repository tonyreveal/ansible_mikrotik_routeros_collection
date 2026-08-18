# Changelog

All notable changes to the `mikrotik.routeros` collection are documented in
this file.

## [1.3.0] - 2026-08-18

### Added

- Added `routeros_rollback` for installing an explicitly selected RouterOS
  package version and aligning RouterBOARD firmware with that package.
- Added checksum, reboot, reconnect, and final-state validation for rollback
  operations.

### Updated

- Bumped the collection version to `1.3.0`.

## [1.2.0] - 2026-08-18

### Added

- Added `routeros_bgp` for BGP connection configuration.
- Added `routeros_ospf` for OSPF instance configuration.
- Added `routeros_static_routes` for static route configuration.

### Removed

- Removed the combined `routeros_routing` role in favor of focused routing roles.
- Removed bond creation from `routeros_mlag`.
- Removed the redundant `routeros_maintenance` role; package and RouterBOARD
  upgrades are provided by `routeros_upgrade`.

### Updated

- Reduced `routeros_baseline` to core identity configuration; DNS and NTP remain
  in their dedicated roles.
- Added optional `mlag_id` support to `routeros_bonding`.
- Updated affected role READMEs and the alphabetized collection role index.
- Bumped the collection version to `1.2.0`.

## [1.1.0] - 2026-08-18

### Added

- Added `routeros_alert_destinations` for RouterOS email alert delivery.
- Added `routeros_bonding` for bonded interface configuration.
- Added `routeros_bridge` for bridge interface creation.
- Added `routeros_dhcp_relay` for DHCP relay configuration.
- Added `routeros_dhcp_server` for DHCP pools, networks, and server instances.
- Added `routeros_dns` for DNS resolver settings and static entries.
- Added `routeros_firewall` for structured firewall address lists, filter rules, and NAT rules.
- Added `routeros_logging` for remote syslog and CEF log forwarding.
- Added `routeros_mlag` for RouterOS 7.22+ MLAG bridge configuration.
- Added `routeros_netwatch` for Netwatch monitoring entries.
- Added `routeros_ntp` for NTP client settings and server configuration.
- Added `routeros_snmp` for SNMP configuration.
- Added `routeros_traffic_flow` for traffic-flow export settings and targets.
- Added `routeros_wireguard` for WireGuard interfaces and peers, including peer-only configuration.

### Removed

- Removed the `routeros_monitoring` role because it combined unrelated logging, SNMP, and Netwatch responsibilities. These capabilities are now provided by focused roles.
- Removed WireGuard implementation from `routeros_vpn` to eliminate duplicate functionality.
- Removed bridge creation from `routeros_vlan`; bridges are now created by `routeros_bridge`.

### Updated

- Refactored `routeros_access` to accept structured users, SSH settings, and API service settings instead of raw command lists.
- Updated `routeros_backup` to append a UTC timestamp in `YYYYMMDD-HHMM` format to exported filenames.
- Refactored `routeros_vpn` to focus on IPsec peer configuration.
- Updated `routeros_vlan` to require an existing `routeros_vlan_bridge_name` and apply VLAN configuration to that bridge.
- Added detailed role READMEs with requirements, examples, variable structures, operational notes, and quality checks.
- Standardized role metadata with Tony Reveal as author, GPL-3.0-or-later licensing, Ansible 2.16 minimum, and RouterOS/MikroTik/network Galaxy tags.
- Standardized YAML document markers and task spacing across role files.
- Added structured collection README documentation following the Ansible partner certification template.
- Documented testing against RouterOS 7.23.3 and 7.24.
- Pinned the `community.routeros` collection dependency to `==3.21.0` in `galaxy.yml`.
- Declared `requires_ansible: '>=2.16'` in `meta/runtime.yml`.
- Updated the collection version from `1.0.0` to `1.1.0`.

### Compatibility notes

- The `routeros_mlag` role supports RouterOS 7.22 and newer only.
- All roles require Ansible Core 2.16 or newer.
- RouterOS command syntax and feature availability should be validated separately for RouterOS major versions and supported hardware platforms.

## [1.0.0]

- Initial collection release.
