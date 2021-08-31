# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$bootstrap=<<SCRIPT
export DEBIAN_FRONTEND=noninteractive
apt-get update
apt-get install apt-transport-https \
            ca-certificates \
            curl \
            software-properties-common -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-get update
apt install docker-ce -y
systemctl status docker
groupadd docker && usermod -aG docker ${USER}
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
     vb.customize ["modifyvm", :id, "--memory", ENV['MEMORY']]
    end
    config.env.enable
    foobar.vm.provision :shell, inline: $bootstrap

    if (ENV['CONNECT_FROM_IRAN'] == 'true')
      foobar.vm.provision :shell, inline: $dockerir
    end

    if(ENV['SERVICE_WORDPRESS'] == 'true')
      foobar.vm.provision :docker
      foobar.vm.provision :docker_compose, yml: "/vagrant/services/wordpress/docker-compose.yml", rebuild: true, run: "always"
    end
  end
end
