# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  #  config.vm.synced_folder "{path on your local machine}", "{path on the vagrant box}"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    vb.name = "php5.3.29"
    # Display the VirtualBox GUI when booting the machine
    vb.customize ['modifyvm', :id, '--memory', '4096', '--cpus', '4', '--ioapic', 'on']
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install language-pack-en -y
    sudo apt-get install software-properties-common -y
    sudo apt-get upgrade python -y
    sudo apt-get install python-pip -y
    sudo apt-get install linux-headers-generic build-essential dkms -y

    # guest additions
    cd /tmp
    echo "downloading VBoxGuestAdditions_5.0.10.iso"
    wget -q http://download.virtualbox.org/virtualbox/5.0.10/VBoxGuestAdditions_5.0.10.iso
    sudo mkdir /media/VBoxGuestAdditions
    sudo mount -o loop,ro VBoxGuestAdditions_5.0.10.iso /media/VBoxGuestAdditions
    sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run <<-EOF
yes
EOF
    rm VBoxGuestAdditions_5.0.10.iso
    sudo umount /media/VBoxGuestAdditions
    sudo rmdir /media/VBoxGuestAdditions

    # php 5.3.29
    sudo rm -f /etc/apt/sources.list.d/dotdeb.list \
    && sudo rm -f /etc/apt/sources.list.d/squeeze.list \
    && gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-key AED4B06F473041FA \
    && gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-key 64481591B98321F9 \
    && gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-key E9C74FEEA2098A6E \
    && gpg -a --export AED4B06F473041FA | sudo apt-key add - \
    && gpg -a --export 64481591B98321F9 | sudo apt-key add - \
    && gpg -a --export E9C74FEEA2098A6E | sudo apt-key add - \
    && sudo echo "deb http://packages.dotdeb.org squeeze all" | sudo tee -a /etc/apt/sources.list.d/dotdeb.list \
    && sudo echo "deb http://httpredir.debian.org/debian squeeze main" | sudo tee -a /etc/apt/sources.list.d/squeeze.list \
    && sudo apt-get update \
    && sudo apt-get install -y --force-yes --no-install-recommends \
          php5-common=5.3.29-1~dotdeb.0 php5-cli=5.3.29-1~dotdeb.0 php5-curl=5.3.29-1~dotdeb.0 php5-intl=5.3.29-1~dotdeb.0 php5-mysql=5.3.29-1~dotdeb.0 \
          php-pear=5.3.29-1~dotdeb.0 php5-dev=5.3.29-1~dotdeb.0 \
    && sudo apt-get clean \
    && curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer

    # docker
    mkdir -p /home/vagrant/Projects
    sudo apt-get install htop -y
    sudo apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    sudo rm -f /etc/apt/sources.list.d/docker.list
    echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" | sudo tee -a /etc/apt/sources.list.d/docker.list
    sudo apt-get update
    sudo apt-get purge lxc-docker* -y
    sudo apt-get install linux-image-generic-lts-trusty -y
    sudo apt-get install docker-engine -y
    sudo usermod -aG docker vagrant
    sudo pip install -U docker-compose 
    sudo rm -f /etc/default/docker
    # echo 'DOCKER_OPTS="$DOCKER_OPTS --insecure-registry={your private docker hub host name}:5000"' | sudo tee -a /etc/default/docker
    sudo service docker restart

  SHELL

end
