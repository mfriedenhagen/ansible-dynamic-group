# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile for bootstrapping a development cluster with
# VirtualBox provider and Ansible provisioner

ANSIBLE_PLAYBOOK = ENV['ANSIBLE_PLAYBOOK'] || "test.yml"
VAGRANTFILE_API_VERSION = "2"
BOX_MEM = ENV['BOX_MEM'] || "128"
BOX_NAME =  ENV['BOX_NAME'] || "debian/jessie64"
CLUSTER_HOSTS = ENV['CLUSTER_HOSTS'] || "vagrant_hosts"

Vagrant.require_version ">= 1.5.0"
nodes = [
  {
    :id => '1',
    :ip => '10.1.42.211'
  },
  {
    :id => '2',
    :ip => '10.1.42.212'
  },
  {
    :id => '3',
    :ip => '10.1.42.213'
  },
  {
    :id => '4',
    :ip => '10.1.42.214'
  },
  {
    :id => '5',
    :ip => '10.1.42.215'
  },
]
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  nodes.each do |opts|
    # Configure 3 Consul nodes
    config.vm.define 'consul' + opts[:id] do |config|
      config.vm.box = BOX_NAME
      config.vm.network :private_network, ip: opts[:ip]
      config.vm.hostname = "consul" + opts[:id] + ".local"
      config.ssh.forward_agent = true
      config.vm.provider "virtualbox" do |v|
        v.name = "consul-node" + opts[:id]
        v.customize ["modifyvm", :id, "--memory", BOX_MEM]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
        v.customize ["modifyvm", :id, "--cpus", "1"]
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      end
      config.vm.provision :hosts do |provisioner|
        nodes.each do |opts|
          provisioner.add_host opts[:ip], ['consul' + opts[:id] + '.local']
        end
      end
    end
  end
end
