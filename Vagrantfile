# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"

$ubootstrap=<<SCRIPT
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
#groupadd docker && usermod -aG docker ${USER}
SCRIPT


$fbootstrap=<<SCRIPT
dnf -y install dnf-plugins-core
dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
dnf -y install docker-ce docker-ce-cli containerd.io
systemctl start docker
docker run hello-world
SCRIPT


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    
    if "#{O}" == 'ubuntu' then
      config.vm.define "foobar-server" do |foobar|
      foobar.vm.box = "bento/ubuntu-20.04"
      end
    else
      config.vm.define "foobar-server" do |foobar|
      foobar.vm.box = "fedora/32-cloud-base"
      end
    end
  end

  foobar.vm.hostname = "foobar-server"
  foobar.vm.network :private_network, ip: "192.168.33.10"
  foobar.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: ".git/", disabled: false
  foobar.vm.provider "virtualbox" do |vb|
   vb.customize ["modifyvm", :id, "--memory", 1024]
  end

  if "#{O}" == 'ubuntu' then
    foobar.vm.provision :shell, inline: $ubootstrap, privileged: true
    end
  else
    foobar.vm.provision :shell, inline: $fbootstrap, privileged: true
    end
  end
  foobar.vm.provision :docker
  foobar.vm.provision :docker_compose, yml: "/vagrant/services/wordpress/docker-compose.yml", rebuild: true, run: "always"
  foobar.vm.provision :docker
  foobar.vm.provision :docker_compose, yml: "/vagrant/services/laravel/docker-compose.yml", rebuild: true, run: "always"
end
end
