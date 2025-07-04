Overall explanation

# Solution for Task 7: Conditional Tasks

Task 7 involves writing an Ansible playbook that performs specific actions based on the type of operating system. You will learn how to use conditionals to install httpd on Red Hat nodes, nginx on Debian nodes, and loops to install multiple packages.

Objective

    Write a playbook to:

        Install httpd on Red Hat-based systems.

        Install nginx on Debian-based systems.

    Use loops to install multiple packages.

1. Understanding Conditional Tasks in Ansible

    Conditionals (when):

        Allows tasks to execute only when a specified condition is true.

        In this task, the condition is based on the ansible_facts gathered by Ansible about the managed node's operating system.

    Loops:

        Used to execute the same task multiple times with different inputs.

2. Steps to Complete the Task

### Step 1: Create the Playbook

    Open a text editor to create the playbook:

        nano conditional_tasks.yml

    Write the playbook:

        ---
        - name: Install packages conditionally
          hosts: all
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
         
            - name: Install multiple packages
              yum:
                name: "{{ item }}"
                state: present
              loop:
                - vim
                - curl
                - git
              when: ansible_facts['os_family'] == 'RedHat'
         
            - name: Install multiple packages on Debian
              apt:
                name: "{{ item }}"
                state: present
              loop:
                - vim
                - curl
                - git
              when: ansible_facts['os_family'] == 'Debian'

Explanation of the Playbook

    Play Level:

        hosts: all: Targets all managed nodes in the inventory.

        Tasks are executed sequentially.

    Task 1 (Install httpd):

        yum: Manages package installation on Red Hat-based systems.

        when: ansible_facts['os_family'] == 'RedHat': Ensures the task runs only on Red Hat-based systems.

    Task 2 (Install nginx):

        apt: Manages package installation on Debian-based systems.

        when: ansible_facts['os_family'] == 'Debian': Ensures the task runs only on Debian-based systems.

    Task 3 (Install multiple packages on Red Hat):

        loop: Repeats the yum module task for each item in the list.

        Items (vim, curl, git) are installed sequentially.

        when ensures this task is limited to Red Hat-based systems.

    Task 4 (Install multiple packages on Debian):

        Similar to Task 3 but uses apt for Debian-based systems.

Step 2: Test the Playbook

    Run the playbook:

        ansible-playbook conditional_tasks.yml

    Expected Output:

        On Red Hat-based nodes:

            httpd is installed.

            vim, curl, and git are installed.

        On Debian-based nodes:

            nginx is installed.

            vim, curl, and git are installed.

### 3. Verify the Results

Check Installed Packages

    For Red Hat Nodes:

        Verify httpd is installed:

            ansible all -m shell -a "rpm -q httpd"

        Verify multiple packages:

            ansible all -m shell -a "rpm -q vim curl git"

    For Debian Nodes:

        Verify nginx is installed:

            ansible all -m shell -a "dpkg -l | grep nginx"

        Verify multiple packages:

            ansible all -m shell -a "dpkg -l | grep -E 'vim|curl|git'"

### 4. Key Concepts for Novice Learners

    Conditionals (when):

        Use facts collected by Ansible to decide task execution.

        Example:

            when: ansible_facts['os_family'] == 'RedHat'

    Modules:

        yum: Used for package management on Red Hat-based systems.

        apt: Used for package management on Debian-based systems.

    Loops:

        Automate repetitive tasks for multiple items.

        Example:

            loop:
              - vim
              - curl
              - git

    Facts:

        Ansible gathers information about managed nodes, stored in ansible_facts.

        os_family fact indicates the operating system family (e.g., RedHat or Debian).

### 5. Troubleshooting Tips

    Packages Not Installed:

        Verify that the ansible_facts['os_family'] value matches the target system’s OS family:

            ansible all -m setup | grep os_family

    Playbook Syntax Issues:

        Run:

            ansible-playbook --syntax-check conditional_tasks.yml

        Correct any errors before execution.

    Loop Failures:

        Ensure the package names are correct for the OS family.

Summary

By completing this task, you’ve learned:

    How to use conditionals (when) to target tasks based on system properties.

    How to automate repetitive tasks using loops.

    The importance of ansible_facts for gathering and utilizing system information.

This task helps you understand how to create flexible, dynamic playbooks tailored to diverse environments, a critical skill for advanced system automation using Ansible.

