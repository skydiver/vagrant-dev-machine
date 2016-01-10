# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

    # Every Vagrant development environment requires a box. You can search for
    # boxes at https://atlas.hashicorp.com/search.
    config.vm.box = "ubuntu/trusty64"

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    # config.vm.network "forwarded_port", guest: 80, host: 8080

    # Create a private network, which allows host-only access to the machine using a specific IP.
    config.vm.network "private_network", ip: "192.168.33.10"
    #config.vm.network "public_network"

    # Share an additional folder to the guest VM. The first argument is
    # the path on the host to the actual folder. The second argument is
    # the path on the guest to mount the folder. And the optional third
    # argument is a set of non-required options.
    # config.vm.synced_folder "../data", "/vagrant_data"
    config.vm.synced_folder "/tmp", "/HOST/tmp"                 # SHARE LOCAL MACHINE /tmp
    config.vm.synced_folder '.', '/vagrant', disabled: true     # DISABLE /vagrant FOLDER

    # Enable provisioning with a shell script. Additional provisioners such as
    # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
    # documentation for more information about their specific syntax and use.
    config.vm.provision "shell", inline: <<-SHELL

        # CREATE SWAP MEMORY
        fallocate -l 1G /swapfile
        chmod 600 /swapfile
        mkswap /swapfile
        swapon /swapfile
        su -c "echo '/swapfile   none    swap    sw    0   0' >> /etc/fstab"

        # FIX SSH LOCALE
        sed -i -e 's/^\AcceptEnv LANG LC_*/#\AcceptEnv LANG LC_*/g' /etc/ssh/sshd_config
        service ssh restart

        # UPDATE PACKAGES LIST
        apt-get update

        # INSTALL UTILS
        apt-get install -y htop mc nano git

        # INSTALL LAMP SERVER
        debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'              # MySQL PASSWORD = root
        debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'        # MySQL PASSWORD = root
        apt-get install -y lamp-server^                                                                 # INSTALL LAMP PACKAGES

        # INSTALL EXTRA PHP LIBRARIES
        apt-get install -y php5-cli php5-curl php5-gd php5-json php5-mcrypt php5-sqlite

    SHELL

    # VIRTUALBOX CONFIG
    #config.vm.provider "virtualbox" do |v|
    #    v.name   = "vagrant-dev-machine"  # VM NAME
    #    v.cpus   = 2                      # VM CORES
    #    v.memory = 2048                   # VM MEMORY
    #end

end
