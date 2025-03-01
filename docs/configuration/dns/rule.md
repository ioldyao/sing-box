---
icon: material/new-box
---

!!! quote "Changes in sing-box 1.9.0"

    :material-plus: [geoip](#geoip)  
    :material-plus: [ip_cidr](#ip_cidr)  
    :material-plus: [ip_is_private](#ip_is_private)  
    :material-plus: [client_subnet](#client_subnet)

!!! quote "Changes in sing-box 1.8.0"

    :material-plus: [rule_set](#rule_set)  
    :material-plus: [source_ip_is_private](#source_ip_is_private)  
    :material-delete-clock: [geoip](#geoip)  
    :material-delete-clock: [geosite](#geosite)

### Structure

```json
{
  "dns": {
    "rules": [
      {
        "inbound": [
          "mixed-in"
        ],
        "ip_version": 6,
        "query_type": [
          "A",
          "HTTPS",
          32768
        ],
        "network": "tcp",
        "auth_user": [
          "usera",
          "userb"
        ],
        "protocol": [
          "tls",
          "http",
          "quic"
        ],
        "domain": [
          "test.com"
        ],
        "domain_suffix": [
          ".cn"
        ],
        "domain_keyword": [
          "test"
        ],
        "domain_regex": [
          "^stun\\..+"
        ],
        "geosite": [
          "cn"
        ],
        "source_geoip": [
          "private"
        ],
        "geoip": [
          "cn"
        ],
        "source_ip_cidr": [
          "10.0.0.0/24",
          "192.168.0.1"
        ],
        "source_ip_is_private": false,
        "ip_cidr": [
          "10.0.0.0/24",
          "192.168.0.1"
        ],
        "ip_is_private": false,
        "source_port": [
          12345
        ],
        "source_port_range": [
          "1000:2000",
          ":3000",
          "4000:"
        ],
        "port": [
          80,
          443
        ],
        "port_range": [
          "1000:2000",
          ":3000",
          "4000:"
        ],
        "process_name": [
          "curl"
        ],
        "process_path": [
          "/usr/bin/curl"
        ],
        "package_name": [
          "com.termux"
        ],
        "user": [
          "sekai"
        ],
        "user_id": [
          1000
        ],
        "clash_mode": "direct",
        "wifi_ssid": [
          "My WIFI"
        ],
        "wifi_bssid": [
          "00:00:00:00:00:00"
        ],
        "rule_set": [
          "geoip-cn",
          "geosite-cn"
        ],
        "invert": false,
        "outbound": [
          "direct"
        ],
        "server": "local",
        "disable_cache": false,
        "rewrite_ttl": 100,
        "client_subnet": "127.0.0.1"
      },
      {
        "type": "logical",
        "mode": "and",
        "rules": [],
        "server": "local",
        "disable_cache": false,
        "rewrite_ttl": 100,
        "client_subnet": "127.0.0.1"
      }
    ]
  }
}

```

!!! note ""

    You can ignore the JSON Array [] tag when the content is only one item

### Default Fields

!!! note ""

    The default rule uses the following matching logic:  
    (`domain` || `domain_suffix` || `domain_keyword` || `domain_regex` || `geosite`) &&  
    (`port` || `port_range`) &&  
    (`source_geoip` || `source_ip_cidr` ｜｜ `source_ip_is_private`) &&  
    (`source_port` || `source_port_range`) &&  
    `other fields`

    Additionally, included rule sets can be considered merged rather than as a single rule sub-item.

#### inbound

Tags of [Inbound](/configuration/inbound/).

#### ip_version

4 (A DNS query) or 6 (AAAA DNS query).

Not limited if empty.

#### query_type

DNS query type. Values can be integers or type name strings.

#### network

`tcp` or `udp`.

#### auth_user

Username, see each inbound for details.

#### protocol

Sniffed protocol, see [Sniff](/configuration/route/sniff/) for details.

#### domain

Match full domain.

#### domain_suffix

Match domain suffix.

#### domain_keyword

Match domain using keyword.

#### domain_regex

Match domain using regular expression.

#### geosite

!!! failure "Deprecated in sing-box 1.8.0"

    Geosite is deprecated and may be removed in the future, check [Migration](/migration/#migrate-geosite-to-rule-sets).

Match geosite.

#### source_geoip

!!! failure "Deprecated in sing-box 1.8.0"

    GeoIP is deprecated and may be removed in the future, check [Migration](/migration/#migrate-geoip-to-rule-sets).

Match source geoip.

#### source_ip_cidr

Match source IP CIDR.

#### source_ip_is_private

!!! question "Since sing-box 1.8.0"

Match non-public source IP.

#### source_port

Match source port.

#### source_port_range

Match source port range.

#### port

Match port.

#### port_range

Match port range.

#### process_name

!!! quote ""

    Only supported on Linux, Windows, and macOS.

Match process name.

#### process_path

!!! quote ""

    Only supported on Linux, Windows, and macOS.

Match process path.

#### package_name

Match android package name.

#### user

!!! quote ""

    Only supported on Linux.

Match user name.

#### user_id

!!! quote ""

    Only supported on Linux.

Match user id.

#### clash_mode

Match Clash mode.

#### wifi_ssid

!!! quote ""

    Only supported in graphical clients on Android and iOS.

Match WiFi SSID.

#### wifi_bssid

!!! quote ""

    Only supported in graphical clients on Android and iOS.

Match WiFi BSSID.

#### rule_set

!!! question "Since sing-box 1.8.0"

Match [Rule Set](/configuration/route/#rule_set).

#### invert

Invert match result.

#### outbound

Match outbound.

`any` can be used as a value to match any outbound.

#### server

==Required==

Tag of the target dns server.

#### disable_cache

Disable cache and save cache in this query.

#### rewrite_ttl

Rewrite TTL in DNS responses.

#### client_subnet

!!! question "Since sing-box 1.9.0"

Append a `edns0-subnet` OPT extra record with the specified IP address to every query by default.

Will overrides `dns.client_subnet` and `servers.[].client_subnet`.

### Address Filter Fields

Only takes effect for IP address requests. When the query results do not match the address filtering rule items, the current rule will be skipped.

!!! note ""

    `ip_cidr` items in included rule sets also takes effect as an address filtering field.

#### geoip

!!! question "Since sing-box 1.9.0"

Match GeoIP with query response.

#### ip_cidr

!!! question "Since sing-box 1.9.0"

Match IP CIDR with query response.

#### ip_is_private

!!! question "Since sing-box 1.9.0"

Match private IP with query response.

### Logical Fields

#### type

`logical`

#### mode

`and` or `or`

#### rules

Included rules.