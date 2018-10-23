# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/bionic64"

  config.ssh.insert_key = false
  
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox",  SharedFoldersEnableSymlinksCreate: false
  
  config.vm.provider :virtualbox do |v|
    v.name = "sh3d"
    v.memory = 2048
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
	v.customize ["modifyvm", :id, "--nictype1", "virtio"]
  end
  
  config.vm.hostname = "sh3d"
  config.vm.network :private_network, ip: "192.168.33.22"

  
  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define :sh3d do |sh3d|
  end
  
  $bootstrap = <<-BOOTSTRAP
#	add-apt-repository ppa:jonathonf/python-2.7 -y
#	apt-get update -y
#	apt-get install python2.7 -y
   BOOTSTRAP


  config.vm.provision "shell", inline: $bootstrap

  
  # Ansible provisioner.
  config.vm.provision "ansible_local" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "provisioning/playbook.yml"
#    ansible.inventory_path = "provisioning/inventory"
    ansible.become = true
	ansible.galaxy_role_file = "requirements.yml"
  end
end