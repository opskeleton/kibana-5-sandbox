# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  device = ENV['VAGRANT_BRIDGE'] || 'eth0'

  config.vm.define :elk_5 do |node|
    node.vm.box = 'ubuntu-16.04.2_puppet-3.8.7' 
    node.vm.hostname = 'elk-5.local'
  end
 
  config.vm.provider :libvirt do |domain,o|
     domain.uri = 'qemu+unix:///system'
     domain.host = 'kibana.local'
     domain.memory = 8096
     domain.cpus = 4
     domain.storage_pool_name = 'kibana'
     o.vm.synced_folder './', '/vagrant', type: 'nfs'
     o.vm.network :public_network, :bridge => device,:dev => device, :mac => '52:54:00:fd:66:22'
  end
 
  config.vm.provision 'shell', inline: 'sudo apt install ruby-augeas -y'
  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = 'manifests'
    puppet.manifest_file  = 'default.pp'
    puppet.options = "--modulepath=/vagrant/modules:/vagrant/static-modules --hiera_config /vagrant/hiera_vagrant.yaml --environment=dev"
  end

end
