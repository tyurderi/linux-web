# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = false

  config.vm.network "private_network", ip: "192.168.44.10"

  config.vm.provider "virtualbox" do |vb|
     vb.name = "web_1"
     vb.memory = "1024"
     vb.cpus = 1
  end
end
