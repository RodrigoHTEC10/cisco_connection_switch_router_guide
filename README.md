# Guide for Cisco's Router and Switch Configuration

The current README.md file corresponds to the guide for Cisco's Router and Switch configuration, including a Global Configuration and particular configurations for diverse scenarios.

# Global Configuration

The following section corresponds to the global configuration for any network device from Cisco.

1. Initial identification tag. (Documentation)

    ```
    !================= Router Configuration ===============
    !=============== <Organization's Name> ================
    !Author: <Name>
    !Date: <Date>
    !Objective: <Objective>
    !======================================================

    ```

2. Access the privileged mode of the device.

    ```
    enable
    ```

3. Set time and date

    ```
    clock set HH:MM:SS <Month. E. Jun> <Day> <Year>
    ```

4. Access the terminal configuration
    ```
    configure terminal 
    ```

5. Name the device
    ```
    hostname <Device Name>
    ```

6. Desactivar busqueda por DNS 
    ```
    no ip domain lookup
    ```

7. Creation of the privileged mode access password.

    For this section the created password to access the privileged mode of the network device through the command <code>enable</code> can be encripted or not.

    | Encrypted password | Non-encrypted password |
    |-------|-------|
    | <pre><code> enable secret &lt;password&gt; </code></pre> | <pre><code> enable password &lt;password&gt; </code></pre> |

8. Establish a banner for initial presentation. 
    
    This is set as the 'message of the day' (motd). Enclose the message between # #
    ```
    banner motd #

    |====== UNAUTHORIZED ACCESS TO THIS DEVICE IS PROHIBITED ======|
    |==============================================================|
    |  Please contact the system administrator for any concerns.   |
    |  Administrator. Rodrigo Hurtado                              |
    |                                                              |
    |  For more configurations, contant me at                      |
    |  hurtadorodrigo.dev@gmail.com                                |
    |==============================================================|

    #
    ```

9. Creation of the accounts' database for authorized access

    Create each of the following lines per user.
    ```
    username <username> privilege <(lower) 0-15 (higher)> secret <password>
    ```

10. Creation of an ip domain name. 

    This will be used to configure remote access by the protocol <code>ssh</code>.
    ```
    ip domain name <domain_name E. tec.com.mx>
    ```

11. Establishment of the encryption method.

    | Inside of Cisco Packet Tracer | Outside of Cisco Packet Tracer |
    |-------|-------|
    | <pre><code>crypto key generate rsa <br>yes<br>&lt;516 / 1024 / 2048&gt;</code></pre> | <pre><code>crypto key generate rsa mod &lt;516 / 1024 / 2048&gt;</code></pre> |

12. Establishment of a mistaken access trial limit

    In order to increase the security of the network devices, establish a limit of error when logging into the system and applying an automatic delay to gain access for the next trial is a good alternative.

    Here the number of trials and the time given between those trials as well as the waiting time can be configured.

    ```
    login  block-for <seconds_block> attemps <attemps_number_mistaken> within <seconds_mistakes>
    ```

<br>

## 13. Console Access Control

  13.1 Access to the line console configuration.

  The number 0 refers to the port of the network device responsible for the console management. Generally there are two ports 0 and 1 although only one of them can be configured and active at a time.

  ```
  line console 0
  ```

  13.2 Establish an execution time after which the console will logout.

  ```
  exec-timeout <number of minutes>
  ```

  13.3 Configuring access to the line console.

  There are two alternatives:
  - Configure local access based on the users database.
  - Configure an exclusive password for the line console. Available if skipped over step 8. The password might be encrypted or not.

  | Access with Local Users | Access with Password Not Encrypted | Access with Password Encrypted |
  |-------|-------|-------|
  | <pre><code> login local </code></pre> | <pre><code> password &lt;password&gt; <br> login </code></pre> | <pre><code> secret &lt;password&gt; <br> login </code></pre> |


  13.4 Login into the system.

  This command allows to hold the console messages after a process is completed instead of interrumpting it inmediately.

  ```
  logging synchronous
  ```

*General Console Configuration in Port 0*

