## 2 - Operating Systems & Linux Basics

### Virtualization & Virtual Machines

You need a hypervisor to run multiple virtual machines using a physical Host OS
Most popular hypervisor is VirtualBox (Used this in the past but sinds I switched to M1 I needed to use UTM because
VirtualBox does not support M1 )
VMs are isolated Type 2 vs Type 1 Hypervisor: Type 1 installed directly on hardware for servers, Type 2 uses host OS on
personal computer

- Benefits Type 2: learn & experiment, don’t endanger main OS, test app on different OS
- Benefits Type 1: efficient usage of hardware resources, abstraction of OS from hardware via VMI (Virtual Machine
  Image) with backups/snaphots

### Linux File System

Everything in Linux is a file(In MAC / Windows it is not the case)
A root user has its own home directory. This directory can be different in different oses (In MAC it is /Users/username)

#### Linux folder structure

* /home/{Username} dir of non-root users (If the user is created with a home directory)
* /bin executables for essential system cmd
* /sbin system binaries, need super user privileges to execute
* /lib shared lib that execs from /bin or /sbin use
* /usr was used for user home dir (histroic reasons due to storage limitations)
* /usr/local programs that YOU install on computer (3rd party apps) available for all users
* /opt 3rd party programs you install that DONT split its components. Ex. IDE, web browsers.
* /boot files required for booting
* /etc system config
* /dev device files (webcam, mouse, keyboard etc)
* /var stores logs
* /var/cache
* /tmp temporary resources required for processes
* /media removable media. Ex. USB
* /mnt temporary mount points

Hidden files (starts with a dot) -  called dotfiles. Prevents important data to accidentally be deleted.

### CLI
nana@nana-ubuntu:~$
  - nana - username
  - nana-ubuntu - computer name
  - ~ - home directory indicator
  - $ - represent regular user. Not a superuser. Root user will have a #(pound) sign

### Commands

* pwd = show current dir. Stands for print working directory.
* ls = list contents
* cd = change dir (cd / = change to root dir and empty cd is go to home)
* mkdir = make dir
* touch = create file
* rm = delete file
* rm -r = delete non-empty dir with files in it. -r stands for recursive
* rmdir = delete empty dir
* clear = clears terminal
* mv <old-name> <new-name> = rename file to new name
* cp -r <dir> <new-dir> = copy contents folder to new folder
* ls -R = list all folders and files in each
* ctrl + r = Search history. Search the commands you executed before rather than using up down arrows
* ctrl + c = Stop currently executing command
* history = lists all recent cmd in terminal
* history <number> = ex. history 20, displays last 20 executed commands
* ctlr + shift + v = paste copied text to terminal
* ls -a = display hidden files
* ls -l = print files in long list format (ls -la for listing hidden files)
* cat = show contents file. Stands for concatenate
* uname -a = show system & kernel
* cat /etc/os-release = show release version
* lscpu = cpu info
* lsmem = memory info
* sudo = grants super user privileges for cmd
* su - <username> = become/login as user
* | = pipe, passes output of one cmd as input of next cmd
* <input> | less = displays reader friendly format for info in CLI
* <input> | grep <pattern> = filter input based on pattern search
*  > = redirect, takes output from previous cmd and sends it to file that you give (overrides contents file)
* > > = appends text to end of file
* > > Can pass multiple cmd in one line separated by ;
* .bash_history = where all commands are stored

### Package manager: APT

Resolves dependencies for installing software
Ensure integrity & authenticity of package
Downloads, installs or updates existing software from repo
Knows where to put all files in system

```apt search <package name>```
  - to search a specific package. You  could also type a command like `java`, it will show not found but it will show suggestion on how to install t using apt
```apt install <package name>```
```apt remove <package name>```
```apt update```
  - will update the package index which is actually a database. Pulls the latest changes from APT repo. This is executed in freshly installed OS.
  - /etc/atp/sources.list -  contains the list of repo link which uses by apt to get the packages to install
```apt upgrade```
```apt autoremove```
```apt full-upgrade```

