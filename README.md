# Dnsmasq on CentOS7/RHEL

This is an Ansible role for installing and configreDnsmasq on CentOS/RHEL 7.

## Role Variables

If you do not specify variables, then the default values will be used.

| Variable                   | Default | Comments                                                                                                                                                  |
| :---                       | :---    | :---                                                                                                                                                      |
| `dnsmasq_addn_hosts`       | -       | Set this to specify a custom host file that should be read in addition to `/etc/hosts`.                                                                   |
| `dnsmasq_authoritative`    | `false` | When `true`, dnsmasq will function as an authoritative name server.                                                                                       |
| `dnsmasq_bogus_priv`       | `true`  | When `true`, Dnsmasq will not forward addresses in the non-routed address spaces.                                                                         |
| `dnsmasq_dhcp_hosts`       | -       | Array of hashes specifying IP address reservations for hosts, with keys `name` (optional), `mac` and `ip` for each reservation. See below.             |
| `dnsmasq_dhcp_ranges`      | -       | Array of hashes specifying DHCP ranges (with keys `start_addr`, `end_addr`, and `lease_time`) for each address pool. This also enables DHCP. See below. |
| `dnsmasq_domain_needed`    | `true`  | When `true`, local requests (i.e. without domain name) are not forwarded.                                                                                 |
| `dnsmasq_domain`           | -       | The domain for Dnsmasq.                                                                                                                                   |
| `dnsmasq_expand_hosts`     | `false` | Set this (and `dnsmasq_domain`) if you want to have a domain automatically added to simple names in a hosts-file.                                         |
| `dnsmasq_listen_address`   | -       | The IP address of the interface that should listen to DNS/DHCP requests.                                                                                  |
| `dnsmasq_interface`        | -       | The network interface that should listen to DNS/DHCP requests.                                                                                            |
| `dnsmasq_option_router`    | -       | The default gateway to be sent to clients.                                                                                                                |
| `dnsmasq_port`             | -       | Set this to listen on a custom port.                                                                                                                      |
| `dnsmasq_resolv_file`      | -       | Set this to specify a custom `resolv.conf` file.                                                                                                          |
| `dnsmasq_srv_hosts`        | -       | Array of hashes specifying SRV records, with keys `name` (mandatory), `target`, `port`, `priority` and `weight` for each record. See below.              |
| `dnsmasq_upstream_servers` | -       | Set this to specify the IP address of upstream DNS servers directly. You can specify one ore more servers as a list.                                    |


### DNS settings

One or more upstream DNS servers can can be specified with the variable `dnsmasq_server`:

```Yaml
    dnsmasq_upstream_servers: ns1.example.ru
  OR
    dnsmasq_upstream_servers:
      - 8.8.4.4
      - 8.8.8.8
```

SRV records can be specified with `dnsmasq_srv_hosts`:

```Yaml
    dnsmasq_srv_hosts:
      - name: _ldap._tcp.example.ru
        target: ldap1.example.ru
        port: 389
      - name: _ldap._tcp.example.ru
        target: ldap2.example.ru
        port: 389
```

### DHCP settings

A DHCP range can be specified with the variable `dnsmasq_dhcp_ranges`:

```Yaml
    dnsmasq_dhcp_ranges:
      - start_addr: '192.168.3.50'
        end_addr: '192.168.3.200'
        lease_time: '9h'
```

IP address reservations based on MAC addres can be specified with `dnsmasq_dhcp_hosts`:

```Yaml
    dnsmasq_dhcp_hosts:
      - name: 'belka'
        mac: '11:22:33:44:55:66'
        ip: '192.168.3.20'
      - name: 'strelka'
        mac: 'aa:bb:cc:dd:ee:ff'
        ip: '192.168.3.21'
```

## Example Playbook

Most Dnsmasq settings have working defaults and don't have to be specified. The simplest configuration would be a DNS forwarder with default settings:

```Yaml
- hosts: server
  roles:
    - rhel_dnsmasq
```

`Alexandr Ivanov. 2021`
