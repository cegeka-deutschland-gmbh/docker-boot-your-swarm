# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"
  config.vm.box_version = "1804.02"


  config.vm.define "docker#1" do |docker1|
    docker1.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
    docker1.vm.hostname = "docker1"
    docker1.vm.network "private_network", ip: "192.168.50.11"
    #docker1.vm.synced_folder "jenkins/", "/var/lib/jenkins/", create: true, mount_options: ['fmode=0777','dmode=0777']
    docker1.vm.provision :shell, inline: <<-SHELL
    yum install -y wget java-1.8.0-openjdk-devel
    wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
    rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
    yum install -y jenkins git
    systemctl enable jenkins
    systemctl start jenkins
    usermod -a -G docker jenkins
    systemctl restart jenkins
    curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
    chmod +x /usr/bin/docker-compose
    SHELL
    docker1.vm.network "forwarded_port", guest: 8080, host:8080
  end

  config.vm.define "docker#2" do |docker2|
    docker2.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    docker2.vm.network "private_network", ip: "192.168.50.12"
    docker2.vm.hostname = "docker2"
  end

  config.vm.define "docker#3" do |docker3|
    docker3.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    docker3.vm.network "private_network", ip: "192.168.50.13"
    docker3.vm.hostname = "docker3"
  end

  config.vm.define "registry" do |registry|
    registry.vm.network "private_network", ip: "192.168.50.14"
    registry.vm.hostname = "registry"
    registry.vm.provision :shell, inline: "docker run -d -p 5000:5000 --restart=always --name registry registry:2"
    registry.vm.network "forwarded_port", guest: 5000, host:5000
  end

  config.vm.provision :hosts do |provisioner|
    # Add a single hostname
    provisioner.add_host '192.168.50.11', ['docker1']
    provisioner.add_host '192.168.50.12', ['docker2']
    provisioner.add_host '192.168.50.13', ['docker3']
    provisioner.add_host '192.168.50.14', ['registry']
  end
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    yum update
    yum install -y yum-utils device-mapper-persistent-data lvm2
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install -y docker-ce
    systemctl enable docker
    systemctl start docker
    SHELL
  config.vm.provision "file", source: "daemon.json", destination: "daemon.json"
  config.vm.provision "shell" do |s|
    s.inline = "cp daemon.json /etc/docker/daemon.json"
    s.privileged = true
  end
  config.vm.provision "shell", inline: <<-SHELL
    systemctl restart docker
    SHELL
end
