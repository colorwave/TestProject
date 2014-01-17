# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"

  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.hostname = "vagrant.example.com"
  config.vm.network :forwarded_port, guest: 80, host: 1234
  config.vm.network :private_network, ip: "192.168.33.10"
  config.vm.synced_folder ".", "/var/www/app.local", :nfs => true
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
  end
  config.vm.provision :puppet do |puppet|
    #puppet.options        = "--verbose --debug"
    #puppet.module_path    = "app/Resources/puppet/modules"
    puppet.manifests_path = "app/Resources/puppet/manifests"
    puppet.manifest_file  = "base.pp"
  end
end
