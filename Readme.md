# Setup Rails Vagrant environment

## Table of Contents

* [Setup Rails Vagrant environment](#setup-rails-vagrant-environment)
  * [Install Vagrant](#install-vagrant)
  * [Vagrant Plugins](#vagrant-plugins)
  * [Setting Vagrant in Rails](#setting-vagrant-in-rails)
    * [Set Cheffile](#set-cheffile)
    * [Set Vagrantfile](#set-vagrantfile)
* [Running Vagrant](#running-vagrant)
  * [Reconfigure machine](#reconfigure-machine)

## Install Vagrant

1. [Install Vagrant](http://www.vagrantup.com/downloads.html)
1. [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Vagrant Plugins

1. vagrant-vbguest automatically installs the host's VirtualBox Guest Additions on the guest system.
1. vagrant-librarian-chef let's us automatically run chef when we fire up our machine.

```
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-librarian-chef-nochef
```

## Setting Vagrant in Rails

```
cd MY_RAILS_PROJECT
vagrant init
touch Cheffile
```

### Set Cheffile

[Cheffile](Cheffile)

### Set Vagrantfile

[Vagrantfile](Vagrantfile)

## Running Vagrant

`vagrant up`

`vagrant ssh` - SSH access

Vagrant sets up the /vagrant folder as a shared directory between the virtual machine and your host operating system. If you cd /vagrant and run ls you will see all the files from your Rails application.

### Reconfigure machine

If you ever edit your Vagrantfile or Cheffile, you can use the following command to reconfigure the machine.

`vagrant provision`

### Troubleshooting

1. `vagrant up` doesn't find provider
Make sure your VirtualBox tools were added to `PATH`.
For example, for Mac: `/Applications/VirtualBox.app/Contents/MacOS/`
