Introduction
============

Here's an Ansible playbook to provision a private VPN server on a DigitalOcean VPS.

Based on the work done here: 
https://raymii.org/s/tutorials/IPSEC_L2TP_vpn_with_Ubuntu_14.04.html

Prerequisites on a clean Ubuntu 14.04 Image
-------------------------------------------

# apt-get update
# apt-get upgrade
# apt-get install ansible

Customize ipsec.secrets with a new secret key.

# openssl rand -hex 32 > ipsec.psk

Run the Ansible playbook.

# ansible-playbook -v -i inventory --connection=local setup-vpn.yml
