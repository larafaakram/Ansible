Overall explanation

# Solution for Task 14: Backup and Restore

Task 14 involves creating a playbook to back up the /etc directory on all managed nodes and restore the backup to a new directory /restored_etc. This task demonstrates file management and recovery processes, essential for system administrators.

Objective

    Back up the /etc directory to /backups on all managed nodes.

    Restore the backup to a new directory /restored_etc.

### 1. Understanding Backup and Restore

    Backup: Creates an archive of the /etc directory for safekeeping.

    Restore: Extracts the archived backup to a specified directory.

### 2. Steps to Complete the Task

Step 1: Create the Playbook

    Open a text editor to create the playbook:

        vi backup_restore.yml

    Write the playbook:

        ---
        - name: Backup and Restore /etc Directory
          hosts: all
          tasks:
            - name: Ensure /backups directory exists
              file:
                path: /backups
                state: directory
                owner: root
                group: root
                mode: '0755'
         
            - name: Back up /etc directory to /backups
              archive:
                path: /etc
                dest: /backups/etc_backup_{{ ansible_date_time.date }}.tar.gz
                format: gz
                owner: root
                group: root
         
            - name: Ensure /restored_etc directory exists
              file:
                path: /restored_etc
                state: directory
                owner: root
                group: root
                mode: '0755'
         
            - name: Restore the backup to /restored_etc
              unarchive:
                src: /backups/etc_backup_{{ ansible_date_time.date }}.tar.gz
                dest: /restored_etc
                remote_src: yes

Explanation of the Playbook

    Task 1 (Create Backup Directory):

        Ensures the /backups directory exists on each managed node.

        Uses the file module to set the directory’s permissions and ownership.

    Task 2 (Backup /etc Directory):

        Uses the archive module to compress the /etc directory into a .tar.gz file.

        Names the backup file with the current date using ansible_date_time.date.

    Task 3 (Create Restore Directory):

        Ensures the /restored_etc directory exists for restoring the backup.

        Sets appropriate permissions and ownership.

    Task 4 (Restore Backup):

        Uses the unarchive module to extract the .tar.gz backup into the /restored_etc directory.

        remote_src: yes ensures the backup file is located on the managed node.

### Step 2: Execute the Playbook

    Run the playbook:

        ansible-playbook backup_restore.yml

    Expected Output:

        Task 1: Creates the /backups directory.

        Task 2: Archives the /etc directory into /backups.

        Task 3: Creates the /restored_etc directory.

        Task 4: Extracts the backup into /restored_etc.

3. Verify the Results

    Check Backup File:

        Ensure the backup file exists in /backups:

            ls /backups

    Check Restored Directory:

        Verify the contents of /restored_etc match /etc:

            ls /restored_etc

4. Key Concepts for Novice Learners

    Modules Used:

        file: Manages directories and file permissions.

        archive: Compresses files or directories.

        unarchive: Extracts compressed files.

    Dynamic Filenames:

        ansible_date_time.date ensures unique filenames by including the current date.

    Backup and Restore Workflow:

        Backup compresses data for storage efficiency.

        Restore extracts the data to its original or a new location.

5. Troubleshooting Tips

    Backup File Missing:

        Check the archive task for errors.

    Restore Fails:

        Ensure the .tar.gz file exists in /backups.

    Permission Issues:

        Verify the owner and mode settings in the playbook.

Summary

By completing this task, you’ve learned:

    How to automate backup and restore processes with Ansible.

    How to manage files and directories for efficient system administration.

    The importance of archiving and recovery in disaster management.

This task provides foundational skills for ensuring system data integrity and recovery in critical environments.
