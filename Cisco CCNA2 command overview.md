Cisco CCNA2 Command Overview
============================================
About
-----
Round brackets () indicate required variables, square brackets [] indicate optional variables.  
Piping symbols  |  indicate options.  
All types of backquotes and asterisks in this source file are to be ignored; they are only relevant for Markdown rendering.

The command list presents the command, followed by a basic description of its functionality.

List of Commands
----------------

### Chapter 6 - Static Routing
Compiled By: Rob Oosthoek

#### IPv4

##### Verify Setup
- `Router #: show ip route`
- `Router #: show ip route static`
- `Router #: show running-config | section ip route`

##### Static Routes
- Next-Hop: use `ip-address-next-router`
- Directly-Connected: use `exit-int`

1. `Router #: conf t`
2. `Router (config)#: ip route (destination-ip) (subnet-mask) (ip-address-next-router | exit-int)`

##### Fully Specified Static Routes
1. `Router #: conf t`
2. `Router (config)#: ip route (destination-ip) (subnet-mask) (ip-address-next-router | exit-int)`

##### Default Static Routes
- Next-Hop: use `ip-address-next-router`
- Directly-Connected: use `exit-int`

1. `Router #: conf t`
2. `Router (config)#: ip route (destination-ip) (subnet-mask) (ip-address-next-router | exit-int)`

##### Static Routes Summarized
Attention: use the correct number of networking-bits with the subnet

1. `Router #: conf t`
2. `Router (config)#: ip route (destination-ip) (subnet-mask) (ip-address-next-router | exit-int)`

##### Floating Static Routes
- Next-Hop: `ip-address-next-router`
- Directly-Connected: `exit-int`

1. `Router #: conf t`
2. `Router (config)#: ip (destination-ip) (subnet-mask) (ip-address-next-router | exit-int)`
3. `Router (config)#: ip (destination-ip) (subnet-mask) (ip-address-next-router | exit-int) (admin-distance)`

Note: Routes configured with admin-distance will be used as backups should the primary route fail.

#### IPv6

##### Verifying
1. `Router #: show ipv6 route`
2. `Router #: show ipv6 route static`
3. `Router #: show running-config | section ipv6 route`

##### Static Routes
- Next-Hop: use `ipv6-address-next-router`
- Directly-Connected: use `exit-int`

1. `Router #: conf t`
2. `Router (config)#: ipv6 unicast-routing`
3. `Router (config)#: ipv6 route (destination-ipv6/prefix-length) (ipv6-address-next-router | exit-int)`

##### Fully Specified Static Routes
1. `Router #: conf t`
2. `Router (config)#: ipv6 unicast-routing`
3. `Router (config)#: ipv6 route (destination-ipv6/prefix-length) (subnet-mask) (exit-int) (link-local-next-router)`

##### Default Static Routes
- Next-Hop: use `ip-address-next-router`
- Directly-Connected: use `exit-int`

1. `Router #: conf t`
2. `Router (config)#: ipv6 unicast-routing`
3. `Router (config)#: ipv6 route ::/0 (ip-address-next-router | exit-int)`

##### Static Routes Summarized
Attention: use the correct number of networking-bits with the subnet

1. `Router #: conf t`
2. `Router (config)#: ipv6 unicast-routing`
3. `Router (config)#: ipv6 route (destination-ipv6/prefix-length) (ipv6-address-next-router | exit-int)`

----------

### Chapter 7 - Routing Dynamically  
Compiled By: Rob Oosthoek

#### IPV4 RIPv1/RIPv2

##### Configuring RIP
- `Router #: conf t`
- `Router (config)#: router rip`
- `Router (config-router)#: ?`

##### Advertising Networks
- `Router #: conf t`
- `Router (config)#: router rip`
- `Router (config-router)#: network 192.168.1.0`
- `Router (config-router)#: network 192.168.2.0`  
What interfaces the Router should use to communicate with other routers. These're also advertised.

##### Examining Default RIP Settings
- `Router #: Show ip protocols`
- `Router #: Show ip route | begin Gateway`

##### Enabling RIPv2
- `Router #: conf t`
- `Router (config)#: router rip`
- `Router (config)#: version 2`