Alternative ways to install a package if not available in package manager
  - reason why it won't be available is sometimes it takes time for these package updates to get verified to be incldued in the repo.
  1. Alternative: Ubuntu Software Center - app name: Ubuntu Software
  2. Alternative: snap package manager - app name: Snappy. Terminal command `snap`
     Snap vs APT:
       Snap:
         - install packages as self contained package including dependencies
         - supports universal linux package `.snap`
         - automatic updates
         - larger installation size. If you install 50 packages with the same dependencies, it will also install the same dependencies 50x since snap is just a single package.
       Apt:
         - install package in separate folders which is recommended way. So if it is available in `apt`, better to use this one instead.
         - only supports specific linux distribution `.dep`
         - manual updates
         - smaller installation size
         - dependencies are shared across installation
  3. Alternative: Add repository to official list of repos
     - command: `add-apt-repository`
     - adds repository to `/etc/atp/sources.list`
     - PPA "Personal Package Archive" - personal repos provided by community. Risky and wont ensure quality and security. Might create difficulty when upgrading ubuntu. Needs to do a background check first who is maintaining the software to ensure trustworthiness.

### VIM text editor

* :wq = quit and save
* :q! = quit without saving
* dd = delete line
* d10d = delete next 10 lines
* u = undo
* A = jump to end of line, switch insert mode
* $ = jump to end line, without switch insert mode
* 0 = jump to beginning of line
* 12G = jump to line 12
* /pattern = search for pattern
* n = jump to next match
* N = jump to previous match
* :%s/old/new = replace old with new throughout file

### Users, Groups & Permissions

3 user categories:

* Root user - unrestricted user
* Regular user - regular user we create a login. Will have own directory /home/dianne
* Service user - (best practice for security) (no login shell) These users can be used for apache or mysql service as an example. 1 service user to run per service for security. Don't run these services using a root user!

Can group users and define group permissions
Users can have multiple groups

/etc/passwd - stores user account infox
  - ex. nana:x:1000:1000:Nana,,,:/home/nana:/bin/bash
  -     [USERNAME:PASSWORD:UID:GID:GECOS:HOMEDIR:SHELL]
  -      - UID - 0 is reserved for root. Generated by system
  -      - GID - primary group ID specific to user. Stored in /etc/group. System auto assigns a new group ID everytime we create a new user. it generates the next available GID from the range of group ids defined in login.defs
  -      - GECOS - user description inputted in prompt. Phone number, home number etc

#### Commands

* adduser <username>
* passwd <username>
* su <username> -- stands for switch user
* su - -- switch root user
* groupadd <groupname> -- to verify if it was created, command `cat /etc/group`
* deluser <username>
* groupdel >groupname>
* usermod [OPTIONS] <username>
* usermod -g <groupname> <username>
* usermod -G <groupname> <username> (overrides secondary groups list)
* usermod -aG <groupname> <username> (appends to existing list)
* gpasswd -d <username> <groupname>
* groups <username> (lists users groups)
* exit (logout user)
* chown <username>:<groupname> <filename> (change file ownership)
* chgrp <groupname> <filename>

adduser, addgroup, deluser, delgroup etc. vs useradd, groupadd, userdel, groupdel etc
  adduser, addgroup etc
    - Interactive and more user friendly. Asks inputs like in passwords
    - auto assigned UID and GID
    - auto creates home directory with skeletal config
    - advisable when manually creating a user
  useradd, groupadd etc
    - low-level utilities. needed informations are provided as a parameter
    - advisable when creating a user via SH (Shell script) automatically

#### Permissions

(owner)

* r = read
* w = write
* x = execute
* -= No permission

(group)

* r = read
* w = write
* x = execute
* -= No permission

(other)

* r = read
* w = write
* x = execute
* -= No permission

##### Chmod
* chmod -x <filename> (remove execute permissions for all owners)
* chmod g-w <filename> (remove write permissions for group)
* chmod g+x <filename> (add execute permissions for group)
* chmod u+x <filename> (add execute permission for user)
* chmod o+x <filename> (add execute permission for others)
* chmod g=rwx <filename> (sets specific block permissions for group)
* chmod 777 <filename> (gives all permission to all owners)
