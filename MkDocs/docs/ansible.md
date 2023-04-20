# Ansible
## Grundlagen
Linux-Distributionen richten das Gerüst für eine Ansible-Vorkonfiguration in `/etc/ansible` ein, etwa `/etc/ansible/ansible.cfg`.

Für SSH sollte man einen Login mit SSH-Key-Authentication ohne Passwortabfrage für einen unpriviligierten Benutzer auf den Ziel-Host einrichten, den nur Ansible benutzt. In `ansible.cfg` kann man im Abschnitt `[default]` den Namen und privaten Schlüssel eintragen.

## Inventory / Hosts
-   Standard Pfad: `/etc/ansible/hosts`
-   Gruppennamen:

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

-   Verbindungsvariablen

```yaml
[group1]
other1.example.com ansible_connection=ssh ansible_user=mein_user
other2.example.com ansible_connection=winrm ansible_user=mein_anderer
```

-   Alias

```yaml
[group1]
host ansible_host=192.0.2.50 ansible_port=5555
```

-   Code nur auf localhost ausführen

```yaml
localhost ansible_connection=local
```

## Testen / Abfragen
-   Verbindung testen

```bash
ansible test01 -m ping -i hosts.ini
```

-   Alle “Fakten” auslesen

```bash
ansible test01 -m setup -i hosts.ini | less
```

-   Ausgabe der Fakten nach bestimmten Parametern filtern

```bash
ansible test01 -m setup -i hosts.ini -a 'filter=ansible_*_mb'
ansible all -m setup -i hosts.ini -a 'filter=ansible_processor*'
```
## Playbooks
In YAML geschriebene, klar strukturierte und verständliche Tätigkeitsanweisungen.

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

Die drei --- kennzeichnen den Beginn eine YAML-Datei. Danach beginnt, was im Ansible-Slang ein "Play" ist: das Zuweisen von Tasks zu einer Auswahl von Hosts.`hosts:` wählt die Hosts aus dem Inventory aus und weist Ansible mit `become:` an, die folgenden Tasks mit Root-Rechten auszuführen.Danach folgt eine Liste mit einem oder mehreren Tasks. Ein Task besteht immer aus einem Modulaufruf in den entsprechenden Parametern, Tasks werden sequenziell abgearbeitet.

Ausführen von Playbooks:

```yaml
ansible-playbook -i inventory install_apache.yaml
```
## Variablen
Man legt auf der gleichen Ebene mit dem Inventory-File die Verzeichnisse `host_vars/` und `group_vars/` an. Variablen, die für alle Hosts gelten sollen, nimmt die Datei `groups_var/all` auf.Ausgehend von den oben gezeigten Inventory könnte man folgende Struktur bauen:

```bash
inventory
group_var/all
        /web
        /dbserver
```