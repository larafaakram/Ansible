Overall explanation

Solution for Task 1: Configure Ansible on the Control Node

Task 1 involves setting up Ansible on the control node by installing the required package, configuring essential settings in the ansible.cfg file, and verifying the installation.

Objective

    Install ansible-core on the control node.

    Create the /home/ansibleadmin/ansible directory for Ansible files.

    Configure the ansible.cfg file with specific settings:

        Default inventory file: /home/ansibleadmin/ansible/inventory

        Disable host key checking: host_key_checking = False

        Set logging: /var/log/ansible/execution.log

    Verify the Ansible installation.

1. Install Ansible-Core

    Update the Package Manager:

        sudo yum update -y

    Install ansible-core:

        sudo yum install -y ansible-core

    Verify Installation:

        Check the Ansible version to confirm the installation:

            ansible --version

        Expected Output:

            ansible [core 2.x.x]

2. Create the Ansible Directory

    Create the Directory:

        Use the mkdir command to create the directory structure:

            mkdir -p /home/ansibleadmin/ansible

    Set Permissions:

        Ensure the directory is owned by ansibleadmin:

            sudo chown ansibleadmin:ansibleadmin /home/ansibleadmin/ansible

3. Configure ansible.cfg

    Create the Configuration File:

        Open the configuration file for editing:

            nano /home/ansibleadmin/ansible/ansible.cfg

    Add the Required Settings:

        Configure the following settings in ansible.cfg:

            [defaults]
            inventory = /home/ansibleadmin/ansible/inventory
            host_key_checking = False
            log_path = /var/log/ansible/execution.log

    Explanation of Settings:

        inventory: Sets the default inventory file location.

        host_key_checking: Disables SSH host key verification to avoid interruptions.

        log_path: Specifies the location for logging Ansible execution details.

    Set Permissions:

        Ensure the file is owned by ansibleadmin:

            sudo chown ansibleadmin:ansibleadmin /home/ansibleadmin/ansible/ansible.cfg

4. Verify Ansible Configuration

    Check Ansible Version:

        Verify that Ansible is installed and configured correctly:

            ansible --version

    Test Inventory Configuration:

        If the inventory file exists, list the nodes:

            ansible-inventory --list

        Ensure the inventory and configuration file are loaded without errors.

    Verify Logging:

        Execute a simple ping command to generate logs:

            ansible all -m ping

        Check the log file for execution details:

            cat /var/log/ansible/execution.log

5. Troubleshooting Tips

    Package Not Found:

        Ensure the EPEL repository is enabled:

            sudo yum install -y epel-release

    Permission Issues:

        Ensure the ansibleadmin user has ownership of the Ansible directory:

            sudo chown -R ansibleadmin:ansibleadmin /home/ansibleadmin/ansible

    Configuration File Errors:

        Validate the configuration file for syntax issues:

            ansible-config dump --only-changed

6. Key Concepts for Novice Learners

    Ansible-Core: The core package required for running Ansible commands and playbooks.

    ansible.cfg: The configuration file where default settings for Ansible are defined.

    Logging: Essential for tracking and troubleshooting Ansible executions.

7. Summary

By completing this task, you’ve learned:

    How to install and configure Ansible on a control node.

    How to customize ansible.cfg to set inventory, disable host key checking, and enable logging.

    How to verify the Ansible installation and configuration.
