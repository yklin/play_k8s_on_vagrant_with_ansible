# -*- mode: ruby -*-
# # vi: set ft=ruby :

Vagrant.require_version ">= 2.0.0"

$master_count = 2
$node_count = 2
$vm_cpus = 1
$vm_memory = 2048

$subnet = "10.10.10"


def setup_vbox(config, name, ip_address, primary: false)
  config.vm.define "#{name}", primary: primary do |n|
    n.vm.hostname = "#{name}"
    n.vm.network "private_network", ip: "#{ip_address}", auto_config: true
    config.vm.provider "virtualbox" do |vb|
      vb.name = "#{name}"
      vb.gui = false
      vb.memory = $vm_memory
      vb.cpus = $vm_cpus
      vb.linked_clone = true
    end
  end
end


Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox"
  config.vm.box = "centos/7"

  # nodes
  (1.. $node_count).each do |index|
    name = "k8s-node-#{index}"
    ip = "#{$subnet}.#{index+100}"
    setup_vbox(config, name, ip)
  end

  # second and others masters
  (2.. $master_count).each do |index|
    name = "k8s-master-#{index}"
    ip = "#{$subnet}.#{index+10}"
    setup_vbox(config, name, ip)
  end

  ## first master to run ansible
  name = "k8s-master-1"
  ip = "#{$subnet}.10"
  setup_vbox(config, name, ip, primary: true)
end
