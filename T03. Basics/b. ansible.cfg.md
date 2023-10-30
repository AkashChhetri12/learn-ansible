# Ansible configuration file [ansible.cfg]

The `ansible.cfg` configuration file is used to customize Ansible's behavior and settings. This file can be placed in several locations, and Ansible will search for it in the following order:

1. In the current working directory where you run Ansible.
2. In your home directory as `~/.ansible.cfg`.
3. In `/etc/ansible/ansible.cfg`.

You can use the `ansible.cfg` file to set a wide range of configuration options, including control the default behavior of Ansible, specify remote user and SSH key settings, define custom module paths, and more.

Here is an example of what the contents of an `ansible.cfg` file might look like:

```ini
[defaults]
# Remote user to use for connections.
remote_user = ansible

# SSH private key file.
private_key_file = ~/.ssh/id_rsa

# Default inventory file.
inventory = inventory.ini

# Module path for custom modules.
module_utils = /usr/local/lib/my_ansible_modules

# Set the number of parallel processes to use.
forks = 5

[ssh_connection]
# SSH persistent control master settings.
control_path_dir = ~/.ansible/cp
```

The `[defaults]` section contains settings that affect the default behavior of Ansible, such as the remote user, SSH key file, inventory file, and more.

The `[ssh_connection]` section allows you to configure SSH-related options, including control master settings for SSH connection reuse.

In addition to the settings shown in the example, there are many other options available in the `ansible.cfg` file, and you can customize them to match your specific needs. Here are a few common settings you might find useful:

- `gathering = smart` or `gathering = explicit` controls how facts are gathered.
- `transport = ssh` or `transport = paramiko` specifies the SSH library to use for connections.
- `pipelining = True` enables or disables SSH pipelining.
- `log_path = /var/log/ansible.log` sets the log file location.

You can also use variables in your `ansible.cfg` file by referencing them like this:

```ini
my_custom_variable = some_value
```

Then, you can access these variables within your playbooks or roles as needed.

It's important to note that some settings in `ansible.cfg` can also be overridden in your inventory files, playbooks, or command-line options. When there's a conflict, Ansible follows a specific order of precedence for configuration settings, with command-line options taking the highest precedence, followed by inventory, playbook, and `ansible.cfg`. This means that you can set global defaults in your `ansible.cfg` and override them as needed in other configuration files or during command execution.

## For general use

The `/etc/ansible/ansible.cfg` is the default configuration file. However, it’s also
recommended that you don’t mess with `/etc/ansible/ansible.cfg` and just use it
a reference. You should create your own Ansible configuration file in your Ansible
project directory.
The `ansible --version` command will show you which configuration file you are
currently using:

```
[<user>@control plays]$ ansible --version
ansible 2.9.14
config file = /etc/ansible/ansible.cfg
configured module search path = ['/home/<user>/.ansible/plugins/modules',
'/usr/share/ansible/plugins/modules']
ansible python module location = /usr/lib/python3.6/site-packages/ansible
executable location = /usr/bin/ansible
python version = 3.6.8 (default, Dec 5 2019, 15:45:45)
```


The two most important sections that you need to define in your Ansible configuration file are:
1. [defaults]
2. [privilege_escalation]
In the [defaults] section, here are the most important settings you need to be aware
of:
- inventory → specifies the path of your inventory file.
- remote_user → specifies the user who will connect to the managed hosts and run the playbooks.
- forks → specifies the number of host that ansible can manage/process in
parallel; default is 5.
- host_key_checking → specifies whether you want to turn on/off SSH key
host checking; default is True.

In the [privilege_escalation] section, you can configure the following settings:
- become → specify where to allow/disallow privilege escalation; default is
False.
- become_method → specify the privilege escalation method; default is sudo.
- become_user → specify the user you become through privilege escalation;
default is root.
- become_ask_pass → specify whether to ask or not ask for privilege escalation password; default is False.
Keep in mind, you don’t need to commit any of these settings to memory. They
are all documented in /etc/ansible/ansible.cfg.

Now create your own ansible.cfg configuration file in your Ansible project directory
/home/<user>/<someplay> and set the following settings 

```
[defaults]
inventory = inventory.ini
remote_user = <user>
host_key_checking = false

[privilege_escalation]
become = true
become_method = sudo
become_user = root
become_ask_pass = false

```

Now run the `ansible --version` command one more time; you should see that your
new configuration file is now in effect:
```[<user>@control plays]$ ansible --version
ansible 2.9.14
config file = /home/<user>/plays/ansible.cfg
configured module search path = ['/home/<user>/.ansible/plugins/modules',
'/usr/share/ansible/plugins/modules']
ansible python module location = /usr/lib/python3.6/site-packages/ansible
executable location = /usr/bin/ansible
python version = 3.6.8 (default, Dec 5 2019, 15:45:45)```