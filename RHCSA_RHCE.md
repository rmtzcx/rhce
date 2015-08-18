Summary
=======
I based the structure of this document on the book 'Red Hat RHCSA/RHCE 7 Cert
Guide: Red Hat Enterprise Linux 7 (EX200 and EX300)' by Sander van Vugt. If you
are not able to attend an official Red Hat training for RHCSA/RHCE, I really
recommend buying Sander van Vugt's book. It is by far the best book to prepare
yourself for the exams.

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
Xen was the official virtualization technology in Red Hat until RHEL 5.

Since RHEL 6 KVM became the official virtualization technology.

KVM support comes directly from the kernel, and it is the backend technology
used in all Red Hat virtualization and cloud solutions (RHEL, RHEV, Red Hat
OpenStack, etc.)

KVM requirements are simple:

  * 64-bit CPU (arch)
  * Hardware virtualization (vmx, svm in /proc/cpuinfo))

Two important modules should be loaded:

    * kvm
    * kvm_intel or kvm_amd

The easiest way to install virtualization support and related tools is

    yum groupinstall "Virtualization Host"
    
**libvirtd** is an interface to interact and manage virtual machines.

There are several tools that use the libvirt API:

  * virt-manager.
  * virt-viewer.
  * virsh.

This is how I created the VMs for this training:

    for i in {11..18} ; do
        virt-install \
            --name=server$i \
            --ram=2048 \
            --vcpus=2 \
            --location=http://rmtzcx01.rmtzcx.mx/isos/autofs/centos-7.1-x86_64 \
            --extra="ip=192.168.7.$i netmask=255.255.255.0 dns=192.168.7.1 gateway=192.168.7.1 noipv6 ksdevice=eth0 inst.ks=http://rmtzcx01.rmtzcx.mx/~rmtzcx/ks/el7.cfg inst.repo=http://rmtzcx01.rmtzcx.mx/isos/autofs/centos-7.1-x86_64 hdlayout=server-small hd1=vda kvmguest inst.keymap=us ntpserver=rmtzcx01.rmtzcx.mx fqdn=server$i.rmtzcx.mx selinux=disabled" \
            --os-type=linux \
            --os-variant=rhel7 \
            --disk=size=15 \
            --network=network=ovsbr1,portgroup=frontend-untagged-vlan192 \
            --graphics=type=spice \
            --hvm \
            --virt-type=kvm &
    done

XML configuration files are stored by default at:

    /etc/libvirt/qemu

Disk images are stored by default at:

    /var/lib/libvirt/images

To dump the XML file:

    virsh dumpxml server11

To edit:

    virsh edit server11

To list all running VMs:

    virsh list

To list all VMs

    virsh list --all

To connect to the serial console:

    virsh console server11

(Press Ctrl + ] to exit)

**Note**: Serial console access requires specific configuration within the VM.

To destroy a VM:

    virsh destroy server11

To start a VM:

    virsh start server11


### Virtualization and Netwoking ###
The virtual host server provides physical access to the network. The VMs access
the network through a 'bridge', usually in one of the following ways:

  1. NAT'ed.
  2. Bridged.

In NAT'ed mode, the virtual host server acts as a gateway for the VMs.

eth0 in the server is **not** part of the bridge, the server has 2 IPs, one in
eth0, and other in the bridge. The VM has an IP in the Bridge segment.

    Network <==> eth0 <---> bridge <==> vnet <==> eth0
               (Server)                           (VM)

In bridged mode, the VMs have direct access to the physical network.

eth0 in the server is part of the bridge, the server has an IP from the Network
segment in the bridge. The VM has an IP in the Network segment too.

    Network <==> eth0 <==> bridge <==> vnet <==> eth0
               (Server)                          (VM)


There are 2 popular bridging tools:

  1. bridge-utils
  2. openvswitch

bridge-utils provides basic bridging support. The main command is brctl.

Open vSwitch provides extended and complete support for bridging, VLANs, etc.
The main command is ovs-vsctl.


Chapter 11. Managing Software
-----------------------------
yum uses repositories to install software. The repositories are defined in
files with .repo extension and are usually located at /etc/yum.repos.d.

Them main configuration file is /etc/yum.conf

A repo configuration file can contain several repositories and each repository
requires the following fields:

  1. [label]
  2. name=
  3. baseurl=

Other common options are:

  1. mirrorlist=
  2. gpgcheck=
  3. gpgkey=

**Question**: Why is importan gpg keys in yum repositories?

Keys that were used for package signing are installed to the /etc/pki/rpm-gpg
directory by default.


### How to create a YUM repo ###

  1. Install createrepo.
  2. Copy the RPMs to the target directory.
  3. Run create /path/to/dir
  4. Configure the .repo file to use it.


