Summary
=======
I based the structure of this document on the book 'Red Hat RHCSA/RHCE 7 Cert
Guide: Red Hat Enterprise Linux 7 (EX200 and EX300)' by Sander van Vugt.

This is not a tutorial, it is just a guide for me to teach RHCE System
Administration and help others to prepare for RHCSA/RHCSA exam.


Requirements
============
  * 1 virtual host server with 4GB RAM (RHEL or CentOS 7 recommended).
  * 2 virtual machines with RHEL or CentOS 7 (server1, client1).
  * Isolated network.

[CentOS download page][]

[CentOS download page]:http://www.centos.org/download/


Part 1: RHCSA
=============
Chapter 1. Installing Red Hat Enterprise Linux Server
-----------------------------------------------------
Red Hat Enterprise Linux has variants and add-ons. A variant is a full OS
installation media. An add-on is a set of packages (RPMs) usually available
from a repository.

Some Red Hat variants are:

  * Red Hat Enterprise Linux Server for High-Performance Computing (HPC).
  * Red Hat Enterprise Linux Server for IBM Power.
  * Red Hat Enterprise Linux Server for IBM System z.
  * Red Hat Enterprise Linux for SAP Business Applications.
  * Red Hat Enterprise Linux for SAP HANA.

Some Red Hat add-ons are:

  * High Availability.
  * Resilient Storage.
  * Smart management.
  * Extended support.

The book recommends installating CentOS with the option 'Server with GUI'. I
will use the 'Minimal installation' option and I will install the additional
packages as required.


### Practice ###
Install CentOS with default options.


Chapter 2. Using Essential Tools
--------------------------------
### Basic Shell Skills ###
#### Executing Commands ####
The shell makes a difference between 4 kinds of commands, in this order:

  * Aliases
  * Functions
  * Built-in commands
  * External commands

**Note:** The book says 3, but I consider 4.

