# Use your server as a remote git repository and automate deployment with git hooks

This two-part guide will walk you through setting up a remote git repository on your server and, optionally, configure it to execute build scripts or other jobs every time you push to a specific branch.

## Create a remote repository

First, login to your server as your non-root user (I'll use one I named `charlie`).
```
$ ssh charlie@<my_server_ip>
```
In your user's home directory (`/home/charlie`), create a bare git repository with
```
$ git init --bare my-repo.git
```
And that's it! You now have a fully functioning remote git repository [caveat](#a-note-on---bare)

I'll configure a local git remository (on my laptop) to push to my new remote (on my server)
```
$ git remote add server charile@<my_server_ip>:/home/charlie/my-repo.git
```
Now I can push and pull from my new remote (which I named `server`) like I would from GitHub or GitLab! For example:
```
$ git push -u server main
```
### A note on `--bare`
I used the `--bare` flag When we created the git repo on our server. This creates a `bare` git repository – one without a working directory. Since a bare repository contains no working directory, you can't make changes to code inside it. This helps to prevent conflicts. It also means that we'll need to take extra steps if we want to be able to build/deploy our code when we push to our remote.


## Automate build and deployment jobs with git hooks
`git` exposes hooks (or events) that fire at various points during execution of various `git` commands. We'll only use the `post-receive` hook but you can read a more comprehensive description of available hooks and their behavior [here](https://www.digitalocean.com/community/tutorials/how-to-use-git-hooks-to-automate-development-and-deployment-tasks).


Use `nano` or `vim` to create a new file at `my-repo.git/hooks/post-receive`.
```
# vim my-repo.git/hooks/post-receive
```
