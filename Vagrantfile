# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Config provider resources -- make all VMs the same
  config.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 1
  end

  config.vm.define :master do |master_config|
    # Config vm box
    master_config.vm.box = "centos/7"
    master_config.vm.box_version = "= 1611.01"
    master_config.vm.host_name = 'saltmaster.local'
    master_config.vm.network "private_network", ip: "192.168.50.10"

    # Sync Salt-specific directories
    master_config.vm.synced_folder "saltstack/salt/", "/srv/salt"
    master_config.vm.synced_folder "saltstack/pillar/", "/srv/pillar"

    # Sync working directory
    master_config.vm.synced_folder ".", "/vagrant"

    # Ensure the private network comes up
    # see https://github.com/mitchellh/vagrant/issues/5590
    # see https://github.com/mitchellh/vagrant/issues/5590#issuecomment-105886361
    # (This seems to only be a problem on CentOS 7.)
    master_config.vm.provision "shell", inline: <<-SHELL
      sudo nmcli connection reload
      sudo systemctl restart network.service
    SHELL

    # Install those to be able to use gitfs
    # see https://github.com/saltstack/salt-bootstrap/issues/245
    master_config.vm.provision "shell", inline: <<-SHELL
      sudo yum -y update
      sudo yum install -y epel-release vim git-core python-setuptools python-devel GitPython
    SHELL

    master_config.vm.provision :salt do |salt|
      salt.master_config = "saltstack/etc/master"
      salt.minion_config = "saltstack/etc/master_minion"
      salt.master_key = "saltstack/keys/master/master.pem"
      salt.master_pub = "saltstack/keys/master/master.pub"
      salt.minion_key = "saltstack/keys/master/master_minion.pem"
      salt.minion_pub = "saltstack/keys/master/master_minion.pub"
      salt.seed_master = {
        "master-minion" => "saltstack/keys/master/master_minion.pub"
      }

      # Install Salt from GitHub
      salt.install_type = "git"
      salt.install_args = "v2016.11.1"
      #salt.bootstrap_options = "-P -c /tmp" # Do NOT need for Git-based bootstraps

      # Make this VM the Salt Master as well as a Minion
      salt.install_master = true
      salt.no_minion = false

      salt.verbose = true
      salt.colorize = true
    end
  end
end
