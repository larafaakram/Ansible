Overall explanation

Solution for Task 4: Configure a User with Specific Privileges

Task 4 involves creating a user named deployer with specific requirements, including group memberships, passwordless sudo access, and a hashed password. This task is a foundational part of managing user privileges and securing Linux systems.

Objective

    Create a user deployer with the following properties:

        Password hashed using SHA-512.

        Member of the wheel and developers groups.

        Passwordless sudo access.

    Validate the configuration using group membership and sudo commands.

1. Steps to Complete the Task

Step 1: Generate a Hashed Password

    Generate the Hashed Password:

        Use the openssl command to generate a SHA-512 hash for the password:

            openssl passwd -6

        Enter the password (e.g., SecurePassword123) when prompted.

        Example Output:

            $6$randomsalt$hashedvalue

    Copy the Hashed Password:

        Save the output for use in the Ansible playbook.

Step 2: Write the Playbook

    Create the Playbook File:

        Open a text editor to create a playbook named configure_user.yml:

            vi /home/ansibleadmin/ansible/configure_user.yml

    Add the Following Content:

        ---
        - name: Configure User with Specific Privileges
          hosts: all
          become: yes
          tasks:
            - name: Create the deployer user
              user:
                name: deployer
                password: "$6$randomsalt$hashedvalue"
                groups: wheel,developers
                state: present
         
            - name: Configure passwordless sudo for deployer
              copy:
                dest: /etc/sudoers.d/deployer
                content: |
                  deployer ALL=(ALL) NOPASSWD:ALL
                owner: root
                group: root
                mode: '0440'

    Explanation:

        user module:

            name: Specifies the username to create.

            password: Adds the SHA-512 hashed password.

            groups: Assigns the user to the specified groups.

            state: Ensures the user exists.

        copy module:

            Creates a sudoers configuration file for passwordless sudo.

            Sets appropriate permissions (0440) for security.

    Save and Exit:

        Save the playbook file and exit the editor.

Step 3: Execute the Playbook

    Run the Playbook:

        ansible-playbook /home/ansibleadmin/ansible/configure_user.yml

    Expected Output:

        The playbook should complete without errors, indicating that the user and sudo configuration were successfully applied.

Step 4: Validate the Configuration

    Check User Creation:

        Verify that the deployer user exists:

            id deployer

        Expected Output:

            uid=1001(deployer) gid=1001(deployer) groups=1001(deployer),10(wheel),1002(developers)

    Test Passwordless Sudo:

        Switch to the deployer user and test sudo access:

            su - deployer
            sudo ls /root

        Expected Output: The command should execute without prompting for a password.

    Verify Sudoers File:

        Ensure the sudoers configuration is correct:

            cat /etc/sudoers.d/deployer

        Expected Content:

            deployer ALL=(ALL) NOPASSWD:ALL

2. Key Concepts for Novice Learners

    User Management:

        The user module simplifies user creation and management.

        SHA-512 is a secure hashing algorithm for storing passwords.

    Passwordless Sudo:

        Allows a user to execute administrative commands without entering a password.

        The /etc/sudoers.d directory is used to manage user-specific sudo configurations.

    File Permissions:

        Sudoers files must have 0440 permissions to prevent unauthorized modifications.

3. Troubleshooting Tips

    User Not Created:

        Verify the playbook syntax:

            ansible-playbook /home/ansibleadmin/ansible/configure_user.yml --syntax-check

    Sudo Command Fails:

        Ensure the /etc/sudoers.d/deployer file exists and has the correct permissions:

            ls -l /etc/sudoers.d/deployer

    Passwordless Sudo Not Working:

        Check the sudoers file for syntax errors:

            visudo -cf /etc/sudoers.d/deployer

4. Summary

By completing this task, you’ve learned:

    How to create a user with specific privileges using Ansible.

    How to configure passwordless sudo for administrative tasks.

    How to validate user configurations and troubleshoot common issues.
