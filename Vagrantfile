# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. See: vagrantup.com.

  # Build against Debian 7, without any config management utilities.
  config.vm.box = "opscode-debian-7.6-i386"
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_debian-7.6-i386_chef-provisionerless.box"

  # Enable host-only access to the machine using a specific IP.
  config.vm.network :private_network, ip: "192.168.33.10"

  # Shared folders.
  # You can only use NFS (second option below) once nfs-utils is installed.
  # config.vm.synced_folder "/path/to/local/folder", "/path/to/remote"
  # config.vm.synced_folder "/path/to/local/folder", "/path/to/remote",
  #   :nfs => !RUBY_PLATFORM.downcase.include?("w32"),
  #   id: "share"

  # Provider-specific configuration for VirtualBox.
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--name", "lemp"]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--memory", 512]
    v.customize ["modifyvm", :id, "--cpus", 1]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

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
    lemp.vm.hostname = "lemp"
  end

end