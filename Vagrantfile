# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  #install image ubuntu xenial64
  config.vm.box = "ubuntu/xenial64"
 # shared folder from host to guest
  config.vm.synced_folder "./", "/home/vagrant"
   config.vm.provider "virtualbox" do |vb|
  # Customize the amount of memory on the VM:
     vb.memory = "2048"
   end
  # Apache HTTP Server
  config.vm.provision "shell", inline: <<-SHELL
    #update ppa
    apt-get update
    # Apache2 HTTP/S Server
    apt-get install -y apache2
    #PHP 
    apt-get install -y php libapache2-mod-php php-mysql
    #restart Apache2
    sudo /etc/init.d/apache2 restart
    #change directory to /var/www/html
    cd /var/www/html
    #clone github adminer in folder html
    wget https://github.com/vrana/adminer/releases/download/v4.3.1/adminer
    -4.3.1-mysql.php
    mv adminer-4.3.1-mysql.php adminer.php
    #install debconf-utils
    apt-get -y install debconf-utils
    #DB_ROOT_PASSWD is a variable to save passwd of user root (in this case the password root is root)
    DB_ROOT_PASSWD=root
    #change root password
    debconf-set-selections <<< "mysql-server mysql-server/root_password
    password $DB_ROOT_PASSWD"
    debconf-set-selections <<< "mysql-server mysql-server/root_password_again
    password $DB_ROOT_PASSWD"
    #install mysql server
    apt-get install -y mysql-server
    # authorize access localhost to everywhere
    sed -i -e 's/127.0.0.1/0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf
    #restart mysql
    /etc/init.d/mysql restart
    #change privilefes to all 
    mysql -uroot mysql -p$DB_ROOT_PASSWD <<< "GRANT ALL PRIVILEGES ON *.* TO
    root@'%' IDENTIFIED BY '$DB_ROOT_PASSWD'; FLUSH PRIVILEGES;"
    SHELL
end
