Overall explanation

# Solution for Task 10: Web Server Deployment

Task 10 involves deploying a web server (httpd or nginx) on the webservers group and verifying that the web server is accessible via HTTP. This task introduces fundamental skills in managing web servers and ensuring their proper deployment.

Objective

    Deploy either httpd or nginx on the webservers group.

    Verify that the web server is running and accessible via HTTP.

### 1. Understanding the Task

    Web Server Deployment: A web server serves HTTP content, such as HTML files, to clients. Popular web servers include httpd (Apache) and nginx.

    Verification: Ensures the web server is active and serving content.

### 2. Steps to Complete the Task

Step 1: Create the Playbook

    Open a text editor to create the playbook:

        vi webserver_deployment.yml

    Write the playbook:

        ---
        - name: Deploy Web Server
          hosts: webservers
          tasks:
            - name: Install httpd on Red Hat-based systems
              yum:
                name: httpd
                state: present
              when: ansible_facts['os_family'] == 'RedHat'
         
            - name: Install nginx on Debian-based systems
              apt:
                name: nginx
                state: present
              when: ansible_facts['os_family'] == 'Debian'
         
            - name: Start and enable the web server
              service:
                name: "{{ 'httpd' if ansible_facts['os_family'] == 'RedHat' else 'nginx' }}"
                state: started
                enabled: true
         
            - name: Deploy a sample HTML file
              copy:
                dest: /var/www/html/index.html
                content: |
                  <!DOCTYPE html>
                  <html>
                  <head>
                      <title>Welcome</title>
                  </head>
                  <body>
                      <h1>Welcome to the Web Server</h1>
                  </body>
                  </html>
                owner: root
                group: root
                mode: '0644'
         
            - name: Verify the web server is accessible
              uri:
                url: http://{{ ansible_default_ipv4.address }}
                return_content: yes
              register: web_content
         
            - name: Display verification result
              debug:
                msg: "Web server content: {{ web_content.content }}"

Explanation of the Playbook

    Play Level:

        hosts: webservers: Targets the webservers inventory group.

    Task 1 (Install Web Server):

        Installs httpd or nginx based on the operating system family.

        Uses the yum module for Red Hat and apt module for Debian.

        The when clause ensures the correct package is installed for the OS.

    Task 2 (Start and Enable Web Server):

        Starts and enables the web server service using the service module.

        Dynamically selects the service name (httpd for Red Hat or nginx for Debian).

    Task 3 (Deploy Sample HTML File):

        Creates a sample HTML file (index.html) in the web server's root directory (/var/www/html).

        Ensures proper ownership and permissions.

    Task 4 (Verify Web Server):

        Uses the uri module to send an HTTP request to the server's IP.

        Captures the response content in the web_content variable.

    Task 5 (Display Verification Result):

        Uses the debug module to display the HTML content returned by the web server.

Step 2: Execute the Playbook

    Run the playbook:

        ansible-playbook webserver_deployment.yml

    Expected Output:

        Task 1 installs the appropriate web server package.

        Task 2 starts and enables the web server.

        Task 3 creates the sample HTML file.

        Task 4 verifies the web server is serving content.

        Task 5 displays the HTML content.

### 3. Verify the Results

Check the Web Server Status

    Ensure the web server service is running:

        ansible webservers -m shell -a "systemctl status httpd"  # For Red Hat
        ansible webservers -m shell -a "systemctl status nginx"  # For Debian

Verify Web Server Content

    Access the web server in a browser:

        Use the IP address of a managed node:

            http://<webserver-IP>

        The browser should display the "Welcome to the Web Server" page.

    Test the server using curl:

        curl http://<webserver-IP>

        Output should display the HTML content of the sample page.

### 4. Key Concepts for Novice Learners

    Dynamic Service Selection:

        Uses Ansible facts to determine the OS and dynamically selects the service to manage.

        Example:

            "{{ 'httpd' if ansible_facts['os_family'] == 'RedHat' else 'nginx' }}"

    Modules Used:

        yum/apt: Installs software packages.

        service: Manages services (start, stop, enable).

        copy: Creates or updates files.

        uri: Sends HTTP requests to test connectivity and content delivery.

        debug: Displays information for verification.

    Facts:

        ansible_facts['os_family']: Determines the operating system family (e.g., RedHat, Debian).

### 5. Troubleshooting Tips

    Service Fails to Start:

        Check service logs for errors:

            ansible webservers -m shell -a "journalctl -u httpd"  # For Red Hat
            ansible webservers -m shell -a "journalctl -u nginx"  # For Debian

    Web Server Not Accessible:

        Verify the firewall allows HTTP traffic:

            ansible webservers -m shell -a "firewall-cmd --list-all"

    Content Not Displayed:

        Ensure the HTML file exists and has the correct permissions:

            ansible webservers -m shell -a "ls -l /var/www/html/index.html"

Summary

By completing this task, you’ve learned:

    How to deploy a web server dynamically based on the OS family.

    How to serve static HTML content using Ansible.

    How to verify web server functionality using HTTP requests.

This task builds foundational skills for deploying and managing web servers, a critical competency for system administrators and automation engineers.
