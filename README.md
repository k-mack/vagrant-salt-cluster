# About

**Note: This is not finished and does not currently work!**

Starts a virtual cluster provisioned with [Salt](https://saltstack.com/).

# Getting Started

1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. Install [Vagrant](https://www.vagrantup.com/docs/installation/)
3. In this repository's working directory, install the *Vbguest plugin*:
```
vagrant plugin install vagrant-vbguest
```
4. Run `vagrant up`

**Note:** A package fails to install, reload and provision the VM by running `vagrant reload --provision`.
