# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

settings = YAML.load_file 'vagrant.yml'
project_name = settings['project']['name']
project_ip = settings['project']['ip']
project_memory = settings['project']['memory']

Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. See: vagrantup.com.

  # Build against Debian 7, without any config management utilities.
  config.vm.box = "opscode-debian-7.6-i386"
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_debian-7.6-i386_chef-provisionerless.box"

  # Enable host-only access to the machine using a specific IP.
  config.vm.network :private_network, ip: project_ip

  # Shared folders.
  # You can only use NFS (second option below) once nfs-utils is installed.
  # config.vm.synced_folder "/path/to/local/folder", "/path/to/remote"
  # config.vm.synced_folder "/path/to/local/folder", "/path/to/remote",
  #   :nfs => !RUBY_PLATFORM.downcase.include?("w32"),
  #   id: "share"

  # Provider-specific configuration for VirtualBox.
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--name", project_name]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--memory", project_memory]
    v.customize ["modifyvm", :id, "--cpus", 1]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # Set entries in hosts file
  # https://github.com/cogitatio/vagrant-hostsupdater
  if Vagrant.has_plugin?("vagrant-hostsupdater")
    config.hostsupdater.remove_on_suspend = true
    config.vm.hostname = project_name
    config.hostsupdater.aliases = ["admin." + project_name, "phpmyadmin." + project_name, "adminer." + project_name]
  end

  # Cache and sharing a common package cache among similiar VM instances
  # https://github.com/fgrehm/vagrant-cachier/
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  #
  # Pre-provisioning
  # Install Ansible on the VM to run main provisioning from the VM itself
  #
  # Uncomment this if you does not have ansible on host machine
  #$script = <<SCRIPT
  #  sudo apt-add-repository ppa:rquillo/ansible -y
  #  sudo apt-get update -y
  #  sudo apt-get install ansible -y
  #SCRIPT
  #config.vm.provision "shell", inline: $script
  #config.vm.provision "shell" do |sh|
  #  sh.inline = "ansible-playbook /vagrant/provisioning/main.yml --inventory-file=/vagrant/provisioning/hosts --connection=local"
  #end

  # Provisioning configuration for Ansible.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.inventory_path = "vagrant/inventory"
    # Run commands as root.
    ansible.sudo = true
    ansible.raw_arguments = ['-v']
  end

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define :lemp do |lemp|
    lemp.vm.hostname = project_name
  end

end