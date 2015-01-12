Cisco CCNA2 command overview - Chapter 10-11
============================================
About
-----
Round brackets () indicate required variables, square brackets [] indicate optional variables.  
All types of backquotes and asterisks in this source file are to be ignored; they are only relevant for Markdown rendering.

The command list presents the command, followed by a basic description of its functionality.

List of Commands
----------------

----------

### Chapter 10

#### Basic PC/DOS Commands
1. `ipconfig /all`  
Shows the complete running network configuration.
2. `ipconfig /release`  
Releases all network settings so a new can be set up.
3. `ipconfig /renew`  
Sets up a new network configuration.

#### Basic DHCPv4 Configuration
##### Standard Router Configuration
1. `ip dhcp excluded-address (low-address) [high-address]`  
Excludes a single multiple addresses from the DHCP-pool.
2. `ip dhcp pool (poolname)`  
Creates a DHCP pool with the specified name and puts the router in DHCP-config mode.
3. `network (network-number) [network mask | /prefix length]`  
Defines the address pool for the DHCP server.
4. `defaul-router (address) [fallback-address1 ... fallback-address7]`  
Defines the default router or gateway for the DHCP server.
5. `dns-server (address) [fallback-address1 ... fallback-address7]`  
Defines the DNS server for the DHCP server.
6. `domain-name (name)`  
Change the domain name used by the DHCP server.
7. `lease (days) [hours] [minutes]`  
Sets the DHCP lease duration. Default is 1 day.
8. `lease infinite`  
Sets the DHCP lease duration to infinite.
9. `netbios-name-server (address) [fallback-address1 ... fallback-address7]`  
Defines the [netBIOS][1] WINS server.

##### Configuring a Router Port as DHCP Client
After selecting the interface to configure, enter:  
`ip address dhcp`

#### Basic DCHPv4 Verification
1. `show running-config | section dhcp`  
Shows only the DHCP configuration of the running config.
2. `show ip dhcp binding`  
Shows the MAC to IP address bindings provided by the DHCP server.
3. `show ip dhcp server statistics`  
Displays information regarding the sent and received DHCP packets.  
This can be used to verify the DHCP configuration and activity.

#### Setting up DHCPv4 Relaying
DHCP relaying enables another router to forward incoming DHCP requests to the router functioning as the actual DHCP server.

1. Select an interface to use as a relay interface  
`interface (interface)`
2. Define the ip helper-address  
`ip helper-address (router-ip-address)`
3. End configuration  
`end`
4. Verify the configuration  
`show ip interface (interface)`

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
1. `show ip dhcp conflict`  
Returns all the address conflicts
2. `show running-config | include no service dhcp`  
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
`access-list (ACL-number) permit udp any any eq 67`  
`access-list (ACL-number) permit udp any any eq 68`
2. Exit config and run:  
`debug ip packet (ACL-number)`

Other useful command: `debug ip dhcp server events`  
This command shows all server events related to DHCP requests.

#### [SLAAC][2] and DHCPv6
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
1. Enable IPv6 routing: `ipv6 unicast-routing`
2. Configure DHCPv6 pool: `ipv6 dhcp pool (pool-name)`
3. Configure pool params: `dns-server (dns-address)`
4. Configure DHCPv6 interface:
  1. Select interface
  2. `ipv6 dhcp server (pool-name)`
  3. `ipv6 nd other-config-flag`

##### Stateless Client
1. Select interface
2. `ipv6 enable`
3. `ipv6 address autoconfig`
4. Verify config in elevated-exec: `show ipv6 dhcp pool`

##### Stateful Server
1. Enable IPv6 routing: `ipv6 unicast-routing`
2. Configure DHCPv6 pool: `ipv6 dhcp pool (pool-name)`
3. Configure Pool params: `address prefix (ipv6-prefix) [lifetime] [valid-lifetime ] [preferred-lifetime]`  
note: Lifetime params can all be replaced by `infinite` if needed.
4. Configure the DHCPv6 interface:
    1. Select interface
    2. `ipv6 dhcp server (pool-name)`
    3. `ipv6 nd managed-config-flag`

##### Stateful Client
1. Select interface
2. `ipv6 enable`
3. `ipv6 address dhcp`
4. Verify config in elevated-exec: `show ipv6 dhcp pool`

Other useful command: `show ipv6 dhcp binding `  
This command displays the automatic binding between the link-local address of the client and the address assigned by the server. 

#### Setting up DHCPv6 Relaying
1. Select interface to use as relay
2. `ipv6 dhcp relay destination (ipv6-server-address)`
3. end

Verify relaying by running `show ipv6 dhcp interface (interface)`

#### Troubleshooting a DHCPv6 Configuration
1. `show ipv6 dhcp conflict`  
Returns all the address conflicts
2. `show ipv6 interface (interface)`  
Verifies the method of address allocation indicated in the RA message as indicated by the settings of the M and O flags.  

##### Troubleshooting Workflow According to Cisco
1. Resolve address conflicts
2. Verify allocation method
3. Test with static address(es)
4. Verify switch port configuration
5. Test for same subnet or VLAN

#### Debugging a DHCPv6 Configuration
1. `debug ipv6 dhcp detail`

[1]: http://www.wikiwand.com/en/NetBIOS
[2]: http://www.wikiwand.com/en/IPv6#/Stateless_address_autoconfiguration_.28SLAAC.29

--------

