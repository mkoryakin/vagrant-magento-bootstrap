# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
if [[ ! -f /vagrant/id_rsa ]];then
  ssh-keygen -t rsa -N "" -f /vagrant/id_rsa
fi
for i in 0 1 2 3; do
  ssh-keyscan 192.168.50.10$i >> /home/vagrant/.ssh/known_hosts
done
cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
cp /vagrant/id_rsa ~/.ssh/
cp /vagrant/id_rsa.pub ~/.ssh/
sudo chown -R vagrant:vagrant /home/vagrant/.ssh
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install -y ansible
SCRIPT

Vagrant.configure(2) do |config|
  config.vbguest.auto_update = true
  config.vm.box = 'ubuntu/trusty64'
  config.vm.box_check_update = true
  config.vm.provision "shell", inline: $script
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
          vb.memory = "1024"
        else
          vb.memory = "512"
        end
        vb.cpus = 1
      end
    end
  end
end
