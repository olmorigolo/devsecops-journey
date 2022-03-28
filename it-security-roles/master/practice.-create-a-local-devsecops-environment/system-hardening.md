---
description: Maturity Level 1-4
---

# System Hardening

**System Hardening** is the process of securing a **system's** configuration and settings to reduce IT vulnerability and the possibility of being compromised. This can be done by reducing the attack surface and attack vectors which attackers continuously try to exploit for purpose of malicious activity.

## ansible

Ansible is a tool for automation of configurations and deployments. Scripts are written in yaml files as playbooks. As a Security tool we can use ansible to apply OS updates, service packs, and patches automatically; remove unnecessary drivers, file sharing, libraries, software, services, and functionality. These tasks are part of system hardening.&#x20;

Installation&#x20;

`pip3 install ansible==2.10.4 ansible-lint==4.3.7`

create an inventory file

```
cat > inventory.ini <<EOL

# Our Company App Inventory
[staging]
staging-ourcompany-app

[prod]
prod-ourcompany-app

EOL
```

In order to run ansible on these machinesvia SSH, we need to put them in our know-hosts list

```
ssh-keyscan -t rsa  staging-ourcompany-app prod-ourcompany-app >> ~/.ssh/known_hosts
```

&#x20;Check the uptime of the production system with ansible

`ansible -i inventory.ini`` `**`prod`**` ``-m`` `**`shell`**` ``-a "uptime"`

Install a service called "ntp" with ansible on the production machine:

`ansible -i inventory.ini`` `**`prod`**` ``-m`` `**`apt`**` ``-a "name=ntp state=present"`

### Ad-hoc commands

Which version of bash is running on my machines?

`ansible -i inventory.ini`` `**`all`**` ``-m`` `**`command`**` ``-a "bash --version"`

Get all parameters and vars of your inventory

`ansible -i inventory.ini all -m setup`

copy a file from executing machine to remote hosts&#x20;

```
ansible -i inventory.ini machinename -m copy -a "src=notes.txt dest=notes.txt"
```

### Playbooks



#### Check if nginx is installed

If nginx is installed, print nginx version to the terminal by using the msg module

```
---
- name: Get the version of nginx
  hosts: all
  remote_user: root
  gather_facts: no

  tasks:
  - name: Check if nginx is installed
    command: which nginx
    register: nginx_version

  - name: Print nginx version
    command: nginx -v | echo
    when: nginx_version != -1

  - debug:
      msg: "Nginx is not installed"
      when: nginx_version == -1
```

#### Register an output

On the machines, using `cat /etc/os-release` outputs the following data:

```
PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
ID=kali
VERSION="2021.1"
VERSION_ID="2021.1"
VERSION_CODENAME="kali-rolling"
ID_LIKE=debian
ANSI_COLOR="1;31"
HOME_URL="https://www.kali.org/"
SUPPORT_URL="https://forums.kali.org/"
BUG_REPORT_URL="https://bugs.kali.org/"
```

We can use this information and store it into a variable with `register: os_release`

```
---
- name: Simple playbook
  hosts: all
  remote_user: root
  gather_facts: no     # what does it mean?

  tasks:
  - name: Show the content of /etc/os-release
    command: cat /etc/os-release
    register: os_release

  - debug:
      msg: "This system uses Ubuntu-based distro"
    when: os_release.stdout.find('Ubuntu') != -1
```

#### Conditionals

```
```
