# -*- mode:1
# : ruby -*-
# # vi: set ft=ruby :
vagrant_dir = File.expand_path(File.dirname(__FILE__))
roles_exist = 0
ansible_verbosity = '-vvv'
# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"

# Require YAML module
require 'yaml'

# Read YAML file with box details
servers = YAML.load_file('servers.yml')

# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Iterate through entries in YAML file
  servers.each do |servers|
    config.vm.define servers["name"] do |srv|
      if Vagrant.has_plugin?("vagrant-protect")
          srv.protect.enabled = servers["protected"]
      end
          srv.vm.box = servers["box"]
          srv.vm.network "private_network", ip: servers["ip"]
          # Virtaulbox specific configuration
          srv.vm.provider :virtualbox do |vb|
            vb.name = servers["name"]
            vb.memory = servers["ram"]
          end
          # Ansible provisioner
          srv.vm.provision :ansible do |ansible|
                ansible.limit = servers[:name]
                ansible.playbook = "tests/site.yml"
                ansible.sudo = "true"
                ansible.sudo_user = "root"
                ansible.host_key_checking = "false"
                ansible.verbose = "#{ansible_verbosity}"
                ansible.raw_arguments = ["--connection=paramiko"]
          end
    end
  end
end
