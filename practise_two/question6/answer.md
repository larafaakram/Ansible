Overall explanation

Solution for Task 6: Secure SSH Configuration

Task 6 involves creating a playbook to secure the SSH configuration across all managed nodes. This includes disabling password-based authentication, denying root login, setting an idle session timeout, and configuring an SSH banner.

Objective

    Disable password-based authentication.

    Deny root login.

    Set an idle session timeout to 10 minutes.

    Configure an SSH banner.

    Verify the changes.

1. Steps to Complete the Task

Step 1: Write the Playbook

    Create the Playbook File:

        Open a text editor to create the playbook:

            vi /home/ansibleadmin/ansible/secure_ssh.yml

    Add the Following Content:

        ---
        - name: Secure SSH Configuration
          hosts: all
          become: yes
          tasks:
            - name: Disable password-based authentication
              lineinfile:
                path: /etc/ssh/sshd_config
                regexp: '^#?PasswordAuthentication'
                line: 'PasswordAuthentication no'
                state: present
         
            - name: Deny root login
              lineinfile:
                path: /etc/ssh/sshd_config
                regexp: '^#?PermitRootLogin'
                line: 'PermitRootLogin no'
                state: present
         
            - name: Set idle session timeout
              lineinfile:
                path: /etc/ssh/sshd_config
                regexp: '^#?ClientAliveInterval'
                line: 'ClientAliveInterval 600'
                state: present
         
            - name: Configure SSH banner
              lineinfile:
                path: /etc/ssh/sshd_config
                regexp: '^#?Banner'
                line: 'Banner /etc/issue.net'
                state: present
         
            - name: Create SSH banner file
              copy:
                dest: /etc/issue.net
                content: |
                  ***************************************************
                  * Authorized Access Only. Unauthorized use is      *
                  * strictly prohibited and will be prosecuted.      *
                  ***************************************************
                owner: root
                group: root
                mode: '0644'
         
            - name: Restart SSH service
              service:
                name: sshd
                state: restarted

Step 2: Execute the Playbook

    Run the Playbook:

        ansible-playbook /home/ansibleadmin/ansible/secure_ssh.yml

    Expected Output:

        The playbook should execute without errors, and the SSH service will restart with the updated configuration.

Step 3: Verify the Changes

    Check SSH Configuration:

        Verify that the sshd_config file contains the updated parameters:

            ansible all -m shell -a "grep -E 'PasswordAuthentication|PermitRootLogin|ClientAliveInterval|Banner' /etc/ssh/sshd_config"

    Test Password Authentication:

        Attempt to log in using a password. The login should fail.

    Test Root Login:

        Attempt to log in as the root user. The login should be denied.

    Test Idle Timeout:

        Leave an SSH session idle for 10 minutes to confirm the timeout.

    Check SSH Banner:

        Log in via SSH to confirm the banner is displayed.

2. Key Concepts for Novice Learners

    SSH Security Settings:

        PasswordAuthentication: Disables password-based login for enhanced security.

        PermitRootLogin: Denies direct root access to reduce risks.

        ClientAliveInterval: Sets the idle session timeout in seconds.

        Banner: Displays a legal or informational message during login.

    Modules Used:

        lineinfile: Edits configuration files.

        copy: Creates or updates files with specific content.

        service: Restarts the SSH service to apply changes.

3. Troubleshooting Tips

    SSH Service Fails to Restart:

        Check for syntax errors in sshd_config:

            ansible all -m shell -a "sshd -t"

    Changes Not Applied:

        Ensure the playbook executed successfully.

        Verify the SSH service was restarted.

    Idle Timeout Not Working:

        Confirm the ClientAliveInterval value in sshd_config.

4. Summary

By completing this task, you’ve learned:

    How to secure SSH access by disabling password authentication and root login.

    How to set an idle session timeout for better security.

    How to configure and deploy an SSH banner for informational or legal purposes.
