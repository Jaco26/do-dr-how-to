
# Setup a non-root user

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

