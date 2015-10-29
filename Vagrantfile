# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant::configure(VAGRANTFILE_API_VERSION) do |global_config|
  global_config.ssh.forward_agent = true
  global_config.ssh.insert_key = false  #Multi-machine vagrant setups confuse ansible provisioning due to different keys for each VM
  global_config.ssh.private_key_path = [ "~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key" ] #vagrant needs to be told of location of a private key as we have turned off insert key above
  global_config.hostmanager.enabled = true
 
  global_config.vm.define("git.internal.tld") do |config|
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
    config.vm.network :private_network, ip: "10.0.1.201"
    config.vm.hostname = "git.internal.tld"
    config.vm.box_url = "https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/14.04/providers/virtualbox.box"
    config.vm.box = "ubuntu/trusty64"

    config.vm.provider :virtualbox do |vb|
      vb.customize [
        "modifyvm", :id,
        "--name", "git.internal.tld",
        "--memory", "512",
        "--cpus", "1",
        "--natdnsproxy1", "off",
        "--natdnshostresolver1", "off"
      ]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
      vb.customize [ "createhd", "--filename", "disk-git", "--size", "5000" ]
      vb.customize [ "storageattach", :id, "--storagectl", "SATAController", "--port", 4, "--device", 0, "--type", "hdd", "--medium", "disk-git.vdi" ]
    end
   
    #start ansible provisioning
    config.vm.provision :ansible do |ansible|
      ansible.raw_ssh_args = "-o ConnectionAttempts=3"
      ansible.playbook =  "site.yml"
      ansible.inventory_path = "inventory/hosts.vagrant"
      ansible.limit = 'all'
    end
  end

end
