## ansible-openvpn-server
#### Configure a CentOS/RHEL system as an OpenVPN server
##### Uses EPEL!

This Ansible content will take a bare system such as a simple VPS and configure it as an OpenVPN Server.

You can configure things such as:

- Cockpit WebUI
- Fail2Ban
- OpenSSH Server Security
- Locking down the system with FirewallD
- Nginx server for serving CA, CRL, and OpenVPN Client configuration

Users are configured via PAM as native Linux users via username/password auth - you will need to create your own users on the system.

## Required Ansible Collections:

```
ansible-galaxy collection install community.general
ansible-galaxy collection install ansible.posix
```

## Before Running

Ensure that the user you are connecting to on your remote system has an `~/.ssh/authorized_keys` created (mode 644) and that your public key is listed in this file.  Otherwise by default, the Ansible Task that configures SSHd will remove Password-based authentication and thus lock you out of the system.

Copy the `example_inventory` to `inventory` and replace the information within to match your target system.

Also, make sure you have two FQDNs set up, `vpn.example.com` and on with a `-ca` suffix on the hostname, in this case that would be `vpn-ca.example.com`.

## Playbook Variables

There are a few basic variables that you need to set in order for the playbook to operate properly:

```yaml
system_hostname: vpn
system_domain: example.com

update_system_packages: true
reboot_after_kernel_update: true

setup_cockpit: false
setup_fail2ban: true
configure_sshd: true
configure_automatic_updates: true

selinux_state: enforcing

pki_certificate_authority_root_ca_organization_name: Example Labs
pki_certificate_authority_root_ca_common_name: Example Labs Root Certificate Authority

openvpn_root_ca_organization_name: Example Labs
openvpn_root_ca_common_name: Example Labs OpenVPN Intermediate Certificate Authority

deploy_ca_http_server: true
deploy_ca_http_server_secure: true
deploy_ca_https_server_letsencrypt: true
deploy_ca_https_server_letsencrypt_email: someoneyouusedtoknow@gmail.com
```

You can place your variable overrides in `extra_vars.yaml` for easy use - this file is listed in the .gitignore

## Running the Playbook

As a checklist for execution:

1. Add your Public SSH key to the remote server - add any additional users `adduser luser` and `passwd luser`
2. Run the `ansible-galaxy collection install` commands
3. Copy over `example_inventory` to `inventory` and modify
4. Create `extra_vars.yaml` and add in overrides
5. Create the 2 needed FQDNs in your DNS provider to point to your OpenVPN server

Then run:

`ansible-playbook -i inventory --extra-vars "@extra_vars.yaml" configure.yaml`