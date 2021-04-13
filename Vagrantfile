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
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'Download and Install the Latest Updates for the OS'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo apt-get update && apt-get upgrade -y
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'Set the Server Timezone to CST'
  echo '########################################################'
  echo '.'
  echo '.'
  echo "America/Chicago" > /etc/timezone
  sudo dpkg-reconfigure -f noninteractive tzdata
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'Habilitando Ubuntu Firewall e liberando SSH & MySQL Ports'
  echo '########################################################'
  echo '.'
  echo '.'
  ufw enable
  ufw allow 22
  ufw allow 3306
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'Instalando packages essenciais'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo apt-get -y install zsh htop
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'Instalando MySQL Server em Non-Interactive mode. Default root password sera var.MYSQL_ROOT_PASSWORD'
  echo '########################################################'  
  echo '.'
  echo '.'
  echo "mysql-server-5.6 mysql-server/root_password password $MYSQL_ROOT_PASSWORD" | sudo debconf-set-selections
  echo "mysql-server-5.6 mysql-server/root_password_again password $MYSQL_ROOT_PASSWORD" | sudo debconf-set-selections
  sudo apt-get -y install mysql-server-5.6
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'Executando MySQL Secure Installation wizard'
  echo '########################################################'
  echo '.'
  echo '.'
  mysql_secure_installation
  sed -i 's/127\.0\.0\.1/0\.0\.0\.0/g' /etc/mysql/my.cnf
  sudo mysql -uroot -p$MYSQL_ROOT_PASSWORD -e 'USE mysql; UPDATE `user` SET `Host`="%" WHERE `User`="root" AND `Host`="localhost"; DELETE FROM `user` WHERE `Host` != "%" AND `User`="root"; FLUSH PRIVILEGES;'
  sudo service mysql restart
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'CRIANDO DATABASE'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo mysql -uroot -p$MYSQL_ROOT_PASSWORD -e "CREATE DATABASE $MYSQL_DATABASE"
  echo '.'
  echo '.'
  echo '########################################################'
  echo 'PERMISSIONANDO USUARIO NA DATABASE'
  echo '########################################################'
  echo '.'
  echo '.'
  sudo mysql -uroot -p$MYSQL_ROOT_PASSWORD -e "GRANT ALL PRIVILEGES ON $MYSQL_DATABASE.* TO '$MYSQL_USER'@'localhost' IDENTIFIED BY '$MYSQL_PASSWORD'"
fi;
  date +"%H:%M:%S"
  sleep 5
SHELL

end