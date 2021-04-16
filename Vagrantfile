# -- mode: ruby --
# vi: set ft=ruby :
require 'yaml'
yaml = YAML.load_file("core/machines.yml")

USER = "slashicorp"
PASS = "@Fiap2tdst2021"
MYSQL_USER = "admdimdim"
MYSQL_ROOT_PASSWORD = "s8u7r$?eropAvuv"
MYSQL_PASSWORD = "@Fiap2tdst2021"
MYSQL_DATABASE = "db_fiap"

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
  end
end

config.vm.provision "shell", inline: <<-SHELL

if [ $HOSTNAME = "mysql-lab" ]; then
  echo '########################################################'
  echo 'ATUALIZANDO S.O'
  sudo apt update -y && sudo apt upgrade -y && sudo apt install wget -y
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'BAIXANDO SCRIPT DE INSTALACAO DO DOCKER'
  echo '########################################################'
  echo '.'
  echo '.'
  curl -fsSL https://get.docker.com -o get-docker.sh
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'EXECUTANDO SCRIPT DE INSTALACAO DOCKER'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo sh get-docker.sh
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'DANDO PREVILEGIOS AO GRUPO DOCKER PARA USUARIO VAGRANT'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo usermod -a -G docker vagrant
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'INICIANDO DOCKER'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo systemctl start docker
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'SETANDO DOCKER PARA INICIAR JUNTO COM O S.O'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo systemctl enable docker
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'REALIZANDO DOWNLOAD DA ULTIMA VERSAO DE DOCKER-COMPOSE'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  echo '.'
  echo '.'
  echo '########################################################'  
  echo 'APLICANDO PERMISSOES DE EXECUTAVEL PARA O SH'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo chmod +x /usr/local/bin/docker-compose
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'CRIANDO LINK SIMBOLICO PARA O BINARIO DO DOCKER-COMPOSE'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'VALIDANDO VERSAO DO DOCKER-COMPOSE'
  echo '########################################################'
  echo '.'
  echo '.'
  docker-compose --version
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'APLICANDO NOSSO DOCKER-COMPOSE'
  echo '########################################################'
  echo '.'
  echo '.'
  docker-compose up -d
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'LISTANDO CONTAINERS'
  echo '########################################################'
  echo '.'
  echo '.'
  docker-compose ps
fi;
  date +"%H:%M:%S"
  sleep 5
SHELL

end