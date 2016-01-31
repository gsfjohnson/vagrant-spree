# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "gsfjohnson/centos67"
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
  end

  config.vm.provision "shell", inline: <<-SHELL
    echo "gem: --no-document" >/etc/gemrc
    cp /etc/gemrc .gemrc
    #yum -y install libxml2-devel libxslt-devel rubygems ruby-devel rubygems-devel \
    #   gcc zlib-devel git patch ImageMagick sqlite-devel
    yum -y install ruby23 libxml2-devel libxslt-devel gcc zlib-devel git patch ImageMagick sqlite-devel
    gem install rails -v 4.2.5
    gem install bundler
    gem install spree_cmd
    if [ -d /vagrant/mystore ]; then
      ln -s /vagrant/mystore .
    else
      rails _4.2.5_ new mystore
      cd mystore
      spree install --auto-accept
      bundle update
    fi
  SHELL
end
