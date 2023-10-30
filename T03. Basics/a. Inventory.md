# Inventory
In Ansible, the inventory file is a critical component for organizing and managing the hosts you want to control. An inventory file specifies the hosts and groups of hosts that Ansible can work with. It can be used to filter and group hosts, manage variables, and more. Here are the basics of working with Ansible inventory:

### Inventory File Basics:

An Ansible inventory file can be written in INI or YAML format. INI format is simpler, while YAML format offers more flexibility. You can use either format based on your preference.

**INI Format Example:**
```ini
[web_servers]
web1 ansible_host=192.168.1.101 ansible_user=admin
web2 ansible_host=192.168.1.102 ansible_user=admin

[db_servers]
db1 ansible_host=192.168.1.201 ansible_user=dbadmin
```

**YAML Format Example:**
```yaml
all:
  hosts:
    web1:
      ansible_host: 192.168.1.101
      ansible_user: admin
    web2:
      ansible_host: 192.168.1.102
      ansible_user: admin
  children:
    db_servers:
      hosts:
        db1:
          ansible_host: 192.168.1.201
          ansible_user: dbadmin
```

### Running Ansible Commands with Inventory Filters:

You can use the `--limit` option to run Ansible commands against specific hosts or groups from your inventory file. Here are some examples:

1. **Run against a single host:**

   ```bash
   ansible-playbook -i inventory.ini playbook.yml --limit web1
   ```

2. **Run against a group of hosts:**

   ```bash
   ansible-playbook -i inventory.ini playbook.yml --limit web_servers
   ```

### Multiple Inventory Files:

You can use multiple inventory files for different environments or purposes. To specify multiple inventory files, you can use the `-i` option multiple times:

```bash
ansible-playbook -i inventory1.ini -i inventory2.ini playbook.yml
```

### Managing Variables with Inventory:

You can define variables in your inventory file to apply host-specific or group-specific settings. Here's an example in INI format:

```ini
[web_servers]
web1 ansible_host=192.168.1.101 ansible_user=admin

[db_servers]
db1 ansible_host=192.168.1.201 ansible_user=dbadmin

[web_servers:vars]
http_port=80
db_server=db1
```

You can access these variables in your playbooks using the `hostvars` dictionary. For example, to use the `http_port` variable:

```yaml
---
- name: Example Playbook
  hosts: web_servers
  tasks:
    - name: Print HTTP Port
      debug:
        msg: "HTTP Port is {{ hostvars[inventory_hostname]['http_port'] }}"
```

### Using the `ansible-inventory` Command:

You can use the `ansible-inventory` command to get information about your inventory, including host and group details, variables, and more. To list all hosts in your inventory, you can use:

```bash
ansible-inventory -i inventory.ini --list
```

To get details about a specific host or group, use:

```bash
ansible-inventory -i inventory.ini --host <hostname>
ansible-inventory -i inventory.ini --group <groupname>
```

This command can be useful for debugging and inspecting your inventory setup.

Ansible's inventory system is versatile and allows you to organize and manage your infrastructure in various ways, making it easier to work with different groups of hosts, apply specific configurations, and handle variable assignments based on your environment or needs.

### Inventory variables
You need an Inventory file (for ex.: development.ini) where you determine the host what you want
to use:
>[MACHINE_NAME]
MACHINE_NAME hostname=MACHINE_NAME ansible_ssh_host=IP_ADDRESS ansible_port=SSH_PORT
ansible_connection=ssh ansible_user=USER ansible_ssh_extra_args="-o StrictHostKeyChecking=no -
o UserKnownHostsFile=/dev/null"

* **hostname** - the hostname of the remote machine
* **ansible_ssh_host** - the ip or domain of the remote host
* **ansible_port** - the port of the remote host which is usually 22
* **ansible_connection** - the connection where we set, we want to connect with ssh
* **ansible_user** - the ssh user
* **ansible_ssh_extra_args** - extra argumentums what you want to specify for the ssh
connection

Required extra args for ssh:

* **StrictHostKeyChecking** - It can ask a key checking what waiting for a yes or no. The Ansible can't answer this question then throw an error, the host not available.
* **UserKnownHostsFile** - Needed for StrictHostKeyChecking option.