##### Disabling Auto Summarization
- `Router #: conf t`
- `Router (config)#: router rip`
- `Router (config)#: no auto-summary`  
RIPv2 must be enabled to do this.

##### Configuring Passive Interfaces
- `Router #: conf t`
- `Router (config)#: router rip`
- `Router (config)#: passive-interface g0/0`  
RIP should only be sending updates to other routers. Int g0/0 no longer sends/receives updates.

##### Propagating A Default Route
- `Router #: conf t`
- `Router (config)#: ip route 0.0.0.0 0.0.0.0 exit-int next-hop-ip`
- `Router (config)#: router rip`
- `Router (config)#: default-information originate`  
This is like a Static Default Route, advertised to other routers.



#### IPV6 RIPng

##### Advertising IPv6 Networks
- `Router #: conf t`
- `Router (config)#: ipv6 unicast-routing`
- `Router (config)#: interface g0/0`
- `Router (config-if)#: ipv6 rip (domain-name) enable`
- `Router (config)#: interface s0/0/0`
- `Router (config-if)#: ipv6 rip (domain-name) enable`
- `Router (config-if)#: no shutdown`

##### Propagating A Default Route
- `Router #: conf t`
- `Router (config)#: ipv6 unicast-routing`
- `Router (config)#: ipv6 route 0::/0 2001:DB8:FEED:1::1 (ip-address-next-router -> niet zeker :S)`
- `Router (config)#: interface s0/0/0`
- `Router (config-if)#: ipv6 rip (domain-name) default-information originate`  
7.3.2.1 anyone? plz? XD

##### Examining RIPng Configuration
- `Router #: show ipv6 protocols`
- `Router #: show ipv6 route`
- `Router #: show ipv6 route rip`

----------

### Chapter 10
Compiled By: Gerard van Kempen

#### Basic PC/DOS Commands
1. `ipconfig /all`  
Shows the complete running network configuration.
2. `ipconfig /release`  
Releases all network settings so a new can be set up.
3. `ipconfig /renew`  
Sets up a new network configuration.

#### Basic DHCPv4 Configuration
##### Standard Router Configuration
1. `Router (config)#: ip dhcp excluded-address (low-address) [high-address]`  
Excludes a single multiple addresses from the DHCP-pool.
2. `Router (config)#: ip dhcp pool (poolname)`  
Creates a DHCP pool with the specified name and puts the router in DHCP-config mode.
3. `Router (dhcp-config)#: network (network-address) [network mask | /prefix length]`  
Defines the address pool for the DHCP server.
4. `Router (dhcp-config)#: default-router (address) [fallback-address1 ... fallback-address7]`  
Defines the default router or gateway for the DHCP server.
5. `Router (dhcp-config)#: dns-server (address) [fallback-address1 ... fallback-address7]`  
Defines the DNS server for the DHCP server.
6. `Router (dhcp-config)#: domain-name (name)`  
Change the domain name used by the DHCP server.
7. `Router (dhcp-config)#: lease (days) [hours] [minutes]`  
Sets the DHCP lease duration. Default is 1 day.
8. `Router (dhcp-config)#: lease infinite`  
Sets the DHCP lease duration to infinite.
9. `Router (dhcp-config)#: netbios-name-server (address) [fallback-address1 ... fallback-address7]`  
Defines the [netBIOS][netbios] WINS server.

##### Configuring a Router Port as DHCP Client
After selecting the interface to configure, enter:  
`Router (if-config)#: ip address dhcp`

#### Basic DCHPv4 Verification
1. `Router #: show running-config | section dhcp`  
Shows only the DHCP configuration of the running config.
2. `Router #: show ip dhcp binding`  
Shows the MAC to IP address bindings provided by the DHCP server.
3. `Router #: show ip dhcp server statistics`  
Displays information regarding the sent and received DHCP packets.  
This can be used to verify the DHCP configuration and activity.

#### Setting up DHCPv4 Relaying
DHCP relaying enables another router to forward incoming DHCP requests to the router functioning as the actual DHCP server.

1. Select an interface to use as a relay interface  
`Router (config)#: interface (interface)`
2. Define the ip helper-address  
`Router (if-config)#: ip helper-address (router-ip-address)`
3. End configuration  
`Router (if-config)#: end`
4. Verify the configuration  
`Router #: show ip interface (interface)`

