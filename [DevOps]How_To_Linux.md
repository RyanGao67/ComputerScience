# Shoud I be installing my JVM technologies using SDKman or apt-get?
![](./img/linux1.png) 

# Why do some system users have /usr/bin/false as their shell? What does that mean?   

* This helps to prevent users from logging onto a system.      
* Sometimes you need a user account for a specific task. Nevertheless, no one should be able to interact with this account on the computer.    
* These are, on one hand, system user accounts.    
* On the other hand, this is an account for which FTP or POP3 access is possible, but just no direct shell login.     

* If you look more closely at the /etc/passwd file, you will find the /bin/false command as a login shell for many system accounts.    
* Actually false is not a shell, but a command that does nothing and then also ends with a status code that signals an error.    
* The result is simple. The user logs in and immediately sees the login prompt again.    

* These users exist to be the owner of specific files or precesses and are not intended to be login accounts.     
* If the value of the "shell" field is not listed in /etc/shells, then programs such as FTP deamons do not allow access.     
* Additionally, for programs that do not check /etc/shells, they make use of the fact that /bin/false will immediately return and deny an interactive shell.    

# Virtualbox 
Virtual box is a full virtualization product, meaning that it allows you to run an unmodified operating system with all of its installed software in a special environment on top of your existing operating system. 

The special environment called a virtual machine is created by virtualization software by intercepting access to certain hardware components and certain features. 

The physical compoter is called the host while the virtual machine is called a guest. Most of the guest code runs unmodified derectly on the host computer and the guest operating system thinks it's running on a real machine.

The guest operating system thinks it's running on real hardware and it really doesn't know that it's inside a virtual machine. If you're doing something inside a virtual machine, in the operating system of the virtual machine, says something like performing this action will erase all the data on all your disks. It's really only referring to the virtual disks that it has access to, not your physical device on your real physical host computer.

# Virtualbox guest additions
Virtual Box provides software that can be installed on the guest operating system. This software is called Virtual Box guest additions. This software allows the guest operating system to access shared folders on the host system share the clipboard and some other similar actions. This is not required but it can be very useful.

# Virtual disk
allocate some resources such as memory and disk space to the virtual machine. When you create a virtual disk for the virtual machine you can make it dynamically allocated.  a virtual disk is simply a file on the host system. When you choose to dynamically allocate that disk it will only use space on your physical disk as that space is used inside the virtual machine guest. this means that the disk file will grow as needed.

# Vagrant box
* Box = Operating System Image
* vagrant box add USER/BOX


This command will download and store that box on your local system. You only need to download a box once as this image will be cloned when you create a new virtual machine with vagrant using the boxes name

# Vagrant Projects
* Vagrant project = Folder with vagrantfile
* mkdir vml
* cd vml
* vagrant init jasonc/centos7  ====> this will initialze the vagrant project file 
* vagrant up

The first time you run the vagrant up command vagrant will import or clone that vagrant box into virtual box and then start it. If vagrant detects that the virtual machine already exists in virtual box it will just simply start it. 

By default when the virtual machine is started,it is started in headless mode meaning that there is no user interface for the machine visible on your local host machine.

```
vagrant up [VM_NAME]
vagrant up master
vagrant ssh [VM_NAME]
vagrant halt [VM] ===> stops the VM 
vagrant up [VM]
vagrant suspend [VM]
vagrant resume [VM]
vagrant destroy [VM]  ====> removes the VM
vagrant ===> list options
```

[https://www.udemy.com/course/linux-shell-scripting-projects/learn/lecture/7980558#overview](https://www.udemy.com/course/linux-shell-scripting-projects/learn/lecture/7980558#overview)

### One useful command: type -a [COMMAND]
```
type -a echo
type -a bash
```
This checks the command information. 

```
for example : 
[vagrant@localhost vagrant]$ type -a echo
echo is a shell builtin
echo is /usr/bin/echo

```

### command: uptime
```
[vagrant@localhost vagrant]$ uptime
 16:40:38 up 25 min,  1 user,  load average: 0.00, 0.01, 0.05
```
This command will show you the uptime. It is not part of the shell. If the command is part of the shell we can 
```
help <COMMAND>
eg, help echo
```
If the command is not part of the shell we 
```
man <COMMAND>
eg, man uptime
```

# Shell Script
[https://github.com/RyanGao67/Shell_Script](https://github.com/RyanGao67/Shell_Script)

# awk    
[https://www.geeksforgeeks.org/awk-command-unixlinux-examples/](https://www.geeksforgeeks.org/awk-command-unixlinux-examples/)  

# uniq
[https://www.geeksforgeeks.org/uniq-command-in-linux-with-examples/](https://www.geeksforgeeks.org/uniq-command-in-linux-with-examples/)

# array
[https://opensource.com/article/18/5/you-dont-know-bash-intro-bash-arrays](https://opensource.com/article/18/5/you-dont-know-bash-intro-bash-arrays)

# RPM 
[https://rpm-packaging-guide.github.io/](https://rpm-packaging-guide.github.io/)

# Service
[https://www.digitalocean.com/community/tutorials/how-to-configure-a-linux-service-to-start-automatically-after-a-crash-or-reboot-part-1-practical-examples](https://www.digitalocean.com/community/tutorials/how-to-configure-a-linux-service-to-start-automatically-after-a-crash-or-reboot-part-1-practical-examples)

[https://linuxconfig.org/how-to-create-systemd-service-unit-in-linux](https://linuxconfig.org/how-to-create-systemd-service-unit-in-linux)

[https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)
