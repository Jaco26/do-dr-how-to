# Initial Server Setup on Ubuntu 20.04
**_source_**: https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04

**_prerequisites_**: [connecting with ssh](./ssh.md)

## Setup a non-root user

### What's wrong with `root`?

The **root** user is the default user in a Linux environment and has extensive privileges. For security and to avoid accidental destructive actions, it is best practice to avoid logging in as **root** on a regular basis. For most day-to-day use, it is best to login as a non-**root** user.

### Create a new user

_We'll create a user called **charlie**. You can replace **charlie** with whatever username you want._

Login as **root** and run:
```
# adduser charlie
```
Enter a new password and optionally fill in the remaining prompts (just hit `enter` for any prompt you wish to skip).

### Grant administrative privileges

Add your new user (**charlie**) to the **sudo** group so that it can execute commands with **root** admin privileges by prepending any command with `sudo`.
```
# usermod -aG sudo charlie
```

### Grant ssh access to new user with ssh keys

_refer to the [ssh section](./ssh.md) if you need a refresher on `ssh` stuff._

It's a good idea to setup the ssh client (your laptop or whatever) that you're using to connect to your server to be able to login as your new non-root user **charlie** using `ssh` with ssh-key authentication. If you're currently logged in as **root** with the same public key you plan to use to login with **charlie**, you can simply copy **root**'s `~/.ssh` directory to `/home/charlie` (where `ssh` will look for `authorized_keys` on a connection attempt with **charlie** as the user (`ssh charlie@<host>`)).

The `rsync` command gives us a simple way of doing this and will take care necessary file ownership permission changes.

>  Note: The rsync command treats sources and destinations that end with a trailing slash differently than those without a trailing slash. When using rsync below, be sure that the source directory (~/.ssh) does not include a trailing slash (check to make sure you are not using ~/.ssh/).
>
> If you accidentally add a trailing slash to the command, rsync will copy the contents of the root account’s ~/.ssh directory to the sudo user’s home directory instead of copying the entire ~/.ssh directory structure. The files will be in the wrong location and SSH will not be able to find and use them.

```
# rsync --archive --chown=charlie:charlie ~/.ssh /home/charlie
```
It's a good idea to stay logged in as **root** and open another terminal to verify that you can log in with your non-root user. In another terminal window, run:
```
$ ssh charlie@<host>
```
If you login successfully, verify that your non-root user can execute commands as a `sudo` user:
```
# try creating a file with nano
$ sudo nano hello.txt
```
If that works, you can safely exit **root** and use your non-root user from here on out for most server admin tasks!

## Setup a basic firewall

Use the `UFW Firewall`, included with Ubuntu 20.04, to connections to certain services (like **OpenSSH**, the service allowing us to connect to the server with `ssh`).

_For a deeper dive, refer to the [UFW Essentials guide](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)_

Applications on your server that can be accessed via network connections can register their profiles with **UFW**. These profiles allow you to manage them by name with **UFW**. We can see which applications have registered profiles with **UFW** by typing:
```
$ sudo ufw app list
Available applications:
  OpenSSH
```
**UFW** is disabled by default and before we enable it, we need to allow access to **OpenSSH** (or we won't be able to log back in with `ssh` with  **UFW** enabled).

```
$ sudo ufw allow OpenSSH
Rules updated
Rules updated (v6)
```
Turn on **UFW**
```
$ sudo ufw enable 
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
```
Check whether or not **UFW** is active and see **UFW** managed rules:
```
$ sudo ufw status 
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)    
```