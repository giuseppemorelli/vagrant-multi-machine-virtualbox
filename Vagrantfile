# -*- mode: ruby -*-
# vi: set ft=ruby :

#######################################
##                                   ##
## GMdotnet                          ##
## Vagrant Multi Machine Virtualbox  ##
## Version 1.1.0                     ##
##                                   ##
#######################################

# Import configuration
require 'yaml'

# Load config.yaml file
vagrantconfig = YAML.load_file('config/config.yaml')

Vagrant.configure("2") do |config|
    vagrantconfig['gmdotnet'].each do |machine|
        # switch from machine to host variable
        host = machine['host']
        if host['enable'] == true
            config.vm.define host['vagrantbox_name'] do |vmhost|
                # hostname
                vmhost.vm.hostname = host['hostname']
                # box name
                vmhost.vm.box = host['box']['name']
                # box version
                if host['box']['version'] != nil
                    vmhost.vm.box_version = host['box']['version']
                end
                # check_update
                vmhost.vm.box_check_update = host['box']['check_update']

                # Hostsupdater plugin
                if host['plugin']['hostsupdater']['enable'] == true

                    # Hostsupdater aliases
                    if host['plugin']['hostsupdater']['aliases'] != nil
                        vmhost.hostsupdater.aliases = host['plugin']['hostsupdater']['aliases']
                    end

                    # Hostsupdater permanent role
                    if host['plugin']['hostsupdater']['permanent'] == true
                        vmhost.hostsupdater.remove_on_suspend = false
                    end

                    # Private IP Network
                    vmhost.vm.network "private_network", ip: host['private_ip'], hostsupdater: true
                else
                    # Private IP Network - without hostsupdater plugin
                    vmhost.vm.network "private_network", ip: host['private_ip']
                end
                ## -*- end hostsupdater plugin folders -*-

                # Shared folders
                if host['share'] != nil
                    host['share'].each do |share|
                        # Shared folders - owner
                        owner = "vagrant"
                        group = "vagrant"
                        if share['folder']['owner'] != nil
                            owner = share['folder']['owner']
                        end
                        if share['folder']['group'] != nil
                            group = share['folder']['group']
                        end
                        vmhost.vm.synced_folder share['folder']['host_folder'], share['folder']['vagrant_folder'], create: true, owner: owner, group: group
                    end
                end
                ## -*- end shared folders -*-

                # Rsync folders
                if host['rsync'] != nil
                    host['rsync'].each do |rsync|
                      rsyncoptions = []
                      rsyncexclude = []
                      rsync['folder']['options'].each do |options|
                          rsyncoptions.push(options)
                      end
                      rsync['folder']['exclude'].each do |options|
                          rsyncexclude.push(options)
                      end
                      vmhost.vm.synced_folder rsync['folder']['host_folder'], rsync['folder']['vagrant_folder'], type: "rsync",
                          rsync__args: rsyncoptions,
                          rsync__exclude: rsyncexclude
                    end
                end
                ## -*- end rsync folders -*-

                # NFS folders
                if host['nfs'] != nil
                    host['nfs'].each do |nfs|
                      nfsoptions = []
                      nfs['folder']['options'].each do |options|
                          nfsoptions.push(options)
                      end
                      vmhost.vm.synced_folder nfs['folder']['host_folder'], nfs['folder']['vagrant_folder'], nfs: true, mount_options: nfsoptions
                    end
                end
                ## -*- end nfs folders -*-

                # Virtualbox options
                vmhost.vm.provider "virtualbox" do |vb|
                    # RAM
                    vb.memory = host['ram']
                end

                # Shell provision
                if host['provision']['script']['enable'] == true
                    vmhost.vm.provision "shell", path: host['provision']['script']['path']
                end

                # Ansible provision
                if host['provision']['ansible']['enable'] == true
                    vmhost.vm.provision "ansible_local" do |ansible|
                        ansible.verbose = "vv"
                        ansible.playbook =  host['provision']['ansible']['playbook_path']
                    end
                end
            end
            ## -*- end vmhost -*-
        end
        ## -*- end host enable -*-
    end
    ## -*- end machine -*-
end
## -*- end Vagrant -*-
