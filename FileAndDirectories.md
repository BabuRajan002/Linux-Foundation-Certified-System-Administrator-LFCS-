# SUID `chmod 2664 samplefile`

A special permission that allows users to run the executable with the permissions of the executable's owner

## Explanation

SUID (Set User ID) is a special type of file permission in Linux that allows users to run an executable with the permissions of the file owner rather than the permissions of the user who runs it. This can be particularly useful for tasks that require higher privileges than those of the user executing the command.

### How SUID Works
When the SUID bit is set on an executable file, the operating system grants the executing user the permissions of the file owner during the execution of that file. This is indicated by an "s" in the file's permission string.

For example, consider the following file permissions:
```
-rwsr-xr-x 1 root root 12345 Nov 26 12:34 /usr/bin/somecommand
```
Here, the "s" in the owner's execute position indicates that the SUID bit is set. When any user runs `/usr/bin/somecommand`, it will execute with the permissions of the file owner (in this case, `root`).

### Real Use Cases

1. **Password Management (`/usr/bin/passwd`)**:
   - The `passwd` command is used to change a user's password. Passwords are stored in `/etc/shadow`, which is only writable by the root user. To allow regular users to change their own passwords without giving them root access, the `passwd` command is set with the SUID bit.
   - Example:
     ```
     -rwsr-xr-x 1 root root 54256 Nov 26 12:34 /usr/bin/passwd
     ```

2. **Ping Command (`/bin/ping`)**:
   - The `ping` command requires raw socket access to send ICMP packets, which typically requires root privileges. By setting the SUID bit on the `ping` executable, regular users can use the command without needing root access.
   - Example:
     ```
     -rwsr-xr-x 1 root root 44168 Nov 26 12:34 /bin/ping
     ```

3. **Mounting Filesystems (`/bin/mount`)**:
   - The `mount` command is used to mount filesystems. Mounting typically requires root privileges, but by setting the SUID bit, users can mount filesystems as specified in `/etc/fstab` without needing root access.
   - Example:
     ```
     -rwsr-xr-x 1 root root 123456 Nov 26 12:34 /bin/mount
     ```

### Security Considerations
While SUID can be very useful, it also poses security risks if not managed properly. If a SUID program has vulnerabilities, it can be exploited to gain elevated privileges. Therefore, it is crucial to:

- **Limit SUID Usage**: Only set the SUID bit on programs that absolutely need it.
- **Regularly Audit SUID Files**: Periodically check for SUID files on the system to ensure no unauthorized files have the SUID bit set.
- **Keep Software Updated**: Ensure that all SUID programs are up-to-date to mitigate known vulnerabilities.

### Checking for SUID Files
To find all SUID files on a system, you can use the `find` command:
```sh
find / -perm /4000 -type f 2>/dev/null
```
This command searches for files with the SUID bit set (`-perm /4000`) and lists them.

In summary, SUID is a powerful feature in Linux that allows users to execute programs with elevated privileges. However, it must be used judiciously and managed carefully to maintain system security.

# SGID (`chmod 2664 samplefile`)

A similar permission, but applies to both executables and directories. 

SGID (Set Group ID) is another special type of file permission in Linux that affects both files and directories. When applied to files, it allows users to execute the file with the permissions of the file's group. When applied to directories, it ensures that files created within the directory inherit the group ownership of the directory, rather than the primary group of the user who created the file.

### How SGID Works

#### On Files
When the SGID bit is set on an executable file, the file runs with the permissions of the group owner of the file, rather than the group of the user who runs it. This is indicated by an "s" in the group execute position of the file's permission string.

For example:
```
-rwxr-sr-x 1 user group 12345 Nov 26 12:34 /usr/bin/somecommand
```
Here, the "s" in the group execute position indicates that the SGID bit is set. When any user runs `/usr/bin/somecommand`, it will execute with the permissions of the group owner (`group`).

#### On Directories
When the SGID bit is set on a directory, new files and subdirectories created within it inherit the group ownership of the directory, rather than the primary group of the user who created the file. This is indicated by an "s" in the group execute position of the directory's permission string.

For example:
```
drwxrwsr-x 2 user group 4096 Nov 26 12:34 /some/directory
```
Here, the "s" in the group execute position indicates that the SGID bit is set on the directory.

### Real Use Cases

1. **Shared Directories**:
   - In collaborative environments, such as a project directory shared by a group of users, setting the SGID bit on the directory ensures that all files created within the directory have the same group ownership. This facilitates easier file sharing and management.
   - Example:
     ```sh
     mkdir /shared/project
     chown user:group /shared/project
     chmod 2775 /shared/project
     ```
     Here, `chmod 2775` sets the SGID bit (2) along with read, write, and execute permissions for the owner and group, and read and execute permissions for others.