A line should inform you of the configuration, eg.:  
`Helper address is 192.198.1.1`

Note: the `ip helper-address` command forwards the following UDP services by default:

- Port 37: Time
- Port 49: TACACS
- Port 53: DNS
- Port 67: DHCP/BOOTP client
- Port 68: DHCP/BOOTP server
- Port 69: TFTP
- Port 137: NetBIOS name service
- Port 138: NetBIOS datagram service

#### Troubleshooting a DHCPv4 Configuration
1. `Router #: show ip dhcp conflict`  
Returns all the address conflicts
2. `Router #: show running-config | include no service dhcp`  
Checks whether `no service dhcp` has __not__ been configured.  
If it has been configured, it would show up in the output.  
If all is OK, the output should remain empty.

##### Troubleshooting Workflow According to Cisco
1. Resolve address conflicts
2. Verify physical connectivity
3. Test with static address(es)
4. Verify switch port configuration
5. Test for same subnet or VLAN

#### Debugging a DHCPv4 Configuration
1. Create an ACL for debugging purposes:  
`Router (config)#: access-list (ACL-number) permit udp any any eq 67`  
`Router (config)#: access-list (ACL-number) permit udp any any eq 68`
2. Exit config and run:  
`Router #: debug ip packet (ACL-number)`

Other useful command: `debug ip dhcp server events`  
This command shows all server events related to DHCP requests.

#### [SLAAC][slaac] and DHCPv6
1. `ipv6 unicast-routing`  
Enables IPv6 routing
2. `no ipv6 nd managed-config-flag` and `no ipv6 nd other-config-flag`  
Applied on interface configuration. Resets SLAAC for that interface.
3. `ipv6 nd other-config-flag`  
Modify RA message to indicate Stateless DHCPv6. Applied in interface configuration.
4. `ipv6 nd managed-config-flag`  
Modify RA message to indicate Stateful DHCPv6. Overrides Stateless flag.

##### Notes on Stateless/Stateful DHCPv6
__Stateless DHCPv6 client__ - The client sends a DHCPv6 INFORMATION-REQUEST message to the DHCPv6 server requesting only configuration parameters, such as DNS server address. The client generated its own IPv6 address using the prefix from the RA message and a self-generated Interface ID.

__Stateful DHCPv6 client__ - The client sends a DHCPv6 REQUEST message to the server to obtain an IPv6 address and all other configuration parameters from the server.

#### Basic DHCPv6 Configuration
##### Stateless Server
1. Enable IPv6 routing: `Router (config)#: ipv6 unicast-routing`
2. Configure DHCPv6 pool: `Router (config)#: ipv6 dhcp pool (pool-name)`
3. Configure pool params: `Router (dhcp-config)#: dns-server (dns-address)`
4. Configure DHCPv6 interface:
  1. Select interface
  2. `Router (if-config)#: ipv6 dhcp server (pool-name)`
  3. `Router (if-config)#: ipv6 nd other-config-flag`

##### Stateless Client
1. Select interface
2. `Router (if-config)#: ipv6 enable`
3. `Router (if-config)#: ipv6 address autoconfig`
4. Verify config in privileged-exec: `Router #: show ipv6 dhcp pool`

##### Stateful Server
1. Enable IPv6 routing: `Router (config)#: ipv6 unicast-routing`
2. Configure DHCPv6 pool: `Router (config)#: ipv6 dhcp pool (pool-name)`
3. Configure Pool params: `Router (dhcp-config)#: address prefix (ipv6-prefix) [lifetime] [valid-lifetime ] [preferred-lifetime]`  
note: Lifetime params can all be replaced by `infinite` if needed.
4. Configure the DHCPv6 interface:
    1. Select interface
    2. `Router (if-config)#: ipv6 dhcp server (pool-name)`
    3. `Router (if-config)#: ipv6 nd managed-config-flag`

##### Stateful Client
1. Select interface
2. `Router (if-config)#: ipv6 enable`
3. `Router (if-config)#: ipv6 address dhcp`
4. Verify config in elevated-exec: `Router #: show ipv6 dhcp pool`

Other useful command: `Router #: show ipv6 dhcp binding `  
This command displays the automatic binding between the link-local address of the client and the address assigned by the server. 

