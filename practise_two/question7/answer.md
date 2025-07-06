Overall explanation

Solution for Task 7: Archive System Logs

Task 7 involves creating a playbook to archive the /var/log directory into a compressed file named system-logs.zip and storing it in the /backups directory on all managed nodes. The archive should have specific ownership settings.

Objective

    Archive the /var/log directory into a file named system-logs.zip located at /backups/system-logs.zip.

    Set ownership of the archive to ansibleadmin.

    Verify the archive creation and permissions.

1. Steps to Complete the Task

Step 1: Write the Playbook

    Create the Playbook File:

        Open a text editor to create the playbook:

            vi /home/ansibleadmin/ansible/archive_logs.yml

    Add the Following Content:

        ---
        - name: Archive System Logs
          hosts: all
          become: yes
          tasks:
            - name: Ensure /backups directory exists
              file:
                path: /backups
                state: directory
                owner: ansibleadmin
                group: ansibleadmin
                mode: '0755'
         
            - name: Archive /var/log into /backups/system-logs.zip
              archive:
                path: /var/log
                dest: /backups/system-logs.zip
                format: zip
         
            - name: Set ownership of the archive
              file:
                path: /backups/system-logs.zip
                owner: ansibleadmin
                group: ansibleadmin

Explanation of the Playbook:

    Ensure Directory Exists:

        The file module ensures the /backups directory is present with the correct ownership and permissions.

    Archive Logs:

        The archive module compresses the /var/log directory into a .zip file and saves it in /backups.

    Set Ownership:

        The file module adjusts the ownership of the archive file to ansibleadmin.

Step 2: Execute the Playbook

    Run the Playbook:

        ansible-playbook /home/ansibleadmin/ansible/archive_logs.yml

    Expected Output:

        The playbook should complete successfully, creating the archive and setting the correct ownership.

Step 3: Verify the Results

    Check the Archive File:

        Verify the presence and ownership of the archive file:

            ansible all -m shell -a "ls -l /backups/system-logs.zip"

        Expected Output:

            -rw-r--r-- 1 ansibleadmin ansibleadmin <size> /backups/system-logs.zip

    Test the Archive:

        Verify the contents of the archive:

            ansible all -m shell -a "unzip -l /backups/system-logs.zip"

        Ensure the output lists files from /var/log.

2. Key Concepts for Novice Learners

    Directory Management:

        Use the file module to create directories and set permissions.

    Archiving:

        The archive module simplifies compressing files or directories into various formats (e.g., .zip, .tar).

    Ownership:

        Set the correct ownership and permissions for security and usability.

3. Troubleshooting Tips

    Archive Not Created:

        Verify that the /var/log directory exists on the managed nodes:

            ansible all -m shell -a "ls /var/log"

    Ownership Not Applied:

        Check for errors in the playbook's file module task.

    Directory Missing:

        Ensure the /backups directory is created before archiving.

4. Summary

By completing this task, you’ve learned:

    How to use Ansible to archive logs for backup purposes.

    How to ensure proper ownership and permissions of backup files.

    How to verify and troubleshoot the results of an archive operation.
