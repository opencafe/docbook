# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"
$bootstrap=<<SCRIPT
dnf -y install dnf-plugins-core
dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
dnf -y install docker-ce docker-ce-cli containerd.io
systemctl start docker
docker run hello-world
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "foobar-server" do |foobar|
    foobar.vm.box = "fedora/32-cloud-base"
    foobar.vm.hostname = "foobar-server"
    foobar.vm.network :private_network, ip: "192.168.33.10"
    foobar.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: ".git/", disabled: false
    foobar.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", 1024]
    end
    foobar.vm.provision :shell, inline: $bootstrap, privileged: true
    foobar.vm.provision :docker
    foobar.vm.provision :docker_compose, yml: "/vagrant/services/wordpress/docker-compose.yml", rebuild: true, run: "always"
    foobar.vm.provision :docker
    foobar.vm.provision :docker_compose, yml: "/vagrant/services/laravel/docker-compose.yml", rebuild: true, run: "always"
  end
end
