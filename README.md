# ansible-sshd-role
===================

A role for [Ansible](http://www.ansible.com) to configure ssh (Client and Server)

## Compatibility

> List of campatible OS's
>   - CentOS/Rhel 7

**WARNING**  Misconfiguration of this role can lock you out of your server! Please test your configuration and its interaction with your users configuration before using in production!

## Dependencies
Ansible: 1.9

## Usage 

The role includes a global variable file located in vars/main.yml wich defines the followin sets of variables

Global configuration set:
- Configuration of ssh user groups 
- Location of the configuration file
- Usage of hosts.allow restrictions
- Configuration of SSH banner

SSH-Server set : 
- SSHD listen variables
- A list of Default varaibles for KEYS, LOGGIN, AUTHENTICATION, Extended_options etc

SSH-Server Group Overide set:
- Overide parameters for SSH Group/User permissions or constraints

## Examples

Global Variable Example
```
#List all the available ssh access groups
sshd_group_list: ["sshadmins","sshusers","sshguests"]
sshd_group_deny: ["root","users"]
sshd_users_deny: ["root","nobody"]

#Location of config file
sshd_config_location: "/etc/ssh/sshd_config"

#SSH banner
sshd_banner: "yes" #sets weathere to use banner

#SSHd in hosts.allow
sshd_hosts_allow:
        preset: "yes"
        condition:
              - "ALL" #exemple : sshd: ALL | sshd: LOCAL
```

SSH-Server Listening parameters:
```
sshd:
  listen:
        ports: ["22","5022"]
        address:
                family: inet   # vars: any, inet, inet6
                ipv4: ['0.0.0.0']
                ipv6: ['::']
```
SSH-Server Group/User Overide parameters:
```
sshd_UG_overide:
        Group:
           1:
                name_data: ["sshadmins"]
                overides:
                        X11Forwarding: "yes"
                        AllowTcpForwarding: "yes"
           2:
                name_data: ["nobody"]
                overides:
                        X11Forwarding: "no"
                        AllowTcpForwarding: "no"
                        PermitTTY: "no"
                        ForceCommand: "cvs server"
        User:
           1:
                name_data: ["anoncvs"]
                overides:
                        X11Forwarding: "no"
                        AllowTcpForwarding: "no"
                        PermitTTY: "no"
                        ForceCommand: "cvs server"
```
Under the overides section just write don the parameters you require !
