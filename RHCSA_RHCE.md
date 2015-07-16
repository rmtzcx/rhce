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
man page. It queries the in the summary. Equivalent to 'apropos'.

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


Chapter 3. Essential File Management Tools
------------------------------------------

Chapter 4. Working with Text Files
----------------------------------

Chapter 5. Connecting to Red Hat Enterprise Linux 7
---------------------------------------------------

Chapter 6. User and Group Management
------------------------------------

Chapter 7. Configuring Permissions
----------------------------------

Chapter 8. Configuring Networking
---------------------------------

Chapter 9. Managing Processes
-----------------------------

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