#### Setting up DHCPv6 Relaying
1. Select interface to use as relay
2. `Router (if-config)#: ipv6 dhcp relay destination (ipv6-server-address)`
3. end

Verify relaying by running `show ipv6 dhcp interface (interface)`

#### Troubleshooting a DHCPv6 Configuration
1. `Router #: show ipv6 dhcp conflict`  
Returns all the address conflicts
2. `Router #: show ipv6 interface (interface)`  
Verifies the method of address allocation indicated in the RA message as indicated by the settings of the M and O flags.  

##### Troubleshooting Workflow According to Cisco
1. Resolve address conflicts
2. Verify allocation method
3. Test with static address(es)
4. Verify switch port configuration
5. Test for same subnet or VLAN

#### Debugging a DHCPv6 Configuration
1. `Router #: debug ipv6 dhcp detail`

[netbios]: http://www.wikiwand.com/en/NetBIOS
[slaac]: http://www.wikiwand.com/en/IPv6#/Stateless_address_autoconfiguration_.28SLAAC.29

---------

### Chapter 11
Compiled By: Gerard van Kempen

#### Configuring Static [NAT][nat]
1. `Router (config)#: ip nat inside source static (local-ip) (global-ip)`
2. `Router (config)#: interface (interface)`  
Specifies inside interface.
3. `Router (if-config)#: ip nat inside`  
Sets inside interface.
4. `Router (if-config)#: exit`
5. `Router (config)#: interface (interface)`  
Specifies outside interface.
6. `Router (if-config)#: ip nat outside`  
Sets outside interface.

#### Verifying Static NAT
- Option 1: `Router #: show ip nat translations`
- Option 2:
  1. `Router #: clear ip nat statistics`
  2. `Router #: show ip nat statistics`

#### Configuring Dynamic NAT
1. `Router (config)#: ip nat pool (name) [start-ip] [end-ip] [netmask (netmask) | prefix-length (prefixlength)]`
2. `Router (config)#: access-list (ACL-number) permit (source) [source-wildcard]`
3. `Router (config)#: ip nat inside source list (ACL-number) pool (name)`
4. Select interface, then: `Router (if-config)#: ip nat inside`
5. Select interface, then: `Router (if-config)#: ip nat outside`

#### Verifying Dynamic NAT
- Option 1: `Router #: show ip nat translations`
- Option 2:
  1. `Router #: clear ip nat statistics`
  2. `Router #: show ip nat statistics`
- Option 3:
  1. `Router #: clear ip nat translations`
  2. `Router #: show ip nat translations`

#### Configuring [PAT][pat]
##### Address Pool
1. `Router (config)#: ip nat pool (name) [start-ip] [end-ip] [netmask (netmask) | prefix-length (prefixlength)]`
2. `Router (config)#: access-list (ACL-number) permit (source) [source-wildcard]`
3. `Router (config)#: ip nat inside source list (ACL-number) pool (name) overload`
4. Select interface, then: `Router (if-config)#: ip nat inside`
5. Select interface, then: `Router (if-config)#: ip nat outside`

##### Single Address
1. `Router (config)#: access-list (ACL-number) permit (source) [source-wildcard]`
2. `Router (config)#: ip nat inside source list (ACL-number) interface (interface) overload`
3. Select interface, then: `Router (if-config)#: ip nat inside`
4. Select interface, then: `Router (if-config)#: ip nat outside`

#### Verifying PAT
- Option 1:
  1. `Router #: clear ip nat statistics`
  2. `Router #: show ip nat statistics`
- Option 2:
  1. `Router #: clear ip nat translations`
  2. `Router #: show ip nat translations`

#### Port Forwarding in IOS
`ip nat inside source static (tcp | udp [local-ip] [local-port] [global-ip] [global-port]) [extendable]`

#### Troubleshooting NAT
Use any combination of following commands to pinpoint the issue:

- `Router #: show ip nat translations`
- `Router #: show ip nat statistics`
- `Router #: clear ip nat translation *`
- `Router #: clear ip nat statistic`
- `Router #: debug ip nat detailed` (debugging)



[nat]: http://www.wikiwand.com/en/Network_address_translation
[pat]: http://www.wikiwand.com/en/Network_address_translation#/Port_address_translation

