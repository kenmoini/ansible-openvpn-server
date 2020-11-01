## ansible-openvpn-server
#### Configure a CentOS/RHEL system as an OpenVPN server
##### Uses EPEL!

This Ansible content will take a bare system such as a simple VPS and configure it as an OpenVPN Server.

You can configure things such as:

- Cockpit WebUI
- Fail2Ban
- OpenSSH Server Security
- Locking down the system with FirewallD
- OpenVPN Users (configuration objects kept on the remote server, or downloaded locally in not running in Tower)

## Before Running

Ensure that the user you are connecting to on your remote system has an `~/.ssh/authorized_keys` created (mode 644) and that your public key is listed in this file.  Otherwise by default, the Ansible Task that configures SSHd will remove Password-based authentication and thus lock you out of the system.