# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "gsfjohnson/centos71"
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum -y install libxml2-devel libxslt-devel rubygems ruby-devel rubygems-devel \
       gcc zlib-devel git patch ImageMagick sqlite-devel
    gem install rails -v 4.2.0
    gem install bundler
    gem install spree_cmd
    ln -s /vagrant/mystore .
  SHELL
end
