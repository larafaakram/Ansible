Overall explanation

# Solution for Task 3: Managing Variables and Facts

Task 3 involves defining variables in a YAML file, creating a playbook to monitor disk usage, and using the debug module to display relevant information. This task helps beginners understand how to work with variables and facts in Ansible.

Objective

    Define the following variables in vars.yml:

        disk_partition: /

        disk_usage_threshold: 80

    Write a playbook to monitor disk usage on managed nodes.

    Use the debug module to display disk usage and trigger a failure if the threshold exceeds 80%.

### 1. Define Variables in vars.yml

Variables allow you to store reusable data that can be referenced in playbooks.

    Create the variables file:

        nano vars.yml

    Add the variables:

        ---
        disk_partition: "/"
        disk_usage_threshold: 80

    Explanation:

        disk_partition: Specifies the partition to monitor (root partition / in this case).

        disk_usage_threshold: Defines the maximum allowable disk usage percentage before triggering an alert.

    Save and close the file.

### 2. Create the Monitoring Playbook

The playbook will:

    Use the ansible.builtin.shell module to retrieve disk usage.

    Use the debug module to display the usage.

    Trigger a failure if the usage exceeds the threshold.

    Create the playbook file:

        nano monitor_disk.yml

    Write the playbook:

        ---
        - name: Monitor Disk Usage
          hosts: all
          vars_files:
            - vars.yml
          tasks:
            - name: Check disk usage
              shell: df -h {{ disk_partition }} | awk 'NR==2 {print $5}' | tr -d '%'
              register: disk_usage
         
            - name: Debug disk usage
              debug:
                msg: "Disk usage on partition {{ disk_partition }} is {{ disk_usage.stdout }}%"
         
            - name: Fail if disk usage exceeds threshold
              fail:
                msg: "Disk usage on {{ disk_partition }} is {{ disk_usage.stdout }}%, which exceeds the threshold of {{ disk_usage_threshold }}%"
              when: disk_usage.stdout|int > disk_usage_threshold

    Explanation of the Playbook:

        Play Level:

            hosts: all: Applies the playbook to all managed nodes.

            vars_files: Includes vars.yml for reusable variables.

        Task 1 (Check Disk Usage):

            The shell module runs a command to get the disk usage of the specified partition:

                df -h {{ disk_partition }}: Displays disk usage for the specified partition.

                awk 'NR==2 {print $5}': Extracts the percentage value.

                tr -d '%': Removes the % symbol.

            The output is stored in the disk_usage variable.

        Task 2 (Debug Disk Usage):

            The debug module displays the current disk usage for the partition.

        Task 3 (Fail if Exceeds Threshold):

            The fail module stops execution and raises an alert if the disk usage exceeds the defined threshold.

            The when condition checks if disk_usage.stdout is greater than disk_usage_threshold.

    Save and close the file.

### 3. Execute the Playbook

    Run the playbook:

        ansible-playbook monitor_disk.yml

    Expected Output:

        Task 1 (Check disk usage): Retrieves the disk usage.

        Task 2 (Debug disk usage): Outputs a message like:

            TASK [Debug disk usage] *********************************************************
            ok: [node1.example.com] => {
                "msg": "Disk usage on partition / is 45%"
            }

        Task 3 (Fail if exceeds threshold):

            If disk usage exceeds 80%, the task fails with a message like:

                TASK [Fail if disk usage exceeds threshold] ***********************************
                fatal: [node1.example.com]: FAILED! => {"msg": "Disk usage on / is 85%, which exceeds the threshold of 80%"}

### 4. Verify the Results

    Check for Failures:

        If any node exceeds the threshold, the playbook will fail for that node with a descriptive error.

    Rerun for Debugging:

        Use ansible-playbook --check monitor_disk.yml to simulate the playbook execution without making changes.

Key Concepts for Novice Learners

    Variables (vars.yml): Centralize reusable data for consistency and maintainability.

    Modules Used:

        shell: Executes shell commands on managed nodes.

        debug: Displays custom messages during playbook execution.

        fail: Triggers failure based on conditions.

    Conditionals (when): Add logic to tasks, ensuring they execute only when specified conditions are met.

    Register: Captures the output of a task for later use.

Troubleshooting Tips

    Playbook Fails Due to Syntax Errors:

        Run:

            ansible-playbook --syntax-check monitor_disk.yml

            Correct any errors before execution.

    No Output from df Command:

        Ensure the partition specified in disk_partition exists on all managed nodes.

    Incorrect Comparison in when:

        Use the |int filter to ensure numerical comparison:

            when: disk_usage.stdout|int > disk_usage_threshold

Summary

By completing this task, you’ve learned:

    How to define variables in Ansible.

    How to use shell commands to gather data and process it within playbooks.

    How to use the debug and fail modules to display and validate information.

    The importance of conditional execution with when.

This task provides foundational skills for using Ansible to monitor and manage systems effectively.
