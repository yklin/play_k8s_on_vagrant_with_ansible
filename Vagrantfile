# -*- mode: ruby -*-
# # vi: set ft=ruby :

Vagrant.require_version ">= 2.2.0"

$instance_count = 3
$vm_cpus = 2
$vm_memory = 2048
$subnet = "10.10.10"


Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox"
  config.vm.box = "bento/centos-7.6"

  (1.. $instance_count).each do |i|
    name = "k8s-#{i}"
    ip = "#{$subnet}.#{i+10}"
    primary = i == 1? true: false

    config.vm.define "#{name}", primary: primary do |node|
      node.vm.hostname = "#{name}"
      node.vm.network "private_network", ip: "#{ip}", auto_config: true

      config.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.gui = false
        vb.memory = $vm_memory
        vb.cpus = $vm_cpus
        vb.linked_clone = true
      end

      if i == $instance_count
        config.vm.provision "ansible_local" do |ansible|
          ansible.playbook = "playbook.yml"
          ansible.inventory_path = "inventory/vagrant/hosts.ini"
          ansible.become = true
          ansible.limit = "all"
          ansible.compatibility_mode = "2.0"
        end
      end
    end

  end

end
