# Setup a basic firewall

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