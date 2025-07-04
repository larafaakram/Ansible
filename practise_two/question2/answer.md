Overall explanation

Solution for Task 2: Create an Inventory File

Task 2 requires creating a static inventory file for Ansible that organizes managed nodes into logical groups. This inventory file serves as the foundation for targeting nodes in Ansible playbooks.

Objective

    Create an inventory file at /home/ansibleadmin/ansible/inventory.

    Include the following groups and their respective nodes:

        [application]: node1.example.com

        [databases]: node2.example.com

        [backup]: node3.example.com

        [webservers]: node4.example.com

    Use fully qualified domain names (FQDNs).

1. Steps to Complete the Task

Step 1: Create the Inventory File

    Navigate to the Ansible Directory:

        cd /home/ansibleadmin/ansible

    Open a New File:

        Use a text editor to create the inventory file:

            nano inventory

    Add Inventory Content:

        Enter the following content into the file:

            [application]
            node1.example.com
             
            [databases]
            node2.example.com
             
            [backup]
            node3.example.com
             
            [webservers]
            node4.example.com

    Save and Exit:

        Save the file and exit the editor (e.g., press CTRL+O, then CTRL+X in Nano).

Step 2: Verify the Inventory File

    Check File Location and Permissions:

        Ensure the inventory file exists and is owned by ansibleadmin:

            ls -l /home/ansibleadmin/ansible/inventory

    List the Inventory:

        Use the ansible-inventory command to verify the file:

            ansible-inventory -i inventory --list

    Expected Output:

        The command should display the inventory groups and their associated nodes in JSON format:

            {
                "_meta": {
                    "hostvars": {}
                },
                "application": {
                    "hosts": ["node1.example.com"]
                },
                "databases": {
                    "hosts": ["node2.example.com"]
                },
                "backup": {
                    "hosts": ["node3.example.com"]
                },
                "webservers": {
                    "hosts": ["node4.example.com"]
                }
            }

Step 3: Test Connectivity

    Ping All Nodes:

        Test connectivity to all nodes listed in the inventory:

            ansible all -m ping -i inventory

    Expected Output:

        Each node should return a SUCCESS response:

            node1.example.com | SUCCESS => {
                "changed": false,
                "ping": "pong"
            }
            node2.example.com | SUCCESS => {
                "changed": false,
                "ping": "pong"
            }
            node3.example.com | SUCCESS => {
                "changed": false,
                "ping": "pong"
            }
            node4.example.com | SUCCESS => {
                "changed": false,
                "ping": "pong"
            }

2. Key Concepts for Novice Learners

    Inventory File:

        The inventory file defines groups and hosts for Ansible to manage.

        Hosts can be grouped logically (e.g., application servers, databases).

    INI Format:

        The inventory file uses a simple INI format, with groups enclosed in square brackets ([group_name]).

    ansible-inventory Command:

        Lists and validates the inventory file.

3. Troubleshooting Tips

    Inventory Not Found:

        Verify the file path:

            ls /home/ansibleadmin/ansible/inventory

    Ping Fails:

        Check for connectivity issues between the control node and managed nodes.

        Ensure the FQDNs in the inventory file resolve to the correct IP addresses:

            ping node1.example.com

    Permission Issues:

        Ensure the file is owned by ansibleadmin:

            sudo chown ansibleadmin:ansibleadmin /home/ansibleadmin/ansible/inventory

4. Summary

By completing this task, you’ve learned:

    How to create a static inventory file for Ansible.

    How to group managed nodes based on their roles.

    How to verify and test the inventory file using Ansible commands.