```
line console 0
  [password <password> | secret <password>]
  login local [login]
  exec-timeout <minutes>
  logging synchronous

```
<br>

## 14. Virtual Console Configuration for Remote Access

  14.1 Access the remote console configuration with number of available connections. Numbers are quantity between two given numbers starting in 0.

  E. 0 5 = 6 connections, 0 1 = 2 connections.

  Following *Cisco's Standard* for Routers a total of recommended connections is 5 (0 4) while for switches is 16 (0 15); however, the recommendation based on security is keep it as low as possible for the only existent users declared in the users database.

  ```
  line vty 0 2
  ```

  14.2 Establish an execution time after which the console will logout.

  ```
  exec-timeout <number of minutes>
  ```

  14.3 Configuring transport method for remote access.

  The default transport method is <code>telnet</code>; however, this protocol is very insecure as it ahs no inscription and is easily detectable. Instead it is passed to the <code>ssh</code> (Secure SHell) which requires the encryption previously declared with crypto key.

  ```
  transport input ssh
  ```

  14.4 Configuring access to the line console.

  There are two alternatives:
  - Configure local access based on the users database.
  - Configure an exclusive password for the line console. Available if skipped over step 8. The password might be encrypted or not.

  | Access with Local Users | Access with Password Not Encrypted | Access with Password Encrypted |
  |-------|-------|-------|
  | <pre><code> login local </code></pre> | <pre><code> password &lt;password&gt; <br> login </code></pre> | <pre><code> secret &lt;password&gt; <br> login </code></pre> |


  14.5 Login into the system.

  This command allows to hold the console messages after a process is completed instead of interrumpting it inmediately.

  ```
  logging synchronous
  ```

*General Remote Console Access for 3 Connections*

```
line vty 0 2
  exec-timeout 2
  transport input ssh
  login local
  logging synchronouss
```
---
<br>

15. Activate password encryption in the device

```
service password-encryption 
```

<br>

## Summary

This marks the end of the global configuration. A complete version of this configuration for Cisco Packet Tracer is presented below:

```{ios}
!================= Router Configuration ===============
!=============== <Organization's Name> ================
!Author: <Name>
!Date: <Date>
!Objective: <Objective>
!======================================================

!================ Configuración General =====================
enable

clock set HH:MM:SS <Month. E. Jun> <Day> <Year>

configure terminal

hostname <Device Name>

no ip domain lookup

enable [secret|password] <password>

banner motd #

|====== UNAUTHORIZED ACCESS TO THIS DEVICE IS PROHIBITED ======|
|==============================================================|
|  Please contact the system administrator for any concerns.   |
|  Administrator. Rodrigo Hurtado                              |
|                                                              |
|  For more configurations, contant me at                      |
|  hurtadorodrigo.dev@gmail.com                                |
|==============================================================|

#

username <username> privilege <(lower) 0-15 (higher)> secret <password>
username <username> privilege <(lower) 0-15 (higher)> secret <password>
username <username> privilege <(lower) 0-15 (higher)> secret <password>

ip domain name <domain_name E. tec.com.mx>

crypto key generate rsa
yes
1024

login  block-for <seconds_block> attemps <attemps_number_mistaken> within <seconds_mistakes>

line console 0
  login local
  exec-timeout 2
  logging synchronous
  
line vty 0 2
  exec-timeout 2
  transport input ssh
  login local
  logging synchronouss

service password-encryption 

!================ Fin de Configuración General =====================

```

<br>
<br>
<br>

# Router Configuration

The configuration of a router may suit for different needs, for such cause the current section is divided into smaller sections that allow to handle different configurations, mainly focusing on defining and configuring subinterfaces for VLANs, declaring physical interfaces and declaring DHCP protocol.

## Physical interfaces configuration

This section is generally established to configure any required port without any specific connection.

```
interface <port_name as G0/1>
  description <description>
  ip address <IP> <MSK>
  no shutdown

```
<br>

### Serial ports  - Device Communication Equipment
This specific configuration which only adds the command <code>clock rate</code> is only established in the DCE sides of a serial connection which is the speed of communication through the DCE and the DCT (Device Communication Terminal).

