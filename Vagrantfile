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
  # Download and Install the Latest Updates for the OS
  sudo apt-get update && apt-get upgrade -y
  # Set the Server Timezone to CST
  echo "America/Chicago" > /etc/timezone
  sudo dpkg-reconfigure -f noninteractive tzdata
  # Enable Ubuntu Firewall and allow SSH & MySQL Ports
  ufw enable
  ufw allow 22
  ufw allow 3306
  # Install essential packages
  sudo apt-get -y install zsh htop
  # Install MySQL Server in a Non-Interactive mode. Default root password will be "$MYSQL_ROOT_PASSWORD"
  echo "mysql-server-5.6 mysql-server/root_password password $MYSQL_ROOT_PASSWORD" | sudo debconf-set-selections
  echo "mysql-server-5.6 mysql-server/root_password_again password $MYSQL_ROOT_PASSWORD" | sudo debconf-set-selections
  sudo apt-get -y install mysql-server-5.6
  # Run the MySQL Secure Installation wizard
  mysql_secure_installation
  sed -i 's/127\.0\.0\.1/0\.0\.0\.0/g' /etc/mysql/my.cnf
  sudo mysql -uroot -p$MYSQL_ROOT_PASSWORD -e 'USE mysql; UPDATE `user` SET `Host`="%" WHERE `User`="root" AND `Host`="localhost"; DELETE FROM `user` WHERE `Host` != "%" AND `User`="root"; FLUSH PRIVILEGES;'
  sudo service mysql restart
  echo 'CRIANDO DATABASE'
  sudo mysql -uroot -p$MYSQL_ROOT_PASSWORD -e "CREATE DATABASE $MYSQL_DATABASE"
  echo 'PERMISSIONANDO USUARIO NA DATABASE'
  sudo mysql -uroot -p$MYSQL_ROOT_PASSWORD -e "GRANT ALL PRIVILEGES ON $MYSQL_DATABASE.* TO '$MYSQL_USER'@'localhost' IDENTIFIED BY '$MYSQL_PASSWORD'"
fi;
  date +"%H:%M:%S"
  sleep 5
SHELL

end