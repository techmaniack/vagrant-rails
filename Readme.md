# Setup Rails Vagrant environment

## Install Vagrant

1. (Install Vagrant)[http://www.vagrantup.com/downloads.html]
1. (Install VirtualBox)[https://www.virtualbox.org/wiki/Downloads]

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

```
site "http://community.opscode.com/api/v1"

cookbook 'apt'
cookbook 'build-essential'
cookbook 'mysql', '5.5.3'
cookbook 'ruby_build'
cookbook 'nodejs'
cookbook 'rbenv', git: 'https://github.com/aminin/chef-rbenv'
cookbook 'vim'
```

### Set Vagrantfile

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use Ubuntu 14.04 Trusty Tahr 64-bit as our operating system
  config.vm.box = "ubuntu/trusty64"

  # Configurate the virtual machine to use 2GB of RAM
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
  end

  # Forward the Rails server default port to the host
  config.vm.network :forwarded_port, guest: 3000, host: 3000

  # Use Chef Solo to provision our virtual machine
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks", "site-cookbooks"]

    chef.add_recipe "apt"
    chef.add_recipe "nodejs"
    chef.add_recipe "ruby_build"
    chef.add_recipe "rbenv::user"
    chef.add_recipe "rbenv::vagrant"
    chef.add_recipe "vim"
    chef.add_recipe "mysql::server"
    chef.add_recipe "mysql::client"

    # Install Ruby 2.2.1 and Bundler
    # Set an empty root password for MySQL to make things simple
    chef.json = {
      rbenv: {
        user_installs: [{
          user: 'vagrant',
          rubies: ["2.2.1"],
          global: "2.2.1",
          gems: {
            "2.2.1" => [
              { name: "bundler" }
            ]
          }
        }]
      },
      mysql: {
        server_root_password: ''
      }
    }
  end
end
```

## Running Vagrant

`vagrant up`

`vagrant ssh` - SSH access

Vagrant sets up the /vagrant folder as a shared directory between the virtual machine and your host operating system. If you cd /vagrant and run ls you will see all the files from your Rails application.

### Reconfigure machine

If you ever edit your Vagrantfile or Cheffile, you can use the following command to reconfigure the machine.

`vagrant provision`
