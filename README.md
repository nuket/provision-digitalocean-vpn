Introduction
============

Here's an Ansible playbook to provision a private VPN server on a DigitalOcean VPS.

Based on the work done here: https://raymii.org/s/tutorials/IPSEC_L2TP_vpn_with_Ubuntu_14.04.html with one major fix to support Windows 7, 8, and 8.1 VPN client connections.

Prerequisites on a clean Ubuntu 14.04 Image
-------------------------------------------

```
# apt-get update
# apt-get upgrade
# apt-get install ansible git
```

Check out this repository:

```
# git clone https://github.com/nuket/provision-digitalocean-vpn.git
# cd provision-digitalocean-vpn
```

### L2TP Preshared Key

Create one of the secret values used to connect to the VPN server:

```
# openssl rand -hex 32 > ipsec.psk
```

### Username and Password

Customize `chap-secrets.j2` with VPN username and password information.

You can generate a random password using `openssl rand -hex 8`. 

The `chap-secrets.j2` entries will look something like (server is always `l2tpd`, IP addresses would probably always be `*`, unless you know what you're doing:

```
# Secrets for authentication using CHAP
# client       server  secret                  IP addresses
alice          l2tpd   0F92E5FC2414101EA       *
bob            l2tpd   DF98F09F74C06A2F        *
```

### Go!

Run the Ansible playbook.

```
# ansible-playbook -v -i inventory --connection=local setup-vpn.yml
```

Running `ipsec verify` should then return the all clear:

```
# ipsec verify
Checking your system to see if IPsec got installed and started correctly:
Version check and ipsec on-path                                 [OK]
Linux Openswan U2.6.38/K3.13.0-24-generic (netkey)
Checking for IPsec support in kernel                            [OK]
 SAref kernel support                                           [N/A]
 NETKEY:  Testing XFRM related proc values                      [OK]
        [OK]
        [OK]
Checking that pluto is running                                  [OK]
 Pluto listening for IKE on udp 500                             [OK]
 Pluto listening for NAT-T on udp 4500                          [OK]
Checking for 'ip' command                                       [OK]
Checking /bin/sh is not /bin/dash                               [WARNING]
Checking for 'iptables' command                                 [OK]
Opportunistic Encryption Support                                [DISABLED]
```

Windows VPN Setup
=================

![Create connection image](/images/windows/create-connection.png?raw=true)

The default PPP Settings can be kept:

![PPP Settings](/images/windows/ppp-settings.png?raw=true)

The Security settings should be upgraded to "Require encryption", and allow MS-CHAP v2:

![Security Tab](/images/windows/security-tab.png?raw=true)

Copy the L2TP Preshared Key, that was generated above, into this field:

![L2TP Preshared Key](/images/windows/l2tp-preshared-key.png?raw=true)

Disable `File and Printer Sharing for Microsoft Networks` and `Client for Microsoft Networks`, as they're chatty and probably nothing you need in this VPN.

![Networking Tab](/images/windows/networking-tab.png?raw=true)

`Disable NetBIOS over TCP/IP` in the Internet Protocol v4 settings:

![WINS Settings](/images/windows/wins-tab.png?raw=true)

Use the username and password you placed in the `chap-secrets.j2` file to connect to the VPN server:

![Connecting...](/images/windows/connecting-sidebar.png?raw=true)

All done!
