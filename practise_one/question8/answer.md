Overall explanation

# Solution for Task 8: MySQL Configuration

Objective

    Configure a MySQL YUM repository on the databases host group.

    Install the MySQL server.

    Verify the MySQL installation.

### 1. Understanding the Task

    YUM Repository Configuration: Setting up a repository enables MySQL installation even if it's not available in default system repositories.

    MySQL Server Installation: Automates the installation of MySQL server.

    Verification: Ensures MySQL is installed correctly.

### 2. Steps to Complete the Task

Step 1: Create the Playbook

Open a text editor and create the playbook:

    nano mysql_config.yml

Write the playbook:

    ---
    - name: Configure MySQL and Install Server
      hosts: databases
      tasks:
        - name: Add MySQL YUM repository
          copy:
            dest: /etc/yum.repos.d/mysql.repo
            content: |
              [mysql57-community]
              name=MySQL 5.7 Community Server
              baseurl=https://repo.mysql.com/yum/mysql-5.7-community/el/$releasever/$basearch/
              enabled=1
              gpgcheck=1
              gpgkey=https://repo.mysql.com/RPM-GPG-KEY-mysql
     
        - name: Install MySQL server
          yum:
            name: mysql-community-server
            state: present
     
        - name: Start and enable MySQL service
          service:
            name: mysqld
            state: started
            enabled: true
     
        - name: Verify MySQL installation
          command: mysql --version
          register: mysql_version
     
        - name: Display MySQL version
          debug:
            msg: "Installed MySQL version: {{ mysql_version.stdout }}"

Explanation of the Playbook

Play Level:

    hosts: databases: Applies the playbook only to hosts in the databases group.

    Tasks execute sequentially.

Tasks:

    Add MySQL YUM Repository:

        The copy module creates a repository file in /etc/yum.repos.d/mysql.repo.

        The baseurl specifies the repository location.

        gpgcheck and gpgkey ensure package authenticity.

    Install MySQL Server:

        The yum module installs mysql-community-server.

    Start and Enable MySQL Service:

        The service module ensures MySQL starts and runs at boot.

    Verify MySQL Installation:

        Runs mysql --version and stores output in mysql_version.

    Display MySQL Version:

        The debug module prints the installed MySQL version.

### Step 2: Execute the Playbook

Run the playbook:

    ansible-playbook mysql_config.yml

Expected Output

    Confirms repository file creation.

    Installs MySQL server package.

    Starts and enables MySQL service.

    Captures and displays the MySQL version.

### 3. Verify the Results

Check the Repository File:

    ansible databases -m shell -a "cat /etc/yum.repos.d/mysql.repo"

Ensure the file contains the correct repository configuration.

Check MySQL Service Status:

    ansible databases -m shell -a "systemctl status mysqld"

Ensure the service is active and running.

Verify MySQL Installation:

    ansible databases -m shell -a "mysql --version"

The output should display the installed MySQL version.

### 4. Key Concepts for Novice Learners

Repository Configuration:

    .repo files in /etc/yum.repos.d/ store repository metadata.

    baseurl points to the repository location.

    gpgkey ensures package authenticity.

Modules Used:

    copy: Creates or updates files on managed nodes.

    yum: Installs or updates packages on RHEL-based systems.

    service: Manages system services.

    command: Runs shell commands.

    debug: Displays custom messages during execution.

Variable Registration:

    The register keyword captures output for later use.

### 5. Troubleshooting Tips

- Repository Not Found:

Verify baseurl accessibility:

    curl -I https://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/

- Package Not Installed:

Ensure the repository file exists and is correctly formatted.

- MySQL Service Fails to Start:

Check logs for errors:

    ansible databases -m shell -a "journalctl -u mysqld"

- Command Not Found:

Ensure mysql binary is in the system’s PATH.

Summary

By completing this task, you’ve learned:

    How to configure YUM repositories.

    How to install and manage MySQL using Ansible.

    How to verify installations using the debug module.
