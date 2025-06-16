Overall explanation

# Solution for Task 6: Ansible Roles

Task 6 introduces the concept of Ansible roles, which allow you to organize and reuse automation tasks efficiently. In this task, you will create a role named webserver to manage the installation and configuration of an HTTP server (httpd), deploy a sample HTML file, and assign the role to a specific inventory group.

Objective

    Create an Ansible role named webserver using ansible-galaxy.

    Configure the role to:

        Install and enable httpd.

        Deploy a sample HTML file to /var/www/html/index.html.

    Assign the role to the webservers inventory group.

### 1. Understanding Ansible Roles

    Roles provide a way to structure your Ansible content into reusable components. Each role can have its tasks, handlers, variables, files, and templates.

    Roles simplify playbooks by organizing related tasks into separate directories.

### 2. Steps to Complete the Task

Step 1: Create the Role Using ansible-galaxy

    Run the ansible-galaxy init command to create the role:

        ansible-galaxy init webserver

        This command creates a directory structure for the role webserver.

    Navigate to the role directory:

        cd webserver

        Directory structure:

            webserver/
            ├── tasks/
            ├── handlers/
            ├── templates/
            ├── files/
            ├── vars/
            ├── defaults/
            ├── meta/

Step 2: Define Role Tasks

    Open the tasks/main.yml file to define the tasks:

        nano tasks/main.yml

    Add the following tasks to:

        Install and enable httpd.

        Deploy the HTML file.

        ---
        - name: Install httpd
          yum:
            name: httpd
            state: present
         
        - name: Start and enable httpd service
          service:
            name: httpd
            state: started
            enabled: true
         
        - name: Deploy sample HTML file
          copy:
            src: index.html
            dest: /var/www/html/index.html
            owner: root
            group: root
            mode: '0644'
          notify: Restart httpd

### Step 3: Add the Sample HTML File

    Place the sample HTML file in the files/ directory:

        nano files/index.html

    Add content to the HTML file:

        <!DOCTYPE html>
        <html>
        <head>
            <title>Welcome</title>
        </head>
        <body>
            <h1>Welcome to the Web Server</h1>
        </body>
        </html>

    Save and close the file.

### Step 4: Add a Handler to Restart httpd

    Open the handlers/main.yml file:

        nano handlers/main.yml

    Define a handler to restart the httpd service:

        ---
        - name: Restart httpd
          service:
            name: httpd
            state: restarted

    Explanation:

        Handlers are triggered by tasks using notify.

        Use this handler to restart the httpd service when configuration files are updated.

### Step 5: Assign the Role in a Playbook

    Create a new playbook named site.yml:

        nano site.yml

    Add the playbook content:

        ---
        - name: Configure webservers
          hosts: webservers
          roles:
            - webserver

    Explanation:

        hosts: webservers: Targets the webservers inventory group.

        roles: - webserver: Assigns the webserver role to this play.

### Step 6: Test the Role

    Run the playbook:

        ansible-playbook site.yml

    Verify the tasks executed successfully:

        Task 1 (Install httpd): Ensures httpd is installed.

        Task 2 (Start and enable httpd): Verifies httpd is running and enabled.

        Task 3 (Deploy sample HTML file): Confirms the HTML file is in the correct location with appropriate permissions.

3. Validation

    Check if the httpd service is running:

        ansible webservers -m shell -a "systemctl status httpd"

    Verify the HTML file:

        Access the server’s IP or hostname in a web browser:

            http://<webserver-IP>

        You should see the "Welcome to the Web Server" page.

Key Concepts for Novice Learners

    Roles:

        Provide a structured way to organize tasks, files, and configurations.

        Promote reusability and simplify playbooks.

    Modules Used:

        yum: Installs or removes packages.

        service: Manages services (start, stop, enable).

        copy: Copies files from the control node to managed nodes.

    Handlers:

        Triggered by tasks using notify.

        Used for actions like restarting services after configuration changes.

Troubleshooting Tips

    Role Not Found:

        Ensure the role directory is in the correct location (roles/ or /etc/ansible/roles).

    HTML File Not Deployed:

        Verify the src path in the copy task matches the location of index.html.

    Service Not Starting:

        Check if httpd is installed correctly:

            ansible webservers -m shell -a "yum list installed httpd"

Summary

By completing this task, you’ve learned:

    How to create and organize Ansible roles.

    The importance of reusable components for efficient automation.

    How to manage services, deploy files, and use handlers within roles.

This task provides the foundational skills to work with roles, a critical aspect of automation and configuration management using Ansible.
