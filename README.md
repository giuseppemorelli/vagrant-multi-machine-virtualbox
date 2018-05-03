[![stable version](https://img.shields.io/badge/stable%20version-1.2.3-green.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/releases/tag/1.2.3)
[![develop](https://img.shields.io/badge/beta%20version-branch%20develop-oran.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/tree/develop)
[![license](https://img.shields.io/badge/license-OSL--3-blue.svg?style=flat-square)](https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/blob/master/LICENSE.txt)

# Vagrant Multi Machine Virtualbox

This is a Multi Machine system for vagrant with Virtualbox provider.  

## Features
- vagrant multi machine: create multiple machine at the same time
- YAML config file. No more Vagrantfile to edit!
- choose your favourite box, or get pre-configured box (see [BOXES.md](BOXES.md))
- vagrant provision with script or ansible (configurable via yaml)
- share your folder with Virtualbox, NFS or Rsync system directly from yaml file
- integration - vagrant plugin HostsUpdater: no more /etc/hosts file to edit!

## Requirements
- virtualbox 5.x
- vagrant > 1.8.4
- (optional)vagrant HostsUpdater plugin: https://github.com/cogitatio/vagrant-hostsupdater
  install with `vagrant plugin install vagrant-hostsupdater`
- (in case of error of shared folder mount errors) vagrant vagrant-vbguest plugin (install with the command `vagrant plugin install vagrant-vbguest`)

## Vagrant boxes created
See `BOXES.md` for software installed list.

- giuseppemorelli/lamp-stack 1.1.0 (debian stretch 9.3)
- giuseppemorelli/lamp-stack 1.0.3 (debian jessie 8.6)
- giuseppemorelli/lamp-stack 1.0.2 (debian jessie 8.6)
- giuseppemorelli/lamp-stack 1.0.1 (debian jessie 8.6) (deprecated)

## How to use
- download latest version [https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/releases](https://github.com/gmdotnet/vagrant-multi-machine-virtualbox/releases)
- extract on your favorite work folder
- rename `config/config.yaml.sample` in `config/config.yaml`
- change settings in `config/config.yaml`
- install git submodule with `git submodule init && git submodule update`
- run `vagrant up` on folder where is `Vagrantfile`
- (optional) make your configuration on vagrant machine entering by run `vagrant ssh`
- have fun and happy coding!

### Config file `config.yaml`

Tab indent: 4 spaces or tab

| Field                               | Type          | Description                                             | Note  |
| ----------------------------------- | ------------- | ------------------------------------------------------- | ----- |
| host                                | group field   | Each group of `host` create a machine in virtualbox     |       |
| enable                              | boolean       | Enable or not the machine                               | Disabled machine aren't managed by Vagrant file, so if you want to destroy it you have to make this flag with `yes` |
| vagrantbox_name                     | string        | Name for vagrant software                               |       |
| hostname                            | string        | Hostname of the machine                                 |       |
|                                     |               |                                                         |       |
| **box**                             |               |                                                         |       |
| box > name                          | string        | Name of the public vagrant box                          | Need to be publish in https://app.vagrantup.com/boxes/search |
| box > version                       | version x.y.z | Vagrant box version                                     | Leave empty if you want to get latest version |      
| box > check_update                  | boolean       | Check for update of your box                            |       |
|                                     |               |                                                         |       |
| private_ip                          | ipv4          | Internal ipv4 of the machine                            | Don't use same subnet of your computer |
| ram                                 | int           | Amount of ram to allocate                               |       |
| cpu                                 | int           | Amount of cpu to use                                    |       |
| **extra_hard_disk**                 |               |                                                         |       |
| extra_hard_disk > create            | boolean       | Flag to create new extra hard disk                      |       |
| extra_hard_disk > filepath          | string        | Relative or absolute path for new extra hard disk       |       |
| extra_hard_disk > size              | int           | Size in GB                                              |       |
|                                     |               |                                                         |       |
| **provision**                       |               |                                                         |       |
| provision > ansible > enable        | boolean       | Enable Ansible provisioning                             |       |
| provision > ansible > playbook_path | string        | Relative path from Vagrantfile of your Ansible Playbook |       |
| provision > script > enable         | boolean       | Enable script provisioning                              |       |
| provision > script > path           | string        | Relative path from Vagrantfile of your script           |       |
|                                     |               |                                                         |       |
| **plugins**                         |               |                                                         |       |
| plugins > hostsupdater > enable     | boolean       | Enable or not hostsupdater plugin                       | https://github.com/cogitatio/vagrant-hostsupdater |
| plugins > hostsupdater > permanent  | boolean       | Your changes to /etc/hosts will be permanent            | Only if you destroy the machine, entries in /etc/hosts will be removed |
| plugins > hostsupdater > aliases    | array         | domain aliases for the same ip                          | Leave blank for no aliases |
|                                     |               |                                                         |       |
| **share**                           |               |                                                         |       |
| share > folder                      | group field   | Group of shared folder via virtualbox system            | https://www.vagrantup.com/docs/synced-folders/basic_usage.html |
| share > folder > host_folder        | string        | Path of the folder on your machine                      |       |
| share > folder > vagrant_folder     | string        | Path of the folder inside vagrant machine               |       |
| share > folder > owner              | string        | Change owner folder inside vagrant machine              | Default: vagrant |
| share > folder > group              | string        | Change owner folder inside vagrant machine              | Default: vagrant |
|                                     |               |                                                         |       |
| **rsync**                           |               |                                                         |       |
| rsync > folder                      | group field   | Group of sync folder via rsync                          |       |
| rsync > folder > host_folder        | string        | Path of the folder on your machine                      |       |
| rsync > folder > vagrant_folder     | string        | Path of the folder inside vagrant machine               |       |
| rsync > folder > owner              | string        | Change owner folder inside vagrant machine              | Default: vagrant |
| rsync > folder > group              | string        | Change owner folder inside vagrant machine              | Default: vagrant |
| rsync > folder > options            | group field   | Rsync parameters. One per line                          | https://www.vagrantup.com/docs/synced-folders/rsync.html |
| rsync > folder > exclude            | group field   | Exclude folders from rsync                              | https://www.vagrantup.com/docs/synced-folders/rsync.html#rsync__exclude |
|                                     |               |                                                         |       |
| **nfs**                             |               |                                                         |       |
| nfs > folder                        | group field   | Group of sync folder via nfs                            | https://www.vagrantup.com/docs/synced-folders/nfs.html |
| nfs > folder > host_folder          | string        | Path of the folder on your machine                      |       |
| nfs > folder > vagrant_folder       | string        | Path of the folder inside vagrant machine               |       |
| nfs > folder > options              | group field   | Nfs options. One per line                               | https://www.vagrantup.com/docs/synced-folders/nfs.html#nfs-synced-folder-options |

## Find out GMdotnet Vagrant projects
- Vagrant multi machine for **Amazon AWS**: [vagrant-multi-machine-amazon-aws](https://github.com/gmdotnet/vagrant-multi-machine-amazon-aws)
- Vagrant multi machine for **Digital Ocean**: [vagrant-multi-machine-digital-ocean](https://github.com/gmdotnet/vagrant-multi-machine-digital-ocean)

## Contribution
Any contribution is highly appreciated. The best way to contribute code is to open a [pull request on GitHub](https://help.github.com/articles/using-pull-requests).<br />Please create your pull request against the `develop` branch
