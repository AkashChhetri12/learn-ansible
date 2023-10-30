
# Ansible Setup

To set up SSH connections between the Ansible controller (your control machine) and the managed nodes, you'll need to ensure that SSH is properly configured, keys are set up, and that the necessary software, such as Python, is available on both the controller and nodes. Here's a step-by-step guide to achieving this:

### On the Ansible Controller (Control Machine):

1. **Install Ansible:**
   If you haven't already, you need to install Ansible on the control machine. You can typically install Ansible using your system's package manager. For example, on a Debian-based system:

   ```bash
   sudo yum update
   sudo yum install ansible
   ```

2. **Generate SSH Key Pair:**
   If you haven't already, generate an SSH key pair on your control machine. This key pair will be used for passwordless authentication to the managed nodes.

   ```bash
   ssh-keygen
   ```

   Follow the prompts to create your SSH key pair. The public key (`~/.ssh/id_rsa.pub`) is what you'll copy to the managed nodes.

3. **Copy Public Key to Managed Nodes:**
   You need to copy your SSH public key to each managed node. You can use `ssh-copy-id` to do this:

   ```bash
   ssh-copy-id <user>@<managed_node_ip>
   ```

   Replace `<user>` with your username on the managed node, and `<managed_node_ip>` with the IP address or hostname of the managed node.

4. **Python Installation:**
   Ansible relies on Python to run its tasks on the managed nodes. Ensure that Python is installed on the managed nodes. Most Linux distributions come with Python pre-installed. You can check its presence by running:

   ```bash
   ssh <user>@<managed_node_ip> python --version
   ```

   If Python is not installed or if it's not in the system's PATH, you may need to install it.

### On the Managed Nodes:

1. **SSHD Configuration:**
   Verify that SSHD (SSH server) is installed and running on the managed nodes. Most Linux distributions come with an SSH server pre-installed, but if it's not, you can install it. Ensure that SSHD is configured to allow SSH key-based authentication.

2. **Python Installation:**
   As mentioned earlier, Ansible relies on Python. Ensure that Python is installed on the managed nodes. Most Linux distributions come with Python pre-installed, but if not, you'll need to install it.

   On Debian-based systems:

   ```bash
   sudo apt-get update
   sudo apt-get install python
   ```

   On Red Hat-based systems:

   ```bash
   sudo yum install python
   ```

   If you need to install Python on a large number of systems, consider using Ansible itself to install Python before running more complex playbooks.

3. **SSH Configuration:**
   Verify that the SSH server on the managed node is configured to allow SSH key-based authentication. Open the SSH configuration file on the managed node (usually `/etc/ssh/sshd_config`) and ensure the following settings are in place:

   ```bash
   PasswordAuthentication no
   PermitRootLogin no
   ```

   After making changes to the SSHD configuration, restart the SSH service to apply the changes:

   ```bash
   sudo systemctl restart sshd  # On systemd-based systems
   ```

