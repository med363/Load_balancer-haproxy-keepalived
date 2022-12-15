# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
#definit argument
  haproxycfg= <<-SHELL
  apt-get update
  apt-get install -y haproxy
  apt-get install -y keepalived
                                                       
  SHELL

#2 box pour lb dans le but de HA (keepalived entant que master et dans l'autre HAproxy en tant que client(backup) et le end user se connecte avec une adress virtuelle permet balayer entre les deux box HAproxy dans le cas si le box haproxy master epuise
  config.vm.define "lb1" do |lb1|
  lb1.vm.box = "ubuntu/xenial64"
  lb1.vm.network "private_network", ip: "192.168.56.10"
  lb1.vm.network "public_network"
  lb1.vm.hostname = "lb1"
  lb1.vm.provision :shell, :inline => haproxycfg
  end
 
  config.vm.define "lb2" do |lb2|
  lb2.vm.box = "ubuntu/xenial64"
  lb2.vm.network "private_network", ip: "192.168.56.11"
  lb2.vm.network "public_network"
  lb2.vm.hostname = "lb2"
  lb2.vm.provision :shell, :inline => haproxycfg
  end 

  config.vm.define "web1" do |web1|
  web1.vm.box = "ubuntu/xenial64"
  web1.vm.network "private_network", ip: "192.168.56.12"
#  web1.vm.synced_folder "c:/flat", "/var/www/html"
  web1.vm.provision "shell", inline: <<-SHELL
     echo "web1" > /etc/hostname
     apt-get update
     apt-get install -y apache2
     echo "<H1>Hello from Server web1</H1>" | tee /var/www/html/index.html
  SHELL
  end

  config.vm.define "web2" do |web2|
  web2.vm.box = "ubuntu/xenial64"
  web2.vm.network "private_network", ip: "192.168.56.13"
#  web2.vm.synced_folder "c:/flat", "/var/www/html"
  web2.vm.provision "shell", inline: <<-SHELL
     echo "web2" > /etc/hostname
     apt-get update
     apt-get install -y apache2
     echo "<H1>Hello from Server web2</H1>" | tee /var/www/html/index.html
  SHELL
  end  
  config.vm.box = "base"
 end
