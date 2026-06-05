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

## 12. Console Access Control

  12.1 Access to the line console configuration.

  The number 0 refers to the port of the network device responsible for the console management. Generally there are two ports 0 and 1 although only one of them can be configured and active at a time.

  ```
  line console 0
  ```

  12.2 Establish an execution time after which the console will logout.

  ```
  exec-timeout <number of minutes>
  ```

  12.3 Configuring access to the line console.

  There are two alternatives:
  - Configure local access based on the users database.
  - Configure an exclusive password for the line console. Available if skipped over step 8. The password might be encrypted or not.

  | Access with Local Users | Access with Password Not Encrypted | Access with Password Encrypted |
  |-------|-------|-------|
  | <pre><code> login local </code></pre> | <pre><code> password &lt;password&gt; <br> login </code></pre> | <pre><code> secret &lt;password&gt; <br> login </code></pre> |


  12.4 Login into the system.

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

## 13. Virtual Console Configuration for Remote Access

  13.1 Access the remote console configuration with number of available connections. Numbers are quantity between two given numbers starting in 0.

  E. 0 5 = 6 connections, 0 1 = 2 connections.

  Following *Cisco's Standard* for Routers a total of recommended connections is 5 (0 4) while for switches is 16 (0 15); however, the recommendation based on security is keep it as low as possible for the only existent users declared in the users database.

  ```
  line vty 0 2
  ```

  13.2 Establish an execution time after which the console will logout.

  ```
  exec-timeout <number of minutes>
  ```

  13.3 Configuring transport method for remote access.

  The default transport method is <code>telnet</code>; however, this protocol is very insecure as it ahs no inscription and is easily detectable. Instead it is passed to the <code>ssh</code> (Secure SHell) which requires the encryption previously declared with crypto key.

  ```
  transport input ssh
  ```

  13.4 Configuring access to the line console.

  There are two alternatives:
  - Configure local access based on the users database.
  - Configure an exclusive password for the line console. Available if skipped over step 8. The password might be encrypted or not.

  | Access with Local Users | Access with Password Not Encrypted | Access with Password Encrypted |
  |-------|-------|-------|
  | <pre><code> login local </code></pre> | <pre><code> password &lt;password&gt; <br> login </code></pre> | <pre><code> secret &lt;password&gt; <br> login </code></pre> |


  13.5 Login into the system.

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

14. Activate password encryption in the device

```
service password-encryption 
```

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

### Serial ports  - Device Communication Equipment
This specific configuration which only adds the command <code>clock rate</code> is only established in the DCE sides of a serial connection which is the speed of communication through the DCE and the DCT (Device Communication Terminal).

```
interface <serial_port_name as S0/0/0>
  description <description>
  clock rate <64000 | 128000 | 256000 | 512000 | 2000000>
  ip address <IP> <MSK>
  no shutdown

```

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

# Switch Configuration



# Troubleshooting and Monitoring Commands

1. Deploy and show the active configuration

```
do show running-config | do sh run
```




# Additional Commands

1. Copy the configuration in execution (RAM) to static storage (NVRAM).

```
copy running-config startup-config
```