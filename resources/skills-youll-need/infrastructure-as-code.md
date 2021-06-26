# Infrastructure as code

## **Ansible**

Manages different systems that define a infrastructure. These systems can be a jenkins, git server, a container, a webserver. Ansible uses ssh to communicate with these systems.

For security testing ansible can help hardening a platform, test security patches, patch systems without downtime.

#### System Hardening

System Hardening is the process of securing a system’s configuration and settings to reduce IT vulnerability and the possibility of being compromised. This can be done by reducing the attack surface and attack vectors which attackers continuously try to exploit for purpose of malicious activity.

Hardening and Securing servers is often time-consuming, error-prone, non-portable and not scalable.

#### Modules

Allow to control system components. Modules are executable libraries \(or scripts\)

#### Tasks 

Smallest entity in ansible. Install a package could be a task. 

#### Roles

A role is a set of tasks to configure a host to serve a certain purpose like configuring a service. A ****role is an independent component which allows reuse of common configuration steps. It ****has to be used within a playbook.

Roles are stored by an active community in ansible galaxy. Like docker hub where you can find community developed containers, you'll find preconfigured roles in galaxy.

#### Playbook

Contains instructions to configure a system/a node

#### Inventory

All systems that are managed by ansible are regrouped into the inventory. 

### Ansible challenges

ssh  key management, monitoring

### More 

[https://serversforhackers.com/s/ansible](https://serversforhackers.com/s/ansible)

[https://www.youtube.com/watch?v=w8fOEEMqpOw](https://www.youtube.com/watch?v=w8fOEEMqpOw)

[https://docs.ansible.com/ansible/latest/user\_guide/intro\_getting\_started.html](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html)

[https://www.tutorialspoint.com/ansible/index.htm](https://www.tutorialspoint.com/ansible/index.htm)

