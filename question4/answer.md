Overall explanation

Solution for Task 4: Ad-Hoc Commands

Task 4 focuses on using Ansible ad-hoc commands to perform basic administrative tasks such as creating a user, setting up passwordless SSH access, and checking system resources. This task introduces learners to the power of ad-hoc commands for quick automation without writing playbooks.

Objective

    Create a user ansibleuser on all managed nodes.

    Set up passwordless SSH access for ansibleuser.

    Check disk space and memory usage on all managed nodes.

1. Create a User ansibleuser on All Managed Nodes

Ad-hoc commands can directly execute modules on managed nodes.

    Command to create the user:

        ansible all -m user -a "name=ansibleuser state=present"

        all: Targets all hosts in the inventory.

        -m user: Uses the user module to manage user accounts.

        -a "name=ansibleuser state=present":

            name=ansibleuser: Specifies the username.

            state=present: Ensures the user exists.

    Explanation:

        This command creates the ansibleuser account on all managed nodes. If the user already exists, the command ensures no changes are made.

    Verify the user:

        ansible all -m command -a "id ansibleuser"

        The command module runs the id command on all nodes to confirm the user’s existence and display its UID, GID, and groups.

2. Set Up Passwordless SSH Access for ansibleuser

Passwordless SSH allows Ansible to connect to managed nodes securely without requiring a password for each command.

    Generate an SSH key pair on the control node:

        ssh-keygen -t rsa -b 2048 -f /home/ansibleuser/.ssh/id_rsa -N ""

        -t rsa: Specifies the RSA algorithm.

        -b 2048: Sets the key length to 2048 bits.

        -f: Specifies the file path for the key pair.

        -N "": Leaves the passphrase empty.

    Copy the public key to all managed nodes:

        ansible all -m authorized_key -a "user=ansibleuser key={{ lookup('file', '/home/ansibleuser/.ssh/id_rsa.pub') }} state=present"

        -m authorized_key: Uses the authorized_key module to manage SSH keys.

        user=ansibleuser: Specifies the user for which the key is added.

        key={{ lookup('file', '/home/ansibleuser/.ssh/id_rsa.pub') }}:

            Adds the content of the public key file to the authorized_keys file for ansibleuser.

        state=present: Ensures the key is added.

    Test SSH connectivity:

        ssh ansibleuser@<managed-node>

        Replace <managed-node> with the hostname or IP of a managed node.

3. Check Disk Space and Memory Usage

Use ad-hoc commands to retrieve system resource information.

    Check disk space:

        ansible all -m command -a "df -h"

        df -h: Displays disk usage in a human-readable format.

        Example output:

            node1.example.com | SUCCESS | rc=0 >>
            Filesystem      Size  Used Avail Use% Mounted on
            /dev/sda1        20G  8G   12G   40% /

    Check memory usage:

        ansible all -m command -a "free -h"

        free -h: Displays memory usage in a human-readable format.

        Example output:

            node1.example.com | SUCCESS | rc=0 >>
                         total        used        free      shared  buff/cache   available
            Mem:         2.0G        1.0G        500M        100M       500M        1.5G

Key Concepts for Novice Learners

    Ad-Hoc Commands:

        Quick one-line commands for performing tasks on managed nodes without creating a playbook.

        Ideal for simple, repetitive tasks or testing configurations.

    Modules Used:

        user: Manages user accounts.

        authorized_key: Manages SSH keys for users.

        command: Executes shell commands on managed nodes.

    SSH and Passwordless Access:

        Critical for Ansible to automate tasks without manual intervention.

        Adds security and efficiency by eliminating password prompts.

    System Resource Commands:

        df -h: Displays filesystem disk usage.

        free -h: Shows memory usage.

Troubleshooting Tips

    User Creation Fails:

        Ensure you have proper permissions to create users on managed nodes.

    SSH Key Not Added:

        Check if the ~/.ssh/authorized_keys file exists on the managed nodes for ansibleuser.

        Ensure the public key content is correct.

    Commands Not Executed:

        Verify that the managed nodes are reachable.

        Use:

            ansible all -m ping

Summary

By completing this task, you’ve learned:

    How to use ad-hoc commands for quick automation.

    The importance of passwordless SSH access for efficient Ansible operations.

    Basic system resource monitoring using df and free.

This task demonstrates the flexibility and power of Ansible for rapid configuration and management of systems, even without writing playbooks.
