# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.99.10"

  config.vm.synced_folder "../data", "/home/vagrant/data"

  config.vm.provider "virtualbox" do |v|
    v.customize ["setextradata", "sandbox", "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    v.name = "sandbox"
    v.memory = "1024"

  end

   config.vm.provision "shell", inline: <<-SHELL
    yum install epel-release -y
    yum install https://centos7.iuscommunity.org/ius-release.rpm -y
    yum update
    yum install ansible git python36u python36u-libs python36u-devel python36u-pip -y
    mkdir -p /vagrant/provisioning/roles
    ansible-galaxy install -r /vagrant/provisioning/requirements.yml -p /vagrant/provisioning/roles --force
    cp -R /vagrant/config/* /vagrant/provisioning/roles/kubectl_role/files/
    chown -R vagrant:vagrant /vagrant/provisioning/roles
    ansible-playbook -i /vagrant/provisioning/inventory /vagrant/provisioning/playbook.yml
   SHELL

end
