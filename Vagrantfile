# -*- mode: ruby -*-
# vi: set ft=ruby :
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Limitate the resources used by our VMs
  config.vm.provider "virtualbox" do |v|
    v.gui = false
    v.memory = 1024
    v.cpus = 2
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "70"]
  end
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "ubuntu/trusty64"
  # Multiple machines can be defined within the same project Vagrantfile
  # using the config.vm.define method call.
  config.vm.define "interview" do |vm_interview|
    # Update the packages
    vm_interview.vm.provision "shell", inline: <<SH
      # Add mongodb packages
      # http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/
      apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
      echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
      # Update packages
      apt-get -y update
SH
    # Install git, mongodb, nodejs
    vm_interview.vm.provision "shell", inline: <<SH
      # It is easiest to install Git on Linux using the preferred
      # package manager of your Linux distribution.
      # Debian/Ubuntu
      # $ apt-get install git
      apt-get -y install git nodejs nodejs-legacy npm mongodb-org
SH
    # Install create default folder and mongodb folders
    vm_interview.vm.provision "shell", inline: <<SH
      # Automatically chdir to vagrant directory upon “vagrant ssh”
      mkdir -p /datas/mongodb
      mkdir -p /home/vagrant
      echo "\n\ncd /home/vagrant\n" >> /home/vagrant/.bashrc
SH
    # Clone repo
    vm_interview.vm.provision "shell", inline: <<SH
      cd /home/vagrant
      git clone https://github.com/bert7/interview
      cd interview
      npm install
SH
    # startup mongodb on vm start
    vm_interview.vm.provision "shell", inline: <<SH
      echo "\n\nservice mongod start\n" >> /etc/rc.local
SH
    vm_interview.vm.network "forwarded_port", guest: 3000, host: 3000
    #  https://docs.vagrantup.com/v2/synced-folders/nfs.html
    vm_interview.vm.synced_folder ".", "/home/vagrant/"
  end
end
