# Ansible Role : deploy_firewall

[![CI](https://github.com/glillico/ansible-role-deploy_firewall/workflows/CI/badge.svg)](https://github.com/glillico/ansible-role-deploy_firewall/actions?query=workflow%3ACI)

Installs and configures either ufw or firewalld.

## Requirements

None.

## Role Variables

### defaults/main.yml

|Variable|Description|
|---|---|
|dpfw_pkgs|Specifies prerequisite packages to be installed.|
|dpfw_firewall_type|The type of  firewall that will be deployed.<br>(Options: firewalld, ufw,), (Default: firewalld).|
|dpfw_firewalld_backend|Specifies the backend firewalld will use.<br>(Options: nftables, iptables), (Default: iptables).|

## firewalld Rules

|Variable|Description|
|---|---|
|dpfw_firewalld_state|Should the rule be enabled or disabled.<br>(Options: absent, disabled, enabled, present).|
|dpfw_firewalld_port|The name or number of a port, can also be used to define a port range.<br>This should be in the format or *port/protocol* or *start_port-end_port/protocol* for port ranges.<br>(Default: null).|
|dpfw_firewalld_service|The name of a service.<br>(Default: null).|
|dpfw_firewalld_zone|The firewalld zone to add/remove the rule to/from.|

## ufw Rules

|Variable|Description|
|---|---|
|dpfw_ufw_rule|Add a firewall rule.<br>(Options: allow, deny, limit, reject), (Default: null).|
|dpfw_ufw_direction|The direction for the rule.<br>(Options: in, incoming, out, outgoing, routed), (Default: null).|
|dpfw_ufw_interface|The network interface for rule.|
|dpfw_ufw_from_ip|The source IP address.|
|dpfw_ufw_from_port|The source port|
|dpfw_ufw_to_ip|The destination IP address.|
|dpfw_ufw_to_port|The destination port.|
|dpfw_ufw_proto|The TCP/IP protocol for the rule.<br>(Options: any, tcp, udp, ipv6, esp, ah, gre, igmp), (Default: null).|
|dpfw_ufw_comment|A comment for the rule.|

## Examples
### firewalld Firewall

Enable the public zone to accept TCP connections on port 8080.
```
  - dpfw_firewalld_state: 'enabled'
    dpfw_firewalld_port: '8080/tcp'
    dpfw_firewalld_zone: 'public'
```

Disables the cockpit service on the public zone.
```
  - dpfw_firewalld_state: 'disabled'
    dpfw_firewalld_service: 'cockpit'
    dpfw_firewalld_zone: 'public'
```
### UFW Firewall

Limits the connections coming in on interfaec enp0s8 from 192.168.60.1 on TCP port 22.

```
  - dpfw_ufw_rule: 'limit'
    dpfw_ufw_direction: 'in'
    dpfw_ufw_interface: 'enp0s8'
    dpfw_ufw_from_ip: '192.168.60.1'
    dpfw_ufw_from_port: ''
    dpfw_ufw_to_ip: ''
    dpfw_ufw_to_port: '22'
    dpfw_ufw_proto: 'tcp'
    dpfw_ufw_comment: ''
```

## Dependencies

None.

## Example Playbook

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - glillico.deploy_firewall

## License

MIT

## Author Information

Created in 2024 by Graham Lillico.
