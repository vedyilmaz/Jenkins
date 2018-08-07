# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
plugins_dependencies = %w( vagrant-gatling-rsync vagrant-docker-compose vagrant-vbguest vagrant-docker-login )
plugin_status = false
plugins_dependencies.each do |plugin_name|
  unless Vagrant.has_plugin? plugin_name
    system("vagrant plugin install #{plugin_name}")
    plugin_status = true
    puts " #{plugin_name}  Dependencies installed"
  end
end

# Restart Vagrant if any new plugin installed
if plugin_status === true
  exec "vagrant #{ARGV.join' '}"
else
  puts "All Plugin Dependencies allready installed"
end

Vagrant.configure("2") do |config|
  class Username
        def to_s
            print "Username: " 
            STDIN.gets.chomp
        end
  end

  class Password
        def to_s
            begin
            system 'stty -echo'
            print "Password: "
            pass = URI.escape(STDIN.gets.chomp)
            ensure
            system 'stty echo'
            end
            pass
        end
    end

  config.vm.box = "ubuntu/xenial64"

  config.vm.provider "virtualbox" do |vb|
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    vb.name = "docker"
    vb.memory = "2048"
    vb.cpus= 1
  end

  config.vm.define "docker" do |m|
      m.vm.hostname = "docker"
     # m.vm.network "public_network" 
      m.vm.network "private_network", ip: "192.168.56.150"
  end

#   config.vm.provision :shell, inline: "apt-get update"
 #  config.vm.provision :docker
  # #config.vm.provision :docker_login, username: "vedyilmaz", password-stdin
  # config.vm.provision "shell", env: {"USERNAME" => "vedyilmaz", "PASSWORD" => Password.new}, inline: <<-SHELL
  #      # echo username: $USERNAME
  #      # echo password: $PASSWORD
  # SHELL

  #config.vm.provision :docker_compose, yml: ["/vagrant/docker-compose.yml"]

  config.vm.provision "ansible_local" do |ansible|
    ansible.become = true
    ansible.playbook = "playbook.yml"
    ansible.install_mode = "default"
  end
  # config.vm.network "public_network"
  
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
