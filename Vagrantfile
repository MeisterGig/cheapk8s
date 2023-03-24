# -*- mode: ruby -*-
# vi: set ft=ruby :

# Drew's note: This is a Vagrantfile used to stand up a Kubernetes cluster for
# local experimentation and learning. To find out more, see this repository:
#
# https://github.com/hybby/cheapk8s

# -----------------------------------------------------
# CUSTOMISE THE BELOW IF REQUIRED
# -----------------------------------------------------
box = "ubuntu/focal64"  # CKE says Ubuntu 20.04, so who am I to argue?
cpus = "2"
memory = "7500"

master_ip_addr = "10.3.0.175"
worker1_ip_addr = "10.3.0.176"
worker2_ip_addr = "10.3.0.177"

# chapter 16 - extra nodes - uncomment when needed (chapter 16)
# lb_ip_addr = "10.0.0.180"
# cp2_ip_addr = "10.0.0.181"
# cp3_ip_addr = "10.0.0.182"

# ------------------
# END CUSTOMISATIONS
# ------------------

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.boot_timeout = 900
  config.vm.synced_folder '.', '/vagrant', disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = memory
    vb.cpus = cpus
    vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end

  # controlplane
  config.vm.define "master" do |server|
    server.vm.box = box
    server.vm.hostname = "master"
    server.vm.post_up_message = "master is up"
    server.vm.network :private_network, ip: master_ip_addr

    # kubernetes api server port
    server.vm.network "forwarded_port", guest: 6443, host: 6443

    server.vm.provision "shell", inline: <<-SHELL
      echo "this is my kubernetes master"
    SHELL
  end

  # more controlplanes and an haproxy node - uncomment when needed (chapter 16)
  #config.vm.define "cp2" do |server|
  #  server.vm.box = box
  #  server.vm.hostname = "cp2.example.com"
  #  server.vm.post_up_message = "cp2 is up"
  #  server.vm.network :private_network, ip: cp2_ip_addr

  #  # kubernetes api server port
  #  server.vm.network "forwarded_port", guest: 6443, host: 6444

  #  server.vm.provision "shell", inline: <<-SHELL
  #    echo "this is my kubernetes controlplane 2"
  #  SHELL
  #end

  #config.vm.define "cp3" do |server|
  #  server.vm.box = box
  #  server.vm.hostname = "cp3.example.com"
  #  server.vm.post_up_message = "cp3 is up"
  #  server.vm.network :private_network, ip: cp3_ip_addr

  #  # kubernetes api server port
  #  server.vm.network "forwarded_port", guest: 6443, host: 6445

  #  server.vm.provision "shell", inline: <<-SHELL
  #    echo "this is my kubernetes controlplane 3"
  #  SHELL
  #end

  #config.vm.define "lb" do |server|
  #  server.vm.box = box
  #  server.vm.hostname = "lb.example.com"
  #  server.vm.post_up_message = "lb is up"
  #  server.vm.network :private_network, ip: lb_ip_addr

  #  # kubernetes api server port
  #  server.vm.network "forwarded_port", guest: 9443, host: 9443
  #  server.vm.network "forwarded_port", guest: 9999, host: 9999

  #  server.vm.provision "shell", inline: <<-SHELL
  #    echo "this is my kubernetes load balancer"
  #  SHELL
  #end

  # workers
  config.vm.define "worker1" do |server|
    server.vm.box = box
    server.vm.hostname = "worker1"
    server.vm.post_up_message = "worker1 is up"
    server.vm.network :private_network, ip: worker1_ip_addr

    server.vm.provision "shell", inline: <<-SHELL
      echo "this is my kubernetes worker (number 1)"
    SHELL
  end

  config.vm.define "worker2" do |server|
    server.vm.box = box
    server.vm.hostname = "worker2"
    server.vm.post_up_message = "worker2 is up"
    server.vm.network :private_network, ip: worker2_ip_addr

    server.vm.provision "shell", inline: <<-SHELL
      echo "this is my kubernetes worker (number 2)"
    SHELL
  end
end
