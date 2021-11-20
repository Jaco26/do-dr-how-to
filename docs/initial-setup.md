# Initial Server Setup on Ubuntu 20.04
**_source_**: https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04

## Setup a non-root user

_For this step, you'll need to be able to [connect to your server using ssh](./ssh.md)_

### What's wrong with `root`?

The **root** user is the default user in a Linux environment and has extensive privileges. For security and to avoid accidental destructive changes, it is best practice to avoid logging in as **root** on a regular basis. For most day-to-day use, it is best to login as a non-**root** user.

### Create a new user

Login as **root** and run:
```
# adduser <newuser>
```
Enter a new password and optionally fill in the remaining prompts (just hit `enter` for any prompt you wish to skip).

### Grant administrative privileges

Add your new user to the **sudo** group so that it can execute commands with **root** admin privileges by prepending any command with `sudo`.
```
# usermod -aG sudo <newuser>
```

### Enable new user `ssh` access with ssh keys

```
# rsync --archive --chown=<newuser>:<newuser> ~/.ssh /home/<newuser>
```

## Setup a basic firewall

Use the `UFW Firewall`, included with Ubuntu 20.04, to connections to certain services (like `OpenSSH`, the service allowing us to connect to the server with `ssh`).

_For a deeper dive, refer to the [UFW Essentials guide](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)_

Applications on your server that can be accessed via network connections can register their profiles with `UFW`. These profiles allow `UFW` to manage their network access. We can see which applications have registered profiles with `UFW` by typing:
```
# ufw app list
Available applications:
  OpenSSH
```
`UFW` is disabled by default but when we enable it, we need to make sure we can access what we need to access.

```
# ufw allow OpenSSH
Rules updated
Rules updated (v6)
```

```
# ufw enable 
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
```

```
# ufw status 
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)    
```