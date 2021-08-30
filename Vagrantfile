# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$bootstrap=<<SCRIPT
apt-get update
apt install apt-transport-https \
            ca-certificates \
            curl \
            software-properties-common
            
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt update
apt-cache policy docker-ce
systemctl status docker
usermod -aG docker ${USER}
SCRIPT

$dockerir=<<SCRIPT
echo 'dockerir'
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "foobar-server" do |foobar|
    foobar.vm.box = "bento/ubuntu-20.04"
    foobar.vm.hostname = "foobar-server"
    foobar.vm.network :private_network, ip: "192.168.33.10"
    foobar.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "1024"]
    end
    foobar.vm.provision :shell, inline: $bootstrap
    foobar.vm.provision :shell, inline: $dockerir
  end
end
