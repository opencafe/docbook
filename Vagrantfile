# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

#############
#$bootstrap=<<SCRIPT
#export DEBIAN_FRONTEND=noninteractive
#apt-get update
#apt-get install apt-transport-https \
#            ca-certificates \
#            curl \
#            software-properties-common -y

#curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
#add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

#apt-get update
#apt install docker-ce -y
#systemctl status docker
#groupadd docker && usermod -aG docker ${USER}
#SCRIPT

#######I added this scripts here#######

$bootstrap=<<SCRIPT
export REDHAT_FRONTEND=noninteractive
yum update
subscription-manager register –username=<username> –auto-attach
subscription-manager repos –enable=rhel-7-server-extras-rpms
subscription-manager repos –enable=rhel-7-server-optimal-rpms
yum update
reboot
yum install docker
systemctl start docker.service
systemctl enable docker.service
systemctl status docker.service

#######################################

$dockerir=<<SCRIPT
echo 'dockerir'
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "foobar-server" do |foobar|
    ###### I commented this line #######
    #foobar.vm.box = "bento/ubuntu-20.04"
    #####I added this line here too#####
    foobar.vm.box = "generic/rhel8"
    ####################################
    foobar.vm.hostname = "foobar-server"
    foobar.vm.network :private_network, ip: "192.168.33.10"
    foobar.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", 1024]
    end
    foobar.vm.provision :shell, inline: $bootstrap
    foobar.vm.provision :shell, inline: $dockerir
    foobar.vm.provision :docker
    foobar.vm.provision :docker_compose, yml: "/vagrant/services/wordpress/docker-compose.yml", rebuild: true, run: "always"
    foobar.vm.provision :docker
    foobar.vm.provision :docker_compose, yml: "/vagrant/services/laravel/docker-compose.yml", rebuild: true, run: "always"
  end
end
