# Parameters, edit here:

static_ip           = '192.168.22.10'
box_name            = 'devilbox'
cpus                = 2
memory              = 2048
path_server         = '/vagrant/server/'
docker_compose_args = '-d --force-recreate'
# docker_compose_args = '-d --force-recreate bind httpd php mysql'

Vagrant.configure("2") do |config|
  # Box
  config.vm.box = "ubuntu/trusty64"

  # Network
  config.vm.network :private_network, ip: static_ip
  config.vm.network :forwarded_port, guest: 80, host: 80
  config.vm.network :forwarded_port, guest: 1053, host: 1053
  config.vm.network :forwarded_port, guest: 3306, host: 3306
  config.vm.network :forwarded_port, guest: 9000, host: 9000
  config.vm.network :forwarded_port, guest: 5432, host: 5432
  config.vm.network :forwarded_port, guest: 6379, host: 6379
  config.vm.network :forwarded_port, guest: 11211, host: 11211
  config.vm.network :forwarded_port, guest: 27017, host: 27017

  # Configure VM
  config.vm.provider :virtualbox do |vb|
    vb.name = box_name
    vb.customize ["modifyvm", :id, "--cpus", cpus]
    vb.customize ["modifyvm", :id, "--memory", memory]

    config.vm.synced_folder ".", "/vagrant/server", id: "core"
    # config.bindfs.bind_folder "/vagrant", "/home/vagrant/server"
  end

  # Docker
  config.vm.provision :docker
  config.vm.provision :docker_compose

  # Boot script
  config.vm.provision "shell", path: "./vagrant/scripts/boot.sh", args: [path_server, docker_compose_args]
end
