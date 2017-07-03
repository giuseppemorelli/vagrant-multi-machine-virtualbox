# Vagrant Multi Machine Virtualbox

This is a Multi Machine system for vagrant with Virtualbox provider.  

## Features
- vagrant multi machine: create multiple machine at the same time
- YAML config file. No more Vagrantfile to edit!
- choose your favourite box, or get pre-configured box (see BOXES.md)
- vagrant provision with script or ansible (configurable via yaml)
- integration - vagrant plugin HostsUpdater: no more /etc/hosts file to edit!

## Requirements
- virtualbox 5.x
- vagrant >1.8.4
- (optional)vagrant HostsUpdater plugin: https://github.com/cogitatio/vagrant-hostsupdater
  install with `vagrant plugin install vagrant-hostsupdater`
- (in case of error of shared folder mount errors) vagrant vagrant-vbguest plugin (install with the command `vagrant plugin install vagrant-vbguest`)

## Vagrant boxes created
See `BOXES.md` for software installed list.

- giuseppemorelli/lamp-stack 1.0.1 (debian jessie 8.5)
- giuseppemorelli/lamp-stack 1.0.2 (debian jessie 8.6)
- giuseppemorelli/lamp-stack 1.0.3 (debian jessie 8.6)

## How to use
- download https://github.com/gmdotnet/Vagrant-LAMP/archive/master.zip
- unzip on your favorite work folder
- rename `config/config.yaml.sample` in `config/config.yaml`
- change settings in `config/config.yaml`
(if you need more information about sync folder and rsync folder just have a look here: https://www.vagrantup.com/docs/synced-folders/basic_usage.html)
- run `vagrant up` on folder where is `Vagrantfile`
- (optional) make your configuration on vagrant machine entering by run `vagrant ssh`
- have fun and happy coding!

## Contribution
Any contribution is highly appreciated. The best way to contribute code is to open a [pull request on GitHub](https://help.github.com/articles/using-pull-requests).<br />Please create your pull request against the `develop` branch

### Credits
Giuseppe Morelli - [giuseppemorelli.net](http://www.giuseppemorelli.net)