2. **System Binaries**:
   - Some system binaries may have the SGID bit set to allow users to execute them with the permissions of a specific group. For example, the `locate` command, which searches for files in a database, may have the SGID bit set to allow users to access the database file, which is typically owned by the `mlocate` group.
   - Example:
     ```
     -rwx--s--x 1 root mlocate 12345 Nov 26 12:34 /usr/bin/locate
     ```

### Security Considerations
Like SUID, SGID can pose security risks if not managed properly. If an SGID program has vulnerabilities, it can be exploited to gain elevated group privileges. Therefore, it is important to:

- **Limit SGID Usage**: Only set the SGID bit on programs and directories that absolutely need it.
- **Regularly Audit SGID Files**: Periodically check for SGID files on the system to ensure no unauthorized files have the SGID bit set.
- **Keep Software Updated**: Ensure that all SGID programs are up-to-date to mitigate known vulnerabilities.

### Checking for SGID Files
To find all SGID files on a system, you can use the `find` command:
```sh
find / -perm /2000 -type f 2>/dev/null
```
This command searches for files with the SGID bit set (`-perm /2000`) and lists them.

In summary, SGID is a useful feature in Linux for managing group permissions on files and directories. It facilitates collaborative work by ensuring consistent group ownership and can provide necessary group-level privileges for certain executables. However, it must be used judiciously and managed carefully to maintain system security.

The SUID and SGID bits primarily affect the execution of files and the creation of new files within directories, but they do not directly grant or restrict permissions to delete or modify existing files. Let's break down how permissions work in these contexts:

### SUID on Executable Files
- **Effect**: When a user executes a file with the SUID bit set, the process runs with the file owner's permissions.
- **Scope**: This elevated permission is only for the duration of the execution of the file. It does not grant the user additional permissions to modify or delete the file itself or other files owned by the file owner.
- **Example**: If `/usr/bin/passwd` has the SUID bit set, a user can change their password (which requires root privileges), but they cannot modify or delete `/usr/bin/passwd` itself unless they have the necessary file permissions.

### SGID on Executable Files
- **Effect**: When a user executes a file with the SGID bit set, the process runs with the group owner's permissions.
- **Scope**: Similar to SUID, this elevated permission is only for the duration of the execution of the file. It does not grant the user additional permissions to modify or delete the file itself or other files owned by the group.
- **Example**: If `/usr/bin/locate` has the SGID bit set to the `mlocate` group, a user can execute it with the permissions of the `mlocate` group, but they cannot modify or delete `/usr/bin/locate` itself unless they have the necessary file permissions.

### SGID on Directories
- **Effect**: When the SGID bit is set on a directory, new files and subdirectories created within it inherit the group ownership of the directory.
- **Scope**: This does not affect the permissions of existing files within the directory. Users can only modify or delete files if they have the appropriate permissions (write permission on the file and the directory).
- **Example**: If `/shared/project` has the SGID bit set, new files created in this directory will inherit the group ownership of `project`. However, users can only delete or modify files if they have write permissions on both the file and the directory.

### File and Directory Permissions
The ability to delete or modify files is governed by the standard Unix file permissions (read, write, execute) and the ownership of the files and directories:

- **File Permissions**: To modify a file, a user needs write permission on the file. To delete a file, a user needs write permission on the directory containing the file.
- **Directory Permissions**: To create, delete, or rename files within a directory, a user needs write and execute permissions on the directory.

### Example Scenario
Consider a shared directory `/shared/project` with the following permissions:
```
drwxrwsr-x 2 user project 4096 Nov 26 12:34 /shared/project
```
- **SGID Bit**: Ensures new files inherit the `project` group.
- **Permissions**: Users in the `project` group can read, write, and execute within the directory.

If a user in the `project` group creates a file:
```
-rw-r--r-- 1 user project 1234 Nov 26 12:34 /shared/project/newfile
```
- **Modify File**: The user can modify `newfile` because they have write permission on it.
- **Delete File**: The user can delete `newfile` if they have write permission on `/shared/project`.

### Conclusion
SUID and SGID provide specific elevated permissions for executing files and managing group ownership in directories, but they do not inherently grant users the ability to modify or delete files unless the standard file and directory permissions allow it. Properly setting and managing these permissions ensures that users have the necessary access without compromising security.

# Sticky Bit `chmod +t sampledir`

A Special permission that can be set on directories. It restricts file detection in that directory

