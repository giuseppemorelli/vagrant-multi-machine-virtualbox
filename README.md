[![stable version](https://img.shields.io/badge/stable%20version-1.1.0-green.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/releases/tag/1.1.0)
[![develop](https://img.shields.io/badge/beta%20version-branch%20develop-oran.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/tree/develop)
[![license](https://img.shields.io/badge/license-OSL--3-blue.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/blob/master/LICENSE.txt)
[![gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg)](https://gitter.im/GMdotnet/Lobby?utm_source=share-link&utm_medium=link&utm_campaign=share-link)

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
- download https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/archive/master.zip
- unzip on your favorite work folder
- rename `config/config.yaml.sample` in `config/config.yaml`
- change settings in `config/config.yaml`
(if you need more information about sync folder and rsync folder just have a look here: https://www.vagrantup.com/docs/synced-folders/basic_usage.html)
- run `vagrant up` on folder where is `Vagrantfile`
- (optional) make your configuration on vagrant machine entering by run `vagrant ssh`
- have fun and happy coding!

### Config file `config.yaml`

| Field                               | Type          | Description                                             | Note |
| ----------------------------------- | ------------- | ------------------------------------------------------- | ---- |
| host                                | group field   | Each group of `host` create a machine in virtualbox     |      |
| enable                              | boolean       | Enable or not the machine                               | Disabled machine aren't managed by Vagrant file, so if you want to destroy it you have to make this flag with `yes` |
| vagrantbox_name                     | string        | Name for vagrant software                               |      |
| hostname                            | string        | Hostname of the machine                                 |      |
| box > name                          | string        | Name of the public vagrant box                          | Need to be publish in https://app.vagrantup.com/boxes/search |
| box > version                       | version x.y.z | Vagrant box version                                     |      |      
| box > check_update                  | boolean       | Check for update of your box                            |                                        |
| private_ip                          | ipv4          | Internal ipv4 of the machine                            | Don't use same subnet of your computer |
| ram                                 | int           | Amout of ram to allocate                                |       |
| provision > ansible > enable        | boolean       | Enable Ansible provisioning                             |       |
| provision > ansible > playbook_path | string        | Relative path from Vagrantfile of your Ansible Playbook |       |
| provision > script > enable         | boolean       | Enable script provisioning                              |       |
| provision > script > path           | string        | Relative path from Vagrantfile of your script           |       |

## Contribution
Any contribution is highly appreciated. The best way to contribute code is to open a [pull request on GitHub](https://help.github.com/articles/using-pull-requests).<br />Please create your pull request against the `develop` branch

### Credits
Giuseppe Morelli - [giuseppemorelli.net](http://www.giuseppemorelli.net)
