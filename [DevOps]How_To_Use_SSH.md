## How to set up SSH keys
### What is ssh keys
Secure Shell (SSH)  is a cryptographic network protocol which allows users to securely perform a number of network services over unsecured network. SSH keys provide a more secure way of logging into a server with SSH than using a password alone. 

Generating a key pair provides you with two long string of characters: a public and a private key.

### Step one - create the RSA key pair
```ssh-keygen -t rsa```

### Step two - store the keys and passphrase
```
$ ssh-keygen -t rsa
```
Output
```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/demo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/demo/.ssh/id_rsa.
Your public key has been saved in /home/demo/.ssh/id_rsa.pub.
The key fingerprint is:
4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a
The key's randomart image is:
+--[ RSA 2048]----+
|          .oo.   |
|         .  o.E  |
|        + .  o   |
|     . = = .     |
|      = S = .    |
|     o + = +     |
|      . o + o .  |
|           . o   |
|                 |
+-----------------+
```
The public key is now located in `/home/demo/.ssh/id_rsa.pub`. The private key (identification) is now located in `/home/demo/.ssh/id_rsa`.

### Step three
```
cat ~/.ssh/id_rsa.pub | ssh demo@198.51.100.0 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```
You may see something like:
```
The authenticity of host '198.51.100.0 (198.51.100.0)' can't be established.
RSA key fingerprint is b1:2d:33:67:ce:35:4d:5f:f3:a8:cd:c0:c4:48:86:12.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '198.51.100.0' (RSA) to the list of known hosts.
user@198.51.100.0's password: 
```
This message helps us to make sure that we haven’t added extra keys that you weren’t expecting.

Now you can go ahead and log into your user profile and you will not be prompted for a password. However, if you set a passphrase when creating your SSH key, you will be asked to enter the passphrase at that time (and whenever else you log in in the future).
