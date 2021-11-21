# Install and use docker

**_source_**: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

**_prerequisites_**: [connecting with ssh](./ssh) and [initial server setup](./initial-setup)

## Installing Docker with APT

_**APT** is Ubuntu's default package manager. Run `man apt` in your Ubuntu environment (your server) to view the manual page documentation or search duckduckgo to learn more about it_

Before we install docker, we'll setup our server to:
1. download docker from Docker's official repository (because Ubuntu's default repository might not have the latest version)
2. cryptographically validate what we download from Docker's repository by adding Docker's GPG key.

- [x] First, download the latest package information from all configured sources.
```
# sudo apt update
```
- [x] Next, install packages which let `apt` use HTTPS (you may already have these installed)
```
# sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
- [x] Then add the official Docker repository GPG key
```
# curl -fsSl https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
- [x] Add the Docker repository to APT sources. This will update our list of available packages to include those available from the Docker repository
```
sudo add-apt-reository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
- [x] Verify that APT is configured to download Docker from the official Docker repository and not Ubuntu's default package repo
```
# apt-cache policy docker-ce
```
You'll want to see something like this
```
docker-ce:
  Installed: (none)
  Candidate: 5:20.10.11~3-0~ubuntu-focal
  Version table:
    5:20.10.11~3-0~ubuntu-focal 500
      500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
```

- [x] Finally, we're ready to install Docker
```
# sudo apt install docker-ce
```
Docker should now be installed, the daemon started, and the process enabled to start on boot.

- [x] Verify that it’s running
```
# sudo systemctl status docker
```
The `docker` CLI should now be available. Follow the steps in the next section to use it without `sudo`

## Using `docker` CLI without `sudo`


By default, only **root** and user's in the **docker** group can use the `docker` CLI. To avoid periodically needing entering your password to use docker, add your non-root user to the **docker** group (this is also useful for granting `docker` privileges to non-root users _without_ admin privileges).

```
# sudo usermod -aG docker ${USER}
```
_If you're not currently logged in as the user you're adding to the **docker** group, you'll need to substitute `${USER}` with their actual user name._

You'll need to logout and log back in to apply the new group membership. You can do this by exiting your ssh session and then reconnecting with your user. An easier option is to run this:
```
# su --login ${USER}
```
This command restarts the shell as a login shell with an environment similar to a real login. After entering your password, you can check to see that your user has been added to the **docker** group by running:
```
# groups
```
You should see output like
```
<youruser> sudo docker
```