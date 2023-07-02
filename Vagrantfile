# -*- mode: ruby -*-
# vi: set ft=ruby :
# To get Vagrand and Virtualbox working on Fedora 38 I needed the most recent version:
#  sudo dnf install ~/Downloads/VirtualBox-7.0-7.0.8_156879_fedora36-1.x86_64.rpm
#  sudo dnf install ~/Downloads/vagrant-2.3.6-1.x86_64.rpm

# There was an error message from Vagrant about version compatibility,
# this env var was recommended.  Maybe it is not needed since updating
# to the most recent versions of Vagrant and Virtualbox (see above), but
# I am leaving the env var specified.
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

Vagrant.configure("2") do |config|

  config.vm.define "clickhouse-01" do |node|
    node.vm.box = "debian/bookworm64"
    node.vm.hostname = "clickhouse-01"
    # the next two lines need `vagrant plugin install vagrant-hosts`
    node.vm.network :private_network, :ip => '192.168.56.2'
    node.vm.provision :hosts, :sync_hosts => true
    node.vm.provider "virtualbox" do |vb|
      #vb.gui = true
      vb.memory = "3072"
      # mount the dir containing the Vagrantfile as /vagrant in VM
      share_home=true
      enable_nfs = true
    end

    # install some packages at VM build, I will update this
    # to install ClickHouse Server or Keeper as needed
    node.vm.provision "shell", inline: <<-SHELL
      /vagrant/mountedfiles/server.sh
    SHELL
  end # end clickhouse-01

  config.vm.define "clickhouse-02" do |node|
    node.vm.box = "debian/bookworm64"
    node.vm.hostname = "clickhouse-02"
    # the next two lines need `vagrant plugin install vagrant-hosts`
    node.vm.network :private_network, :ip => '192.168.56.3'
    node.vm.provision :hosts, :sync_hosts => true
    node.vm.provider "virtualbox" do |vb|
      #vb.gui = true
      vb.memory = "3072"
      # mount the dir containing the Vagrantfile as /vagrant in VM
      share_home=true
      enable_nfs = true
    end

    # install some packages at VM build, I will update this
    # to install ClickHouse Server or Keeper as needed
    node.vm.provision "shell", inline: <<-SHELL
      /vagrant/mountedfiles/server.sh
    SHELL
  end # end clickhouse-02

  config.vm.define "clickhouse-keeper-01" do |node|
    node.vm.box = "debian/bookworm64"
    node.vm.hostname = "clickhouse-keeper-01"
    # the next two lines need `vagrant plugin install vagrant-hosts`
    node.vm.network :private_network, :ip => '192.168.56.4'
    node.vm.provision :hosts, :sync_hosts => true
    node.vm.provider "virtualbox" do |vb|
      #vb.gui = true
      vb.memory = "2048"
      # mount the dir containing the Vagrantfile as /vagrant in VM
      share_home=true
      enable_nfs = true
    end

    # install some packages at VM build, I will update this
    # to install ClickHouse Server or Keeper as needed
    node.vm.provision "shell", inline: <<-SHELL
      /vagrant/mountedfiles/keeper.sh
    SHELL
  end # end keeper-01

  config.vm.define "clickhouse-keeper-02" do |node|
    node.vm.box = "debian/bookworm64"
    node.vm.hostname = "clickhouse-keeper-02"
    # the next two lines need `vagrant plugin install vagrant-hosts`
    node.vm.network :private_network, :ip => '192.168.56.5'
    node.vm.provision :hosts, :sync_hosts => true
    node.vm.provider "virtualbox" do |vb|
      #vb.gui = true
      vb.memory = "2048"
      # mount the dir containing the Vagrantfile as /vagrant in VM
      share_home=true
      enable_nfs = true
    end

    # install some packages at VM build, I will update this
    # to install ClickHouse Server or Keeper as needed
    node.vm.provision "shell", inline: <<-SHELL
      /vagrant/mountedfiles/keeper.sh
    SHELL
  end # end keeper-02

  config.vm.define "clickhouse-keeper-03" do |node|
    node.vm.box = "debian/bookworm64"
    node.vm.hostname = "clickhouse-keeper-03"
    # the next two lines need `vagrant plugin install vagrant-hosts`
    node.vm.network :private_network, :ip => '192.168.56.6'
    node.vm.provision :hosts, :sync_hosts => true
    node.vm.provider "virtualbox" do |vb|
      #vb.gui = true
      vb.memory = "2048"
      # mount the dir containing the Vagrantfile as /vagrant in VM
      share_home=true
      enable_nfs = true
    end

    # install some packages at VM build, I will update this
    # to install ClickHouse Server or Keeper as needed
    node.vm.provision "shell", inline: <<-SHELL
      /vagrant/mountedfiles/keeper.sh
    SHELL
  end # end keeper-03

end
