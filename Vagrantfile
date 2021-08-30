# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$bootstrap=<<SCRIPT
apt-get update
apt-get -y install wget curl
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "foobar-server" do |foobar|
    foobar.vm.box = "ubuntu/20.04"
    foobar.vm.hostname = "foobar-server"
    #foobar.vm.network :private_network, ip: "192.168.33.10"
    foobar.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "1024"]
    end
    foobar.vm.provision :shell, inline: $bootstrap
  end

end
