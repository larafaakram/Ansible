Overall explanation

#Solution for Task 2: User Management

Task 2 focuses on creating and configuring a new user devops with specific requirements, such as a custom UID, a specified home directory, and inclusion in the sudo group. Here’s a step-by-step explanation designed for novice learners to complete this task successfully.

Objective

    Create a user devops with:

        UID: 1001

        Home Directory: /home/devops

    Add the user to the sudo group.

    Verify the creation of the user and group membership.

### 1. Create the User devops

The useradd command in Linux is used to add new users. Here, you’ll create the devops user with a custom UID and a specific home directory.

    Command to create the user:

        sudo useradd -u 1001 -d /home/devops -m devops

        -u 1001: Assigns UID 1001 to the user.

        -d /home/devops: Specifies the home directory for the user.

        -m: Ensures the home directory is created if it doesn’t exist.

    Set a password for the user:

        sudo passwd devops

        Enter a strong password when prompted.

    Explanation:

        A user’s UID (User ID) uniquely identifies them in the system.

        The home directory is where user-specific files and settings are stored. Specifying it ensures the user has a dedicated workspace.

### 2. Add the User to the sudo Group

The sudo group provides users with administrative privileges, allowing them to execute commands as the root user.

    Add devops to the sudo group:

        sudo usermod -aG sudo devops

        usermod: Modifies user properties.

        -aG sudo: Appends the user to the sudo group without removing them from existing groups.

    Explanation:

        Without administrative privileges, the devops user would be unable to perform critical tasks requiring elevated permissions.

        Adding the user to the sudo group ensures they can execute commands with sudo while maintaining system security.

### 3. Verify the User and Group Membership

It’s important to confirm that the devops user has been created with the correct attributes and is a member of the sudo group.

    Verify the user’s details:

        id devops

        This command displays the user’s UID, GID, and groups.

        Example output:

            uid=1001(devops) gid=1001(devops) groups=1001(devops),27(sudo)

    Verify the home directory:

        ls -ld /home/devops

        Ensure the home directory exists and has appropriate ownership.

        Example output:

            drwxr-xr-x 2 devops devops 4096 Jan 10 10:00 /home/devops

    Test sudo access:

        Switch to the devops user:

            su - devops

        Run a command with sudo:

            sudo ls /root

        If successful, the command will execute without errors.

Key Concepts for Novice Learners

    UID (User ID): A unique identifier for a user in the system. It’s essential to avoid conflicts with existing UIDs.

    Home Directory: A personal directory for each user, storing files, settings, and configurations.

    sudo Group: A special group granting administrative privileges to its members.

    id Command: Used to view user information, including UID, GID, and group memberships.

    ls -ld Command: Lists the directory details, including permissions and ownership.

Troubleshooting Tips

    User already exists:

        If the user devops exists, remove it before re-creating:

            sudo userdel -r devops

    Group membership not updated:

        Log out and back in as the devops user to refresh group memberships.

    Home directory not created:

        Use the -m flag during user creation or manually create it:

            sudo mkdir /home/devops
            sudo chown devops:devops /home/devops

Summary

By completing this task, you’ve:

    Created a user with a custom UID and home directory.

    Added the user to the sudo group for administrative access.

    Verified the user’s configuration and group membership.

This step ensures you understand user management basics, a critical skill for system administration.
