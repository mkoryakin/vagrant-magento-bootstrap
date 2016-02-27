# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vbguest.auto_update = true
  config.vm.box = 'ubuntu/trusty64'
  config.vm.box_check_update = true
  {
    'main' => '192.168.50.100',
    'php1' => '192.168.50.101',
    'php2' => '192.168.50.102',
    'db'   => '192.168.50.103',
  }.each do |short_name, ip|
    config.vm.define short_name do |host|
      host.vm.network 'private_network', ip: ip
      host.vm.provider "virtualbox" do |vb|
        if short_name == "db"
          vb.memory = "768"
        else
          vb.memory = "512"
        end
        vb.cpus = 1
      end
    end
  end
end
