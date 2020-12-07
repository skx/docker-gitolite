# Docker image for Gitolite

Forked from [the original](https://github.com/jgiannuzzi/docker-gitolite) location, as produced by [Jonathan Giannuzzi](https://github.com/jgiannuzzi).


## Quick setup

You'll need two directories to store state:

* SSH keys for admin-purposes
* Repository locations

Create them like so:

        mkdir -p $(pwd)/state/keys
        mkdir -p $(pwd)/state/repos

Launch like this the first time:

        docker run --rm -e SSH_KEY="$(cat ~/.ssh/id_rsa.pub)" -e SSH_KEY_NAME="$(whoami)" -v $(pwd)/state/keys:/etc/ssh/keys -v $(pwd)/state/repos:/var/lib/git jgiannuzzi/gitolite true

Once you've done that the initial SSH keys, etc, will be created.  Run the service for real like so:

        docker run -d --name gitolite -p 4444:22 -v $(pwd)/state/keys:/etc/ssh/keys -v $(pwd)/state/repos:/var/lib/git jgiannuzzi/gitolite


## Post Launch

Once launched clone the repository, and update as appropriate:

        git clone ssh://git@localhost:4444/gitolite-admin.git


## docker compose setup

Something like this:

    version: "3.8"
    services:
      git_host:
        restart: always
        image: jgiannuzzi/gitolite
        ports:
          - "4444:22"
        volumes:
          - /srv/git.steve.fi/gitolite/repos:/var/lib/git
          - /srv/git.steve.fi/gitolite/keys:/etc/ssh/keys



