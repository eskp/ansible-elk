# -*- mode: ruby -*-
# vi: set ft=ruby :

required_plugins = %w( vagrant-hosts )
required_plugins.each do |plugin|
  puts "Plugins missing. Run 'vagrant plugin install #{plugin}'" unless Vagrant.has_plugin? plugin
end

SUBNET="10.8.8"

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "centos65"
  #config.vm.box = "ubuntu1404"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.define "elk" do |node|
    node.vm.hostname = "elk"
    node.vm.network "private_network", ip: "#{SUBNET}.11"
    node.vm.network "forwarded_port", guest: 5601, host: 5601
    node.vm.network "forwarded_port", guest: 9200, host: 9200
    node.vm.network "forwarded_port", guest: 25826, host: 25826
    node.vm.network "forwarded_port", guest: 80, host: 8080
    #node.vm.provision :shell, path: "ansible.sh"
    #node.vm.provision :shell, inline: 'ansible-playbook /vagrant/ansible/monitoring.yml -c local -v'
    node.vm.provision "ansible" do |a|
      a.playbook = "ansible/elk.yml"
    end
    node.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
  end

  config.vm.define "forwarder" do |node|
    node.vm.hostname = "forwarder"
    node.vm.network "private_network", ip: "#{SUBNET}.12"
    node.vm.network "forwarded_port", guest: 9000, host: 9000
    # Populate /etc/hosts with each VM in this file
    node.vm.provision :hosts
    node.vm.provision "ansible" do |a|
      a.playbook = "ansible/forwarder.yml"
    end
  end

end
