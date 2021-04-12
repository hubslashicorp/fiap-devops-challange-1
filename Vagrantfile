# -- mode: ruby --
# vi: set ft=ruby :
require 'yaml'
yaml = YAML.load_file("core/machines.yml")

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
  echo 'BAIXANDO ATUALIZACOES PARA O REPO LOCAL'
  sudo apt-get update
  echo 'INSTALANDO PACOTE DO MYSQL-SERVER'
  sudo apt-get install mysql-server -y
  echo 'INICIANDO MYSQL-SERVER'
  sudo systemctl start mysql
  echo 'SETANDO MYSQL PARA INICIAR JUNTO COM O S.O'
  sudo systemctl enable mysql
  echo 'CONFIGURAÇÕES INICIAIS DO MYSQL INLINE, ALTERANDO SENHA DO ROOT'
  sudo mysql -e "UPDATE mysql.user SET Password = PASSWORD('$MYSQL_ROOT_PASSWORD') WHERE User = 'root'"
  echo 'REMOVENDO ACESSO DO USUARIO ANONIMO CRIADO'
  sudo mysql -e "DROP USER ''@'localhost'"
  echo 'REMOVENDO USUARIO DEFAULT'
  sudo mysql -e "DROP USER ''@'$(hostname)'"
  echo 'REMOVENDO DATABASE TEST'
  sudo mysql -e "DROP DATABASE test"
  echo 'LIMPANDO PREVILEGIOS DEFAULT'
  sudo mysql -e "FLUSH PRIVILEGES"
  echo 'CRIANDO DATABASE'
  sudo mysql -e "CREATE DATABASE $MYSQL_DATABASE"
  echo 'PERMISSIONANDO USUARIO NA DATABASE'
  sudo mysql -e "GRANT ALL PRIVILEGES ON $MYSQL_DATABASE.* TO '$MYSQL_USER'@'localhost' IDENTIFIED BY '$MYSQL_PASSWORD'"
fi;
  date +"%H:%M:%S"
  sleep 5
SHELL

end