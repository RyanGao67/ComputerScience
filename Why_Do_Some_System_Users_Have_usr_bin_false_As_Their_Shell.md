Why do some system users have /usr/bin/false as their shell? What does that mean?   

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
