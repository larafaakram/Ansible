Overall explanation

# Solution for Task 9: SSH Security

Task 9 involves hardening SSH configurations on proxy nodes by disabling root login, setting up key-based authentication, and restricting access to specific users. This task emphasizes secure access practices for managed nodes.

Objective

    Disable root login via SSH.

    Set up key-based authentication.

    Restrict SSH access to specific users.

    Verify connectivity after applying changes.

1. Understanding SSH Hardening

    Disable Root Login: Prevents direct root access over SSH, enhancing security by requiring users to authenticate with non-root accounts.

    Key-Based Authentication: Uses SSH keys instead of passwords for secure, automated login.

    Restrict Access: Limits SSH access to specific users, reducing the risk of unauthorized access.

### 2. Steps to Complete the Task

Step 1: Create the Playbook

    Open a text editor to create the playbook:

        vi ssh_security.yml

    Write the playbook:

        ---
        - name: Secure SSH Configuration
          hosts: proxy
          tasks:
            - name: Disable root login
              lineinfile:
                path: /etc/ssh/sshd_config
                regexp: '^PermitRootLogin'
                line: 'PermitRootLogin no'
                state: present
                backup: yes
              notify: Restart SSH
         
            - name: Disable password authentication
              lineinfile:
                path: /etc/ssh/sshd_config
                regexp: '^PasswordAuthentication'
                line: 'PasswordAuthentication no'
                state: present
                backup: yes
              notify: Restart SSH
         
            - name: Restrict SSH access to specific users
              lineinfile:
                path: /etc/ssh/sshd_config
                line: 'AllowUsers ansibleuser'
                state: present
                backup: yes
              notify: Restart SSH
         
            - name: Create ansibleuser if not present
              user:
                name: ansibleuser
                state: present
                shell: /bin/bash
         
            - name: Set up SSH key-based authentication
              authorized_key:
                user: ansibleuser
                state: present
                key: "{{ lookup('file', '/home/ansibleuser/.ssh/id_rsa.pub') }}"
         
          handlers:
            - name: Restart SSH
              service:
                name: sshd
                state: restarted

### 3. Explanation of the Playbook

    Play Level:

        hosts: proxy: Targets the proxy nodes group.

    Task 1 (Disable Root Login):

        Uses the lineinfile module to ensure PermitRootLogin no is present in /etc/ssh/sshd_config.

        backup: yes creates a backup of the original file before changes.

    Task 2 (Disable Password Authentication):

        Ensures PasswordAuthentication no is configured in the SSH configuration file.

    Task 3 (Restrict SSH Access):

        Adds AllowUsers ansibleuser to limit SSH access to the ansibleuser.

    Task 4 (Create ansibleuser):

        Uses the user module to create the ansibleuser if it doesn’t exist.

    Task 5 (Set Up Key-Based Authentication):

        Uses the authorized_key module to copy the public key from the control node to the managed nodes for the ansibleuser.

    Handler (Restart SSH):

        Restarts the SSH service to apply configuration changes.

### 4. Execute the Playbook

    Run the playbook:

        ansible-playbook ssh_security.yml

    Expected Output:

        Tasks modify the SSH configuration, create the ansibleuser, and set up key-based authentication.

        The SSH service restarts to apply the changes.

### 5. Verify the Results

    Check SSH Configuration:

        ansible proxy -m shell -a "grep -E 'PermitRootLogin|PasswordAuthentication|AllowUsers' /etc/ssh/sshd_config"

        Verify the following lines are present:

            PermitRootLogin no
            PasswordAuthentication no
            AllowUsers ansibleuser

    Test Key-Based Login:

        From the control node, test logging in as ansibleuser:

            ssh ansibleuser@<proxy-node>

    Check SSH Service Status:

        ansible proxy -m shell -a "systemctl status sshd"

        Ensure the service is active and running.

### 6. Key Concepts for Novice Learners

    sshd_config:

        The primary configuration file for the SSH service.

        Critical options include PermitRootLogin, PasswordAuthentication, and AllowUsers.

    Modules Used:

        lineinfile: Modifies lines in a file.

        user: Manages user accounts.

        authorized_key: Manages SSH keys for users.

        service: Controls services (start, stop, restart).

    Handlers:

        Used to restart services after configuration changes.

### 7. Troubleshooting Tips

    SSH Service Fails to Restart:

        Check for syntax errors in /etc/ssh/sshd_config:

            sshd -t

    Key-Based Login Fails:

        Ensure the correct public key is copied to the managed nodes:

            cat /home/ansibleuser/.ssh/authorized_keys

    Cannot SSH as ansibleuser:

        Verify the AllowUsers directive includes ansibleuser.

Summary

By completing this task, you’ve learned:

    How to secure SSH access by disabling root login and password authentication.

    How to set up key-based authentication for secure and automated login.

    How to restrict SSH access to specific users.

This task emphasizes the importance of securing SSH access to managed nodes, a critical practice in system administration and automation.
