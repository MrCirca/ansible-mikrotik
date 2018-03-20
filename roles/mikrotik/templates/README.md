Inventory Example
For routeros_users:
```
routeros_users:
  - name: "admin"
    password: "123321" 
```
For ethernet interfaces
```
routeros_ethernet:
  - name: adsl1
    interface_number: 5
  - name: adsl2
    interface_number: 6
```
For bridge interfaces
```
routeros_bridge_interfaces:
  bridge-LAN:
    ports:
      - ether0
      - ether1
      - ether2
    ip_address: "192.168.100.1"
    gw: "192.168.100.1"
    subnet: 192.168.100.0/24
  bridge_guests:
    ports:
      - ether0
      - ether1
      - ether2
    ip_address: "192.168.200.1/24"
    gw: "192.168.200.1"
    net: "192.168.200.0"
```
For router identity
```
routeros_identity: test_router
```
For DHCP server
```
routeros_dhcp:
  main_dhcp:
    pool_name: main-pool
    ip_range: "192.168.2.2-192.168.2.254"
    interface: bridge1-LAN
  guest_dhcp:
    pool_name: guest-pool
    ip_range: "192.168.4.2-192.168.4.254"
    interface: bridge_guests
```
For wireless
```
routeros_wireless:
  main:
    ssid: "mikrotik_wifi"
    wpa: "test_wpa_main"
  guest:
    ssid: "mikrotik_guest_wifi"
    wpa: "test_wpa_guest"
```
For pppoe-client:
```
routeros_adsl:
  - interface: adsl1
    name: Vodafone
    username: username_adsl1
    password: password_adsl1
    down: 15000
    up: 1000

  - interface: adsl2
    name: Wind
    username: username_adsl2
    password: password_adsl2
    down: 35000
    up: 4000

```
For vpn
```
routeros_vpn:
  - server: vpn.mrcirca.test
    username: user_vpn
    password: pass_vpn
    name: test_vpn
```
For routes
```
routeros_routes:
  - name: Test-VPN
    dst_address: 172.17.0.0/16
    gateway: 172.17.1.1
    distance: 1
  - name: ADSL1
    dst_address: 0.0.0.0/0
    gateway: test_gateway
    distance: 1
  - name: ADSL2
    dst_address: 0.0.0.0/0
    gateway: test2
    distance: 10
    routing_mark: dsl1-only

```
For firewall
```
routeros_firewall:
  address_lists:
    critical:
      - 192.168.0.0/24
    medium:
      - 192.168.1.0/24
    non-critical:
      - 192.168.1.2/24
  chains:
    src-nat:
      - comment: comment_test1
  address_lists:
    critical:
      - 192.168.0.0/24
    medium:
      - 192.168.1.0/24
    non-critical:
      - 192.168.1.2/24
  chains:
    src-nat:
      - comment: comment_test1
        action: masquerade
        out_interface: adsl1
        disabled: "no"
      - comment: comment_test2
        action: masquerade
        out_interface: adsl2
        disabled: "no"
    postrouting:
      - comment: fefefefe
        action: mark-packet
        out_interface: adsl1
        src_address_list: medium
        dscp:
          - 6
          - 11
          - 24
          - 26
          - 40
          - 46
          - 48
        disabled: "no"
        passthrough: "yes"
        tcp_flags:
      - comment:
        action: mark-packet
        out_interface: adsl2
        connection_type: sip
        src_address_list: critical
        protocol: udp
        dscp:
          - 11
          - 6
          - 46
          - 40
          - 48
          - 24
          - 26
        disabled: "no"
        passthrough: "yes"
    forward:
      - comment:
        action: mark-packet
        in_interface: adsl1
        src_address_list: medium
        disabled: "no"
        dst_address_list: critical
        passthrough: "yes"
      - comment:
        action: mark-packet
        in_interface: adsl2
        disabled: "no"
        dst_address_list: medium
        passthrough: "yes"

```
