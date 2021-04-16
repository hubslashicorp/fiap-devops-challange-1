# -- mode: ruby --
# vi: set ft=ruby :
require 'yaml'
yaml = YAML.load_file("core/machines.yml")

unless Vagrant.has_plugin?("vagrant-docker-compose")
  system("vagrant plugin install vagrant-docker-compose")
  puts "Dependencies installed, please try the command again."
  exit
end

Vagrant.configure("2") do |config|
  yaml.each do |server|
    config.vm.define server["name"] do |srv|
      srv.vm.box = server["sistema"]
      srv.vm.network "private_network", ip: server["ip"]
      srv.vm.hostname = server["hostname"]
      srv.vm.provider "virtualbox" do |vb|
        vb.name = server["name"]
        vb.memory = server["memory"]
        vb.cpus = server["cpus"]
      end
    config.vm.provision :shell, inline: "apt-get update"
    config.vm.provision :docker
    config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", run: "always"
  end
end

config.vm.provision "shell", inline: <<-SHELL

if [ $HOSTNAME = "mysql-lab" ]; then
  echo '########################################################'
  echo 'VALIDANDO VERSAO DO DOCKER-COMPOSE'
  echo '########################################################'
  echo '.'
  echo '.'
  docker-compose --version
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'LISTANDO CONTAINERS'
  echo '########################################################'
  echo '.'
  echo '.'
  sleep 4
  ls -lha
  docker-compose ps
  sleep 4
fi;
  date +"%H:%M:%S"
SHELL

end