### YUM ###
Common YUM commands:

  * yum search
  * yum provides
  * yum info
  * yum list
  * yum install
  * yum remove
  * yum group list
  * yum group install
  * yum update
  * yum clean all
  * yum history


### RPM ###
####  Installation options ####

  * rpm -i
  * rpm -U
  * rpm -F
  * rpm -e

#### Query the database ####

  * rpm -q
  * rpm -qa
  * rpm -qi
  * rpm -ql
  * rpm -qd
  * rpm -qc
  * rpm -qf
  * rpm -q --scripts

#### Query packages ####
  * rpm -qp

#### Package verification ####
  * rpm -V
  * rpm -Va
  * rpm -q gpg-pubkey


Chapter 12. Scheduling Tasks
----------------------------
### Cron ###
crond wakes up every minute to see if there is anything that you be run.

Verify the status of crond

    systemctl status crond -l

Regular cron files have 6 fields:

  1. Minute
  2. Hour
  3. Day of month
  4. Month
  5. Day of week
  6. Task

Exercises:

  * Any minute between 11:00 and 11:59
  * Every day at 11 a.m. on weekdays only
  * Every hour on weekdays on the hour
  * Every 2 hours on the hour on December second and every Friday in December

Global cron file is /etc/crontab, try not to use it. Better use:

  * Cron files in /etc/cron.d
  * Scripts in /etc/cron.hourly, cron.dialy, cron.weekly, and cron.monthly
  * User-specific files that are created with crontab -e
  * /etc/cron.hourly
  * /etc/cron.daily
  * /etc/cron.weekly
  * /etc/cron.monthly


#### Anacron ####

Cron run anacron every hour (/etc/cron.hourly).

Anacron runs the job in /etc/cron.{daily,weekly,monthly)}.

Anacron configuration file has 4 fields:

  1. Period in days
  2. Delay in minutes
  3. Job ID
  4. Command


#### Cron security ####
If the {anacron,at,cron}.allow file exists, a user must be listed in it to be
allowed to use the anacron/at/cron. If the {anacron,at,cron}.allow file does
not exist but the {anacron,at,cron}.deny file does exist, then a user must not
be listed in the {anacron,at,cron}.deny file in order to use cron. If neither
of these files exists, only the super user is allowed to use {anacron,at,cron}.


### At ###
Sometimes you may need to run a job just once, rather than regularly. For this
you use the at command. (at, atq, atrm)

Verify the status of atd

    systemctl status atd -l

Some examples:

    at -f mycrontest.sh 10:25
    at -f mycrontest.sh 10pm tomorrow
    at -f mycrontest.sh 2:00 tuesday
    at -f mycrontest.sh 2:00 july 11
    at -f mycrontest.sh 2:00 next week

/etc/at.{allow,deny} files works as cron ones.


Chapter 13. Configuring Logging
-------------------------------
A log service collects information from services and events in a system. The 2
main log systems are rsyslog and journald.


### rsyslog ###
rsyslog stores the log in text forma in /var/log.


A typical way to monitor rsyslog file is:

    tail -f /var/log/<FILE_NAME>

Common rsyslog files are:

  * /var/log/messages
  * /var/log/dmesg
  * /var/log/secure
  * /var/log/audit/audit.log
  * ...

Messages are categorized by priorities and facilities. Read SELECTORS section
on man 5 rsyslog.conf.

The logger command can be used by users to send messages to rsyslog. Examples:

    logger "Hello World"
    logger -p authpriv.debug "Goodbye World"

**Question**: Where did each message go?

The main configuration file for rsyslog is /etc/rsyslog.conf and it is
organized in sections:

    * Modules
    * Global directives
    * Rules


#### Log rotation ####
What would happen if log were never rotated?

There is a log rotation tool called logrotate which is run daily by cron.

logrotate's main configuration file is /etc/logrotate.conf.

Programs can add logrotate configurations to /etc/logrotate.d.


### journald ###
journald stores the logs in binary format in /run/log/journal. To make the
journal persistent:

    mkdir /var/log/journal
    chown root:systemd-journal /var/log/journal
    chmod 2755 /var/log/journal
    pkill -USR1 systemd-jounrnald

journalctl can be used to see the logs:

  * systemctl status <UNIT>
  * journalctl
  * journalctl -f
  * journalctl -b
  * journalctl -p err
  * journalctl --since yesterday (or YYY-MM-DD)
  * journalctl <TAB> <TAB> (if bash-completion is installed)
  * journalctl _SYSTEMD_UNIT=sshd.service --no-pager


The main configuration file is /etc/systemd/journald.conf


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
