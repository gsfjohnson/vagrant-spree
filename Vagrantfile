# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "gsfjohnson/centos67"
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
  end

  config.vm.provision "shell", inline: <<-SHELL
    sed -i "s|mirrorlist=|#mirrorlist=|; s|#baseurl=http://mirror.centos.org/|baseurl=http://mirrors.arsc.edu/|" /etc/yum.repos.d/CentOS-Base.repo
    echo "gem: --no-document" >/etc/gemrc
    cp /etc/gemrc .gemrc
    yum -y install ruby23 libxml2-devel libxslt-devel gcc zlib-devel git patch ImageMagick sqlite-devel nodejs
    gem install rails -v 4.2.5
    gem install bundler
    gem install spree_cmd
    if [ -d /vagrant/mystore ]; then
      ln -s /vagrant/mystore .
    else
      rails _4.2.5_ new mystore
      cd mystore
      echo >> Gemfile
      echo "gem 'spree', '3.0.5'" >>Gemfile
      echo "gem 'spree_gateway', github: 'spree/spree_gateway', branch: '3-0-stable'" >>Gemfile
      echo "gem 'spree_auth_devise', github: 'spree/spree_auth_devise', branch: '3-0-stable'" >>Gemfile
      bundle update
      echo 'Devise.secret_key = "`openssl rand -hex 50`"' >config/initializers/devise.rb
      spree install --auto-accept
      sed -i "s|^end$||"  config/environments/development.rb
      echo "  config.web_console.whitelisted_ips = '0.0.0.0/0'" >>config/environments/development.rb
      echo "end" >>config/environments/development.rb
      cd ..
      chown -R vagrant:vagrant mystore
    fi

  SHELL
end