```
interface <serial_port_name as S0/0/0>
  description <description>
  clock rate <64000 | 128000 | 256000 | 512000 | 2000000>
  ip address <IP> <MSK>
  no shutdown

```
<br>

The clock rate can also be configured in physical interfaces other than Serial that are DCE of a Device Communication arrange.

### Subinterfaces Configuration

Subinterfaces are declared when a port of the Router is the default gateway of several Virtual Local Area Networks (VLANs). 

One subinterface must be declared per each VLAN in the network. Each of these subinterfaces must have its own IP address and MSK that comes from the respective VLAN logical distribution (generally logical direction with VLMS, and last or first IP address).

The encapsulation protocol <code>dot1q</code> allows to handle VLAN communication correctly between Cisco and Non-Cisco devices.

```
interface <physical_port>.<VID>
    description <Description>
    encapsulation dot1q <VID>
    ip address <IP> <MSK>
```

Once all the subinterfaces of a physical interface or port have been declared, then the port itself is bringed up.

```
interface <physical_port>
    no shutdown
```
<br>

## DCHP Configuration

DHCP (Dynamic Host Configuration Procol) is the network porotocol that automaticallt assigns IP configurations to devices on a network.

This simplifies and allows to skip over a direct IP address configuration.

Any IP address that forms part of a network with DHCP and wants to be reserved or is in use must be exlcuded from it, including *always the default gateway*.

The excluded addresses can be either a range or a singular address.

```
ip dhcp excluded-address <first_ip | Start of an IP range> <End of an IP range>

ip dhcp pool <pool_name>
  network <IP_network> <MSK>
  default-router <IP_default_gateway_subinterface>
  dns-server  <IP_dns_server>
```
<br>

## Connection to ISP

This configuration allows to redistirbute all network traffic going to the exterior passing it to more than one IPS.

In case there is only one service providor:

```
ip route 0.0.0.0  0.0.0.0  <physical interface>
```

In case there are more than one service provider:

Here the number is a priority, the lower the number, the higher the priority. When no number is given, it is declared as the highest 1.

```
ip route 0.0.0.0  0.0.0.0  <physical interface> <number>
ip route 0.0.0.0  0.0.0.0  <physical interface> <number>
```

<br>
<br>
<br>

# Switch Configuration

Similarly to the router configuration, the switch configuration changes depending on the characteristics of the network. 

## VLAN Configuration

Generally the VLAN configuration in one switch is divided in the following steps:
1. Creation of the VLAN database.
2. Assign ports to the VLANs used in the given switch.
3. Assign ports in TRUNK mode.
4. Assign the Native VLAN and respective switch gateway.

However, not always all these steps are used in all switches as some shurtcuts might be used as the central switch can collect the declaration of the VLANs and pass it to others through the VTP protocol. For this purpose, three configurations are shown, each for a different type of switch.

<br>

### Only Switch (All Steps)

1. Creation of the VLAN database.

```
vlan <VID>
    name <name>
    exit
```

Repeat the previous process for as many VLANs as used in the Switch. 

Once all the used VLANs are declared, then the second step is good to go.

2. Assign ports to the VLANs used in the given switch.

Per each range of interfaces or individual interface in case they are not consecutive declare their switchport mode as <code>access</code> and assign it to a specific VLAN in order to work.

```
interface range <start_port>/<start>-<end>
    description <description>
    switchport mode access
    switchport access vlan <VID>
    
```

3. Assign ports in TRUNK mode.

```
interface range <start_port>/<start>-<end>
    description <description>
    switchport mode trunk
    no shutdown
```

After declaring all ports in both modes (access and trunk) it is recommended to shutdown all remaining ports so they are not used or exploided without the administrator concent.

```
interface range <start_port>/<start>-<end>
    shutdown
```
4. Assign the Native VLAN and respective switch gateway.

Generally the VLAN of the switch is declared as the Native or Administrative VLAN, which has a concrete IP address to be assigned. The default gateway is the IP Address assigned to the subinterface of the same VLAN it was assigned to.

