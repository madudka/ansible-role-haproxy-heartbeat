HAProxy + Heartbeat
=========

HAProxy and Heartbeat Debian installer for load balancing.

Tested on OS Debian GNU/Linux 12 (bookworm) with kernel Linux 6.1.0-23-amd64.

Install
-------
```
ansible-galaxy install madudka.haproxy_heartbeat
```

Requirements
------------
Min ansible version: 2.16.8

The Ansible role should include the `become: true` parameter to ensure that tasks are executed with elevated privileges.
You can use any method to pass the `ansible_become_user` and `ansible_become_password` parameters.
More: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_privilege_escalation.html

When using early versions of Ansible you need to install `community.general` collection:

```
ansible-galaxy collection install community.general
```


Role Variables
--------------
Default lower priority variables for this role `/defaults/main.yml`:

| Name                         | Description                         |  Value example                  |
|------------------------------|-------------------------------------|---------------------------------|
| `cfg_owner`                  | User owning the filesystem object.  | root                            |
| `cfg_group`                  | Group owning the filesystem object. | root                            |
| `sysctl_conf_path`           | Path to the sysctl.conf file.       | /etc/sysctl.conf                |
| `haproxy_cfg_path`           | Path to the haproxy.cfg file.       | /etc/haproxy/haproxy.cfg        |
| `haproxy_cfg_backup_path`    | Path to the backup of haproxy.cfg.  | /etc/haproxy/haproxy.cfg.backup |
| `haproxy_user`               | HAProxy process user.               | haproxy                         |
| `haproxy_group`              | HAProxy process group.              | haproxy                         |
| `balance_type`               | Load balancing algorithm.           | roundrobin                      |
| `heartbeat_authkeys_path`    | Path to the authkeys file.          | /etc/ha.d/authkeys              |
| `heartbeat_cf_path`          | Path to the ha.cf file.             | /etc/ha.d/ha.cf                 |
| `heartbeat_haresources_path` | Path to the haresources file.       | /etc/ha.d/haresources           |


Variables associated with this role `/vars/main.yml`:

| Name                        | Description                                | Value example                                              |
|-----------------------------|--------------------------------------------|------------------------------------------------------------|
| `haproxy_virtual_ip`        | Virtual IP address managed by HA software. | 192.168.0.99                                               |
| `haproxy_binding_port`      | Port for HAProxy binding.                  | 6443                                                       |
| `haproxy_network_interface` | Network interface for the VIP.             | enp0s3                                                     |
| `haproxy_subnet_mask`       | Subnet mask in CIDR notation.              | 24                                                         |
| `frontend_name`             | Name of the frontend.                      | kubernetes                                                 |
| `backend_name`              | Name of the backend.                       | kubernetes-master-nodes                                    |
| `server_list`               | List of servers for proxy connections.     | - { name: vm-master-1, ip: "192.168.0.101", port: "6443" } |
| `heartbeat_authkey`         | Authkey for securing node communication.   | 7f639b2a29c325d4318843b2ea58081a                           |
| `loadbalancer_master`       | Master load balancer server.               | "{{ loadbalancer_list[0].name }}"                          |
| `loadbalancer_list`         | List of load balancer servers.             | - { name: vm-lb-1 }                                        |

Dependencies
------------

No dependencies.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - { role: madudka.haproxy_heartbeat }
      become: true

License
-------

GPL-3.0-only

Author Information
------------------

https://madudka.github.io/