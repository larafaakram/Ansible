Overall explanation

Solution for Task 19: Debugging SSH Issues

Task 19 requires troubleshooting and resolving SSH connectivity problems between the control node and managed nodes. This task emphasizes the essential skills for ensuring secure and reliable communication in Ansible setups.

Objective

    Identify and fix SSH issues between the control node (controller.example.com) and managed nodes.

    Ensure passwordless SSH access is functional.

1. Understanding SSH in Ansible

    Role of SSH: Ansible uses SSH for communication with managed nodes. Passwordless SSH is recommended for seamless automation.

    Common SSH Issues:

        Missing or incorrect SSH keys.

        Host key verification failures.

        Incorrect user permissions or configurations.

2. Steps to Complete the Task

Step 1: Test SSH Connectivity

    Verify SSH Access:

        Test connectivity from the control node to a managed node:

            ssh ansibleuser@<managed-node-ip>

        If prompted for a password, passwordless SSH is not set up correctly.

    Check for Errors:

        Common errors include:

            Permission denied.

            Host key verification failed.

            Connection timed out.

Step 2: Configure Passwordless SSH Access

    Generate an SSH Key Pair:

        On the control node, generate a key pair if one does not exist:

            ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa

        Press Enter to accept the default path and file names.

    Copy the Public Key to Managed Nodes:

        Use the ssh-copy-id command to copy the public key to the managed nodes:

            ssh-copy-id ansibleuser@<managed-node-ip>

        Enter the password when prompted.

    Verify Passwordless SSH Access:

        Test SSH connectivity again:

            ssh ansibleuser@<managed-node-ip>

        Ensure no password is required.

Step 3: Fix Host Key Verification Issues

    Disable Host Key Checking (Temporary Fix):

        Edit /etc/ansible/ansible.cfg to disable host key checking:

            [defaults]
            host_key_checking = False

        Alternatively, set the ANSIBLE_HOST_KEY_CHECKING environment variable:

            export ANSIBLE_HOST_KEY_CHECKING=False

    Manually Accept Host Keys (Permanent Fix):

        Manually connect to each managed node to accept the host key:

            ssh ansibleuser@<managed-node-ip>

Step 4: Troubleshoot Common Issues

    Check SSH Daemon Status:

        Ensure the SSH service is running on the managed nodes:

            sudo systemctl status sshd

    Verify Firewall Rules:

        Ensure port 22 is open for SSH:

            sudo firewall-cmd --list-all
            sudo firewall-cmd --add-service=ssh --permanent
            sudo firewall-cmd --reload

    Validate Permissions:

        Ensure correct permissions for the .ssh directory and files on both control and managed nodes:

            chmod 700 ~/.ssh
            chmod 600 ~/.ssh/authorized_keys

Step 5: Test Ansible Connectivity

    Ping All Managed Nodes:

        Use Ansible to validate connectivity:

            ansible all -m ping

    Expected Output:

        All nodes should respond with:

            <managed-node>: SUCCESS => {
                "changed": false,
                "ping": "pong"
            }

3. Key Concepts for Novice Learners

    SSH Basics:

        Public and private key pairs enable secure communication.

        The public key must be added to the ~/.ssh/authorized_keys file on managed nodes.

    Common SSH Issues:

        Permission Denied: Indicates incorrect keys or user permissions.

        Host Key Verification Failed: Occurs when the host key changes.

    Ansible Configuration:

        Use ansible.cfg to manage SSH settings for all playbooks.

4. Troubleshooting Tips

    Connection Refused:

        Ensure the SSH daemon is running on the managed node:

            sudo systemctl start sshd

    Permission Denied:

        Verify the correct user and permissions on the managed node:

            ls -ld ~/.ssh
            ls -l ~/.ssh/authorized_keys

    Host Key Issues:

        Remove old host keys if necessary:

            ssh-keygen -R <managed-node-ip>

5. Summary

By completing this task, you’ve learned:

    How to troubleshoot and fix SSH connectivity issues.

    How to configure passwordless SSH access for Ansible.

    The importance of secure and reliable SSH communication in automation.

This task highlights critical skills for maintaining a robust Ansible environment, ensuring seamless connectivity between control and managed nodes.