```
int vlan <native_VID>
    description <description>
    ip address <IP> <MSK>
    no shutdown

ip default-gateway <IP_gateway_subinterface>

```

<br>

### Central Switch | Core Switch

Generally a central or core switch does not have any terminal devices connected to it. Instead, all its ports are TRUNK as it is the responsible for redistributing the network traffic from all the available switches connected to it to the router, which regulates and segments traffic between VLANs and to the exterior through the Internet Service Providers.

As the main characteristics of this equipment:
- All its ports are trunk
- Can contain all the VLANs database and transmit it to all the switches it is connected to through the VTP protocol which allows to only write the VLANs once and this will be passed to others as long as the VTP protocol is configured between both.

0. Configure VTP protocol as server

For this section you will have to define a domain and a password that will be repeated when declaring the access to such protocol in the other devices.

```
vtp domain <domain>
vtp mode server 
vtp password <password>
```

1. Creation of the VLAN database.

```
vlan <VID>
    name <name>
    exit
```

Repeat the previous process for all the VLANs of the network. 

As all ports of the switch are TRUNK we will skip over step 2.

3. Assign (all) ports in TRUNK mode.

```
interface range <start_port>/<start>-<end>
    description <description>
    switchport mode trunk
    no shutdown
```

4. Assign the Native VLAN and respective switch gateway.

Generally the VLAN of the switch is declared as the Native or Administrative VLAN, which has a concrete IP address to be assigned. The default gateway is the IP Address assigned to the subinterface of the same VLAN it was assigned to.

```
int vlan <native_VID>
    description <description>
    ip address <IP> <MSK>
    no shutdown

ip default-gateway <IP_gateway_subinterface>

```



<br>

### Secondary Switch 

Finally, a secondary switch is that which is connected to terminal equipments and distributes its VLANs through different ports series. As the VTP domain will be activated, there is no need for VLAN declaration here.

0. Configure VTP protocol as client

```
vtp domain <domain>
vtp mode client 
vtp password <password>
```

It is very important the same domain and password are being used. If there are other TRUNK ports assigned to this device, it will transmit the VTP protocol if more switches are connected to it.

2. Assign ports to the VLANs used in the given switch.

Per each range of interfaces or individual interface in case they are not consecutive declare their switchport mode as <code>access</code> and assign it to a specific VLAN in order to work.

```
interface range <start_port>/<start>-<end>
    description <description>
    switchport mode access
    switchport access vlan <VID>
    
```

3. Assign ports in TRUNK mode.

```
interface range <start_port>/<start>-<end>
    description <description>
    switchport mode trunk
    no shutdown
```

After declaring all ports in both modes (access and trunk) it is recommended to shutdown all remaining ports so they are not used or exploided without the administrator concent.

```
interface range <start_port>/<start>-<end>
    shutdown
```
4. Assign the Native VLAN and respective switch gateway.

Generally the VLAN of the switch is declared as the Native or Administrative VLAN, which has a concrete IP address to be assigned. The default gateway is the IP Address assigned to the subinterface of the same VLAN it was assigned to.

```
int vlan <native_VID>
    description <description>
    ip address <IP> <MSK>
    no shutdown

ip default-gateway <IP_gateway_subinterface>

```

<br>

This marks the end of the Switch Configuration section.

For the examples of switches with their global configuration, check the within the folder <code>switches</code>.

<br>
<br>
<br>

# Troubleshooting and Monitoring Commands

1. Deploy and show the active configuration

```
do show running-config | do sh run
```

2. Show the interfaces ip address and their state (UP | DOWN)

```
do show ip interface brief | do sh ip int brief
```

3. Show active vlans and ports in the switch

```
do show vlan brief
```

<br>
<br>
<br>

# Additional Commands

1. Copy the configuration in execution (RAM) to static storage (NVRAM).

```
copy running-config startup-config
```

2. Delete VLANs data from a switch in order to reload it.

```
delete vlan.dat
```

Repeat the same command twice until it gives an error that is not sure which is that file.

3. Reload a switch

```
reload
```

Confirm in both bases and do not give or save any configuration.