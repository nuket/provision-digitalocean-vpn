Introduction
============

Here's an Ansible playbook to provision a private VPN server on a DigitalOcean VPS.

Based on the work done here: 
https://raymii.org/s/tutorials/IPSEC_L2TP_vpn_with_Ubuntu_14.04.html

Prerequisites on a clean Ubuntu 14.04 Image
-------------------------------------------

```
# apt-get update
# apt-get upgrade
# apt-get install ansible
```

Customize ipsec.secrets with a new secret key.

```
# openssl rand -hex 32 > ipsec.psk
```

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
