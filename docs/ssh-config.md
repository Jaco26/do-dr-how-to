# Enable or disable password authentication


With a digitalocean droplet, you can launch the recovery console to login as `root` and run the following:
```
# sudo nano /etc/ssh/sshd_config
```
This will load the relevant config file in the `nano` text editor. Use the arrow keys to find the line that says:
```
PasswordAuthentication <yes|no>
```
_You can also use `control + w` to enter "search mode". Type `PasswordAuthentication` and hit enter to go directly to the correct line_

To **enable** password auth, set it to:
```
PasswordAuthentication yes
```
To **disable** password auth, set it to:
```
PasswordAuthentication no
```

Hit `control + x` to begin saving your changes. At the bottom of the screen you'll see:
```
Save modified buffer?                                                                                            
Y Yes
N No           ^C Cancel
```
Hit `y` to save. At the bottom of the scree you'll see:
```
File Name to Write: /etc/ssh/sshd_config                                                                                                                                                                                    
^G Get Help        M-D DOS Format        ...
^C Cancel          M-M Mac Format        ...
```
Hit `enter` to confirm you want to save without changing the filename.

You will exit `nano` and be back in the regular console interface. Next, restart the SSH service with this command:
```
# sudo systemctl restart ssh
```
Congrats! You have successfully enabled password authentication.

This is only a good thing if you need to use `ssh-copy-id` to copy a client's public key to this server.

Be sure you follow the steps above to disable password auth when you are done copying keys!