To identify the type, use the 'type' command. Example:

    [rmtzcx@rmtzcx01 ~]$ type ls
    ls is aliased to `ls --color=auto'

    [rmtzcx@rmtzcx01 ~]$ type help
    help is a shell builtin

    [rmtzcx@rmtzcx01 ~]$ type date
    date is /usr/bin/date


#### I/O Redirection and pipes ####
There are 3 standard streams:

    +--------+-----------------+----+
    | Name   | Description     | FD |
    +--------+-----------------+----+
    | stdin  | Standard input  |  0 |
    | stdout | Standard output |  1 |
    | stderr | Standard error  |  2 |
    +--------+-----------------+----+

There are 2 special streams for network interaction:

    /dev/tcp/HOST/PORT
    /dev/udp/HOST/PORT

There are several redirectors:

    +------------+-------------------------------------------------+
    | Redirector | Description                                     |
    +------------+-------------------------------------------------+
    | [n]>       | Redirect stdout, n is 1 by default              |
    | [n]>>      | Append redirection of stdout. n is 1 by default |
    | [n]<       | Redirect stdin, n is 0 by default               |
    | <<[-]      | Here document, read from stdin until delimiter  |
    | <<<        | Here string, read from stdin                    |
    | 2>&1       | Redirect stderr to stdout                       |
    | &>         | Redirect stderr and stdout to the same stream   |
    | &>>        | Redirect stderr and stdout to the same stream   |
    | |          | Redirect stdout to stdin                        |
    +------------+-------------------------------------------------+

__Homework__: Investigate how to download a file (HTTP) with bash.


#### History ####
How to interact with history:

  * 'history' command.
  * 'up' key.
  * Ctrl + R for backwards searches.
  * !NUMBER
  * !STRING

There some useful environment variables (with my current settings):

  * HISTCONTROL=ignoreboth
  * HISTSIZE=10000
  * HISTTIMEFORMAT='%F %T '

The history command has interesting options:

  * -c  Clear
  * -d  Delete
  * -a  Append history to historu file
  * -n  Read new lines from history file
  * -w  Write to history file


### Vim ###
This is the best vim cheet sheet I know:
[http://vim.rtorr.com](http://vim.rtorr.com/)


### Shell environment ###
A shell is an interactive interface to the system. It could be a text or a
graphical interface.


#### Variables ####
There are 2 types of variables:

  1. Variables with special meaning for bash.
  2. Variables defined by the user without any meaning for the shell.

To see the current shell variables type '**set**'.

__Homework__: Undestand the difference between '**set**' and '**env**'.

To set a variable just type the name of the variable followed by the value.

    NAME="Rodolfo"

    echo "$NAME"

The assigment can be prefixed with '**declare**' to set variable attributes.

To export a variable prefix the assigment with '**export**':

    export NAME="Rodolfo"

To unset a variable use the '**unset**' command:

    unset NAME


#### Configuration files ####
When bash is running in a interactive session, there are 2 modes:

  1. A login shell
     The scripts running during startup and shutdown sequence for a login
     shell are as follows:

        /etc/profile
        ~/.bash_profile or ~/.bash_login or ~/.profile
        ~/.bashrc
        /etc/bashrc
        [ Interactive session ]
        ~/.bash_logout

  2. A bash session:

        ~/.bashrc
        /etc/bashrc


#### Banners ####
Content of /etc/issue _should_ be displayed before the user logs in.

Content of /etc/motd is displayed after the user logs in.


### Help ###
#### --help ####
Most of the commands have a --help or -h option.


#### help ####
'**help**' is a bash built-in command that displays the help text from
built-in commands.


#### man ####
Man pages are organized in sections:

  * Section 1 User Commands
  * Section 2 System Calls
  * Section 3 Subroutines
  * Section 3p Perl Modules
  * Section 4 Devices
  * Section 5 File Formats
  * Section 6 Games
  * Section 7 Miscellaneous
  * Section 8 System Administration
  * Section 9 Kernel routines
  * Section n New

The '-k' option for the 'man' command is very useful to find out the correct
man page. It queries the the summary. Equivalent to 'apropos'.

    man -k date
    apropos date

To see all man files for 'date':
    
    man -wa date

To see a brief description (equivalent to 'whatis'):
    
    man -f

To update the manual page index caches:

    mandb


#### info ####
[http://www.gnu.org/software/texinfo/manual/info/info.html](http://www.gnu.org/software/texinfo/manual/info/info.html)

    info info


#### /usr/share/doc ####
Programs usually store documentation in this path.


Chapter 3. Essential File Management Tools
------------------------------------------
### Working with the File System Hierarchy ###
#### Defining the File System Hierarchy ####
The FSH is described in man 7 hier.

Most relevant directories are:

  * /
  * /bin
  * /boot
  * /dev
  * /etc
  * /home
  * /lib, /lib64
  * /opt
  * /proc
  * /root
  * /run
  * /sbin
  * /sys
  * /tmp
  * /usr
  * /var


#### Understanding Mounts ####
/ is the root of the file system. Any other mount point is a subdirectory of
the root (/).

A mount point is a directory where a file system is 'presented' to the system,
such as, partitions, logical volumes, or network shares.

Some common mount points are:

  * /boot
  * /home
  * /tmp
  * /var

The 'mount' command can give an overview of the mounted file systems, mount a
specific file system or even mount all file systems defined in /etc/fstab.

'findmnt' lists all mounted filesytems or search for a file system. It is also
able to search in /etc/fstab, /etc/mtab or /proc/self/mountinfo.

The 'df -T' command can be useful to display the mounted file systems.


### Managing Files ###
#### Globbing ####
Globbing is a built-in feature in the shell.

The wildcards are : '?', '*', '[]'.

Globbing is not a regular expression.

#### Managing and Working with Directories ####
Directory commands:

  * mkdir
  * cd
  * rmdir
  * pushd
  * popd


#### Absolute and Relative Pathnames ####
All absolute path names start with '/'.

Why sometimes we add ./ to execute a command?


#### Listing Files and Directories ####
Common 'ls' options:

  * ls -l
  * ls -a
  * ls -1
  * ls -lrt
  * ls -d

**Trivia**: How to list files without 'ls'?


#### File operations ####

  * Copy (cp).
  * Move (mv).
  * Rename (mv, rename).
  * Delete (rm, rmdir).

**Trivia**: How to copy a file witout using 'cp', 'rsync', 'scp'?


### Using Links ###
There are 2 types of links:

  * Hard links.
  * Soft links.


#### Hard Links ####
An inode is an structure that stores information about a file. Some of the
information is:

  * Ownership (user and group).
  * Permissions.
  * Access, change, and modification times.
  * Hard link counter.
  * Size.
  * Blocks.
  
The stat system call retrieves a file's inode number and some of the
information in the inode:

    $ stat /etc/passwd
      File: ‘/etc/passwd’
      Size: 2453          Blocks: 8          IO Block: 4096   regular file
    Device: fd01h/64769d    Inode: 397987      Links: 1
    Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
    Context: system_u:object_r:passwd_file_t:s0
    Access: 2015-07-17 23:17:05.283205821 -0500
    Modify: 2015-07-17 23:17:05.283205821 -0500
    Change: 2015-07-17 23:17:05.316206175 -0500

A file is removed from the inode table when the link counter is 0.

inodes numbers are shown with ls -i.

Hard links are created with:

    ln

**Note**: The file name is not stored in the inode structure.


#### Soft Links ####
Soft links are 'pointers' to a file name. It has its own inode.

Soft links are created with:

    ln -s

Be very careful when removing soft links pointing to a directory.


### Working with Archives and Compressed Files ###
tar doesn't compress, it just group or ungroup files.

Common options:

  * -c: Create a new archive.
  * -x: Extract files from an archive.
  * -v: List files processed.
  * -f: Specify source or destination.
  * -C: Change to directory.
  * -t: List the contents of an archive.
  * -r: Append files to the end of an archive.
  * -z: Filter the archive through gzip/gunzip.
  * -j: Filter the archive through bzip2/bunzip2.
  * -J: Filter the archive through xz/unxz.

Archive and (un)compress in 2 steps:

    tar -cf /tmp/etc.tar /etc
    gzip /tmp/etc.tar

    gunzip /tmp/etc.tar.gz
    tar -xf /tmp/etc.tar

Archive and (un)compress in 1 step:

    tar -zcf /tmp/etc.tar.gz /etc

    tar -zxf /tmp/etc.tar.gz


Chapter 4. Working with Text Files
----------------------------------
### Text file tools ###

    * less
    * cat / tac
    * more
    * head
    * tail
    * cut
    * sort
    * wc
    * comm
    * join
    * paste
    * expand
    * unexpand
    * fmt
    * grep
    * sed
    * awk

**Exercise**: Show lines 10 to 20 in a 30-line file.

**Exercise**: Sort a list of IP addresses.

**Exercise**: Do not show blank lines and comments from a file.

**Exercise**: Create a self extract bash shell. Hint:

    sed -e '0,/^#__SFX__#$/d'


Chapter 5. Connecting to Red Hat Enterprise Linux 7
---------------------------------------------------
### Local Consoles ###
#### Virtual terminals ####
Also known as virtual consoles

Alt + F1 .. F6

chvt


#### Pseudo Terminals Devices ####
Created when opening a terminal in a graphical environment or remote sessions.
Pseudo terminal are in /dev/pts

You can know the current terminal with the command 'tty'.
ls /proc/self/fd


#### Reboot and Shutting down a server ####

    init 0
    poweroff
    halt
    systemctl poweroff
    systemctl halt
    shutdown -h now

    init 6
    reboot
    systemctl reboot
    shutdown -r now
    echo b > /proc/sysrq-trigger


### SSH and Friends ###

  * ssh

        ssh -p 2222
        ssh -X
        ssh -L 8080:localhost:8080
        ssh -R 8080:localhost:8080

        ssh-keygen
        ssh-copy-id
        ssh-agent
        ssh-add

  * PuTTY
  * scp
  * rsync

**Question**: What is the SSH server fingerprint?

**Question**: What is man in the middle attack?


#### Terminal Multiplexers ####

    tmux 

    screen


Chapter 6. User and Group Management
------------------------------------
### User Types ###

  * Privileged.
  * Unprivileged.


Commands:

  * id
  * whoami
  * su
  * sudo
  * visudo


### Accounts ###

  * User accounts
  * System accounts

User configuration files:

  * /etc/passwd (man 5 passwd)
  * /etc/shadow (man 5 shadow)

#### Managing Users ####

  * useradd
  * userdel
  * usermod
  * passwd
  * vipw
  * vipw -s

Configuration files:

  * /etc/default/useradd
  * /etc/login.defs
  * /etc/skel
  * chage command
  * passwd command


### Group Accounts ###

  * Primary groups
  * Supplementary groups


#### Managing Groups ####

  * groupadd
  * groupdel
  * groupmod
  * gpasswd
  * vigr
  * vigr -s

Configuration files:

  * /etc/group
  * /etc/gshadow


Chapter 7. Configuring Permissions
----------------------------------
### File and directories ownership ###
There are 4 types of basic permissions:

  1. User (owner)
  2. Group
  3. Others
  4. Special bits (suid, guid, sticky)

Read, write, and execute permissions:

    +------------+----------+--------------+-------------------------+
    | Permission  | Numeric | Files        | Directories             |
    +-------------+---------+--------------+-------------------------+
    | Read        |    4    | Open         | List                    |
    | Write       |    2    | Modify       | File operations         |
    | Execute     |    1    | Run          | Enter                   |
    | SUID        |    4    | Run as owner | N/A                     |
    | SGID        |    2    | Run as group | Inherit group ownership |
    | Sticky      |    1    | N/A          | Delete own files        |
    +-------------+---------+--------------+-------------------------+

Example:

    [rmtzcx@rmtzcx01 ~]$ ls -ld /tmp/
    drwxrwxrwt. 10 root root 240 Jul 30 21:53 /tmp/

Commands:

  * chown
  * chgrp
  * chmod
  * groups
  * newgrp
  * umask


### Access Control Lists ###
ACL is a second level permissions. They are supported from the filesystem.

See man 5 acl

    # tune2fs -l /dev/sda1 |grep -i acl
    Default mount options:    user_xattr acl

Examples:

    # Add read and write permissions to /etc/passwd
    setfacl -m u:rmtzcx:rw- /etc/passwd
    setfacl -m g:rmtzcx:rw-,o:rw- /etc/passwd

    # Show the current ACL
    getfacl /etc/passwd

    # Set effective permission with mask
    setfacl -m m:r-- /etc/passwd

    # Remove the permissions
    setfacl -x u:rmtzcx /etc/passwd
    setfacl -b /etc/passwd


### Extended Attributes ###

  * lsattr
  * chattr


Chapter 8. Configuring Networking
---------------------------------
### Networking Fundamentals ###

  * IPv4
    - 32-bit
    - 10.0.0.0/8 - Class A
    - 172.16.0.0/12 - Class B
    - 192.168.0.0/16 - Class C
    - Network mask
    - Gateway
    - Binary notation
    - NAT - SNAT/DNAT

  * IPv6
    - 128-bit

  * Protocols and ports
    - /etc/services
    - ftp: 21
    - ssh: 22
    - http: 80
    - https: 443

  * MAC Addresses
    - 48-bit


### Network Addresses and Interfaces ###
Network addresses are assigned to network cards in 2 ways:

  * Fixed IP.
  * Dynamically IP (DHCP).

In RHEL7 the network card name nomenclature changed to 'Consistent Network
Device Namning'.

[See RHEL 7 documentation](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/ch-Consistent_Network_Device_Naming.html)


#### Network tools ####
Check the network configuration:

  * ip address show  (ifconfig -a)
  * ip route show  (route)
  * ip link show
  * ss  (netstat)

**Note**: Old network tools are in the net-tools package.


#### Network Manager ####
Network configuration is handled by the NetworkManager service.

There are several ways to interact with the NetworkManager service:

  * nmcli (Text tool)
  * nmtui (Interactive text tool)
  * nm-connection-editor (Graphical tool)

In the NetworkManager context:

  * A device is a network interface card.
  * A connection is the configuration used on a device.

You can create multiple connections for a device.

Examples:

    nmcli connection show
    nmcli connection show eth0
    
    nmcli devices show
    nmcli devices show eth0

    nmcli connection add --help
    nmcli connection add con-name "eth0-static" ifname eth0 autoconnect no type ethernet ip4 172.16.7.4/24
    nmcli connection modify "eth0-static" connection.autoconnect no
    nmcli connection modify "eth0-static" ipv4.dns 4.4.4.2
    nmcli connection modify "eth0-static" +ipv4.dns 8.8.8.8
    nmcli connection modify "eth0-static" +ipv4.addresses 172.16.7.44/24
    nmcli connection show "eth0-static"
    nmcli connection up eth0-static 


**Note**: See man nmcli-examples for more examples

All configuration files are kept in /etc/sysconfig/network-scripts 


#### Hostname ####

There are different ways to change the hostname:

    * Use nmtui and select the option Change Hostname.
    * Use hostnamectl set-hostname.
    * Edit the contents of the configuration file /etc/hostname.


and verify with:

    * hostname
    * hostnamectl status


Chapter 9. Managing Processes
-----------------------------
### Shell Jobs ###

  * &
  * Ctrl + c
  * Ctrl + z
  * Ctrl + d
  * fg
  * bg
  * jobs

### Processes ###

  * Processes vs Threads
  * [Kernel threads]
  * ps aux / ps -ef
  * ps fax
  * pstree
  * pgrep
  * nice
  * renice

**Question**: What is a signal?

  * kill
  * killall
  * pkill
  * top

Chapter 10. Working with Virtual Machines
-----------------------------------------

Chapter 11. Managing Software
-----------------------------

Chapter 12. Scheduling Tasks
----------------------------

Chapter 13. Configuring Logging
-------------------------------

Chapter 14. Managing Partitions
-------------------------------

Chapter 15. Managing LVM Logical Volumes
----------------------------------------

Chapter 16. Basic Kernel Management
-----------------------------------

Chapter 17. Configuring a Basic Apache Server
---------------------------------------------

Chapter 18. Managing and Understanding the Boot Procedure
----------------------------------------------------------

Chapter 19. Troubleshooting the Boot Procedure
----------------------------------------------

Chapter 20. Using Kickstart
---------------------------

Chapter 21. Managing SELinux
----------------------------

Chapter 22. Configuring a Firewall
----------------------------------

Chapter 23. Configuring Remote Mounts and FTP
---------------------------------------------

Chapter 24. Configuring Time Services
-------------------------------------


Part 2: RHCE
============

Chapter 25. Configuring External Authentication and Authorization
-----------------------------------------------------------------

Chapter 26. Configuring an iSCSI SAN
------------------------------------

Chapter 27. System Performance Reporting
----------------------------------------

Chapter 28. System Optimization Basics
--------------------------------------

Chapter 29. Configuring Advanced Log Features
---------------------------------------------

Chapter 30. Configuring Routing and Advanced Networking
-------------------------------------------------------

Chapter 31. An Introduction to Bash Shell Scripting
---------------------------------------------------

Chapter 32. Advanced Firewall Configuration
-------------------------------------------

Chapter 33. Managing Advanced Apache Services
---------------------------------------------

Chapter 34. Configuring DNS
---------------------------

Chapter 35. Configuring a MariaDB Database
------------------------------------------

Chapter 36. Configuring NFS
---------------------------

Chapter 37. Configuring Samba File Services
-------------------------------------------

Chapter 38. Setting Up an SMTP Server
-------------------------------------

Chapter 39. Configuring SSH
---------------------------

Chapter 40. Managing Time Synchronization
------------------------------------------
