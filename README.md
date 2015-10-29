# ansible-git
Ansible deployment of git server with gitolite authentication and cgit web interface on Ubuntu 14.04

## Description

This role deploys a git repository server with gitolite for repo management and cgit web frontend for navigation/viewing of repos.

## To create in vagrant:
Vagrant uses "hostmanager" plugin. Before running, install with: vagrant plugin install vagrant-hostmanager

`vagrant up`

git service:

`ssh git@git.internal.tld`

cgit service:

`http://git.internal.tld/cgit`

To begin managing git with gitolite, clone the admin repo:

`git clone ssh@git.internal.tld/gitolite-admin`

edit `inventory/group_vars/all.yml` to change passwords or software versions.

## Gitolite bootstrapping
gitolite will be bootstrapped with a public key from ./files/ directory to allow you to clone it right away. Save your key there as `./files/<username>.pub`. This <username> must corrispond with `gitolite_username` in inventory/group_vars.all.yml . Alternatively, do nothing and ansible will use your public key from `~/.ssh/id_rsa.pub`.
you can then clone the admin repo and begin manageing git.


