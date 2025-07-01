Overall explanation

Solution for Task 17: Service Management

Task 17 involves creating a playbook to manage system services, specifically ensuring that the firewalld service is running and managing the httpd service on web servers. This task emphasizes essential skills for controlling services with Ansible.

Objective

    Ensure firewalld is running on all nodes.

    Start and enable the httpd service on nodes in the webservers group.

1. Understanding Service Management with Ansible

    firewalld: A firewall management tool for Red Hat-based systems, critical for network security.

    httpd: The Apache web server, commonly used for serving web content.

    Service Module: Ansible's service module is used to start, stop, enable, or disable system services.

2. Steps to Complete the Task

Step 1: Create the Playbook

    Open a text editor to create the playbook:

        vi service_management.yml

    Write the playbook:

        ---
        - name: Manage Services
          hosts: all
          tasks:
            - name: Ensure firewalld is running
              service:
                name: firewalld
                state: started
                enabled: true
         
        - name: Manage HTTPD on Web Servers
          hosts: webservers
          tasks:
            - name: Install httpd if not already installed
              yum:
                name: httpd
                state: present
         
            - name: Start and enable httpd service
              service:
                name: httpd
                state: started
                enabled: true

3. Explanation of the Playbook

Playbook Structure:

    Play 1: Manage firewalld on All Nodes

        Ensures the firewalld service is running and enabled to start on boot.

        Uses the service module for service control.

    Play 2: Manage httpd on Web Servers

        Installs the httpd package on nodes in the webservers group if not already installed.

        Starts and enables the httpd service to ensure it runs at boot.

Module Details:

    service Module:

        name: Specifies the service to manage.

        state: started: Ensures the service is running.

        enabled: true: Ensures the service starts on boot.

    yum Module:

        name: Specifies the package to manage.

        state: present: Ensures the package is installed.

4. Execute the Playbook

    Run the playbook:

        ansible-playbook service_management.yml

    Expected Output:

        Ensures firewalld is running on all nodes.

        Installs, starts, and enables httpd on nodes in the webservers group.

5. Verify the Results

firewalld:

    Check the status of firewalld on all nodes:

        ansible all -m shell -a "systemctl status firewalld"

httpd:

    Verify the httpd installation on web servers:

        ansible webservers -m shell -a "rpm -q httpd"

    Check the status of the httpd service:

        ansible webservers -m shell -a "systemctl status httpd"

    Test the web server:

        Access the server's IP address in a web browser:

            http://<webserver-IP>

6. Key Concepts for Novice Learners

    Service Management:

        firewalld is critical for managing firewall rules and security.

        httpd is used to serve web content.

    Modules Used:

        service: Manages system services.

        yum: Manages packages on Red Hat-based systems.

    Group Targeting:

        Hosts in the webservers group are managed separately.

7. Troubleshooting Tips

    Service Fails to Start:

        Check service logs:

            ansible all -m shell -a "journalctl -u firewalld"
            ansible webservers -m shell -a "journalctl -u httpd"

    Package Installation Issues:

        Verify the repository configuration:

            ansible webservers -m shell -a "yum repolist"

    Connectivity Issues:

        Ensure firewalld rules allow HTTP traffic:

            ansible all -m shell -a "firewall-cmd --list-all"

Summary

By completing this task, you’ve learned:

    How to manage system services using Ansible.

    How to ensure critical services like firewalld and httpd are installed, started, and enabled.

    The importance of securing services and ensuring they run at startup.

This task builds essential skills for managing and automating service configurations in Red Hat-based environments.
