Overall explanation

# Solution for Task 1: Ansible Installation and Configuration

Task 1 focuses on setting up Ansible on the control node and preparing it for managing other systems. This step-by-step explanation is tailored for novice learners to grasp the concepts and procedures easily.

### 1. Install ansible-core on controller.example.com

Ansible is a configuration management tool that allows you to automate tasks on multiple nodes. The ansible-core package provides the minimal version of Ansible required for automation.

    Update the system’s package index:

        sudo dnf update -y

    Install ansible-core:

        sudo dnf install ansible-core -y

        The -y flag automatically confirms the installation prompt.

    Verify the installation:

        ansible --version

        This command outputs the installed Ansible version and confirms that Ansible is ready to use.

### 2. Create a User ansibleuser with Passwordless Sudo Access

This user will manage tasks on the control node.

    Create the user:

        sudo useradd ansibleuser

    Set a password for the user:

        sudo passwd ansibleuser

    Grant passwordless sudo access:

        Edit the sudoers file using visudo to ensure safe editing:

            sudo visudo

        Add the following line to allow passwordless sudo for ansibleuser:

            ansibleuser ALL=(ALL) NOPASSWD: ALL

    Test the sudo privileges:

        Switch to the ansibleuser account:

            su - ansibleuser

        Run a command with sudo to verify:

            sudo ls /root

        If the command executes without a password prompt, the setup is correct.

### 3. Configure /etc/ansible/hosts

The inventory file lists all the nodes managed by Ansible, grouped logically by roles.

    Open the inventory file for editing:

        sudo nano /etc/ansible/hosts

    Add the following groups and nodes:

        [webservers]
        node1.example.com
         
        [databases]
        node2.example.com
         
        [backup]
        node3.example.com

        Replace nodeX.example.com with actual hostnames or IPs of managed nodes.

    Save and exit the file.

### 4. Update /etc/ansible/ansible.cfg

This file customizes Ansible's behavior and paths.

    Open the configuration file for editing:

        sudo nano /etc/ansible/ansible.cfg

    Add or update the following configurations:

        [defaults]
        inventory = /etc/ansible/hosts   # Inventory file location
        log_path = /var/log/ansible.log  # Log file path
        host_key_checking = False        # Disable host key checking

    Save and exit the file.

### 5. Verify the Configuration

After setting up Ansible, it’s crucial to validate the configurations to ensure they are applied correctly.

    Dump the active configuration:

        ansible-config dump --only-changed

        This command lists only the parameters you have changed in ansible.cfg.

    Check connectivity to all nodes (if accessible):

        ansible all -m ping

        If the setup is correct, you should receive a “pong” response from each node.

Key Concepts for Novice Learners

    Ansible Control Node: The main system where Ansible is installed and from which tasks are executed.

    Managed Nodes: Systems managed by Ansible, listed in the inventory file.

    Inventory Groups: Logical grouping of nodes to simplify task targeting.

    Passwordless Sudo: Allows a user to execute administrative tasks without repeatedly entering a password.

    Configuration File (ansible.cfg): Centralized file to define Ansible’s behavior globally.

By following these steps, you have successfully completed Task 1 and prepared the control node for further automation tasks. This foundational setup ensures Ansible can communicate and manage the defined nodes efficiently.
