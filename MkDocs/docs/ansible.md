# Ansible
## Basics
Linux distributions set up the framework for an Ansible preconfiguration in `/etc/ansible`, such as `/etc/ansible/ansible.cfg`.

Für SSH sollte man einen Login mit SSH-Key-Authentication ohne Passwortabfrage für einen unpriviligierten Benutzer auf den Ziel-Host einrichten, den nur Ansible benutzt. In `ansible.cfg` kann man im Abschnitt `[default]` den Namen und privaten Schlüssel eintragen.

For SSH, an unprivileged user with SSH key authentication (without password prompt), should be used to access the target host. The user should explicitly used for Ansible. In `ansible.cfg` you can enter the name and private key in the `[default]` section.

## Inventory / Hosts
-   default path: `/etc/ansible/hosts`
-   group name:

```yaml
test.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

-   connection variables

```yaml
[group1]
other1.example.com ansible_connection=ssh ansible_user=my_user
other2.example.com ansible_connection=winrm ansible_user=other_user
```

-   Alias

```yaml
[group1]
host ansible_host=192.0.2.50 ansible_port=5555
```

-   run code on localhost only

```yaml
localhost ansible_connection=local
```

## Testen / Query
-   test connection

```bash
ansible test01 -m ping -i hosts.ini
```

-   query all "facts"

```bash
ansible test01 -m setup -i hosts.ini | less
```

-   filter output 

```bash
ansible test01 -m setup -i hosts.ini -a 'filter=ansible_*_mb'
ansible all -m setup -i hosts.ini -a 'filter=ansible_processor*'
```
## Playbooks
Exact, structured instructions, written in YAML.

```yaml
---
- name: Install Apache Webserver 
	hosts: web 
	become: yes 

tasks:
- name: Install Latest Version 
	yum:	
			name: httpd 
			state: latest
```

The three --- indicate the beginning of a YAML file. After that begins what in Ansible slang is a "play": assigning tasks to a selection of hosts.`hosts:` selects the hosts from the inventory and with `become:` instructs Ansible to execute the following tasks with root privileges. This is followed by a list of one or more tasks. A task always consists of a module call in the appropriate parameters. Tasks are processed sequentially.

Execution of playbooks:

```yaml
ansible-playbook -i inventory install_apache.yaml
```
## Variablen
One creates on the same level with the inventory file the directories `host_vars/` and `group_vars/`. Variables that should be valid for all hosts are stored in the file `groups_var/all`. Based on the above shown inventory you could build the following structure:

```bash
inventory
group_var/all
        /web
        /dbserver
```