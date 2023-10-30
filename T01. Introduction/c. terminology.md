# Ansible terminology

Ansible has several key terms and concepts that are important to understand when working with the tool. Here are some fundamental Ansible terminology:

1. **Control Machine:** The system where Ansible is installed and from which you manage your infrastructure. This is where you write playbooks and execute Ansible commands.

2. **Managed Nodes:** The target machines where Ansible connects and performs actions. These machines can be servers, network devices, or any system that Ansible manages.

3. **Inventory:** A list of managed nodes that Ansible can connect to and manage. It defines the hosts and groups of hosts in your infrastructure. Inventory files can be in INI or YAML format.

4. **Playbook:** A YAML file that describes a series of tasks to be executed on remote hosts. Playbooks are used to define automation workflows and configurations.

5. **Task:** An individual action that Ansible performs on a remote host, such as installing packages, copying files, or configuring services. Playbooks consist of one or more tasks.

6. **Module:** Self-contained units of code that Ansible uses to perform tasks on remote hosts. Modules are responsible for tasks like managing users, copying files, and configuring services.

7. **Role:** A way to organize and package related playbooks, tasks, and variables. Roles promote code reusability and are a best practice for structuring Ansible automation code.

8. **Inventory Variables:** Variables defined in the inventory file that can be assigned to individual hosts or groups of hosts. These variables can be used in playbooks to make tasks more dynamic.

9. **Facts:** Information collected by Ansible from remote hosts. These facts include details about the system, such as the operating system, IP addresses, hardware, and more. Facts can be used in playbooks for conditional tasks.

10. **Handlers:** Special tasks defined in playbooks that are only triggered when a change in the state of a system is required. For example, restarting a service only if a configuration file has changed.

11. **Ad-Hoc Commands:** Single, one-off commands that are run on remote hosts using the `ansible` command-line tool. Ad-hoc commands are helpful for quick tasks and don't require writing full playbooks.

12. **Modules Library:** A collection of built-in Ansible modules that provide functionality for various tasks, such as managing files, user accounts, packages, and more.

13. **Vault:** A feature in Ansible for encrypting sensitive data, such as passwords and secret keys, to keep them secure within your playbooks and roles.

14. **Play:** A set of tasks that are executed on a specific group of hosts. A playbook can include one or more plays, allowing you to orchestrate different tasks on different groups of hosts.

15. **YAML:** The file format used for Ansible playbooks, roles, and inventory files. YAML is a human-readable data serialization language.

