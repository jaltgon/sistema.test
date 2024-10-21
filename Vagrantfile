# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  config.vm.box = "debian/bookworm64"

  config.vm.define :master do |master|
    master.vm.network "private_network", ip: "192.168.57.103"
    master.vm.hostname = "tierra.sistema.test"

    master.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y bind9 dnsutils
    SHELL

    master.vm.provision "shell", name: "master-dns", inline: <<-SHELL
      cp -v /vagrant/named /etc/default
      cp -v /vagrant/named.conf.options /etc/bind
      cp -v /vagrant/named.conf.local /etc/bind
      cp -v /vagrant/dns.sri /var/lib/bind
      sudo systemctl restart bind9
    SHELL
  end

  config.vm.define "slave" do |slave|
    slave.vm.network "private_network", ip: "192.168.57.102"
    slave.vm.hostname = "venus.sistema.test"

    slave.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y bind9 dnsutils
    SHELL

    slave.vm.provision "shell", name: "slave-dns", inline: <<-SHELL
      cp -v /vagrant/named.conf.slave /etc/default/named.conf
      cp -v /vagrant/named.conf.local.slave /etc/bind/named.conf.local
      cp -v /vagrant/named.conf.options.slave /etc/bind/named.conf.options
      sudo systemctl restart bind9
    SHELL
  end
  
end