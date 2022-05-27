# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64" # "ubuntu/jammy64" does not work - auth error in box
  config.vm.boot_timeout = 600
  config.vm.network "private_network", ip: "10.12.0.202", auto_correct: true
  config.vm.synced_folder "simple_tree", "/usr/share/nginx/simple_tree", owner: "www-data", group: "www-data"
  config.vm.synced_folder "deep_tree", "/usr/share/nginx/deep_tree", owner: "www-data", group: "www-data"
  config.vm.synced_folder "domain_tree", "/usr/share/nginx/domain_tree", owner: "www-data", group: "www-data"
  config.vm.synced_folder "config_nginx", "/etc/nginx-preset"
  config.vm.synced_folder "config_php", "/etc/php-preset"
  #config.ssh.username = 'vagrant'
  #config.ssh.password = 'vagrant'
  #config.ssh.insert_key = 'true'
  config.vm.provider "virtualbox" do |v|
    v.name = "Vagrant-LEMP"
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
    v.customize ["modifyvm", :id, "--memory", 2048]
    v.customize ["modifyvm", :id, "--cpus", 1]
  end

  if defined?(VagrantPlugins::HostsUpdater)
    config.vm.hostname = "lemp.test"
    config.hostsupdater.aliases = [
      "www.lemp.test"
    ]
  end

  config.vm.provision "shell", inline:<<-SHELL
    echo ""
    echo ""
    echo "======================================="
    echo "|   Initializing Vagrant LEMP Setup   |"
    echo "======================================="
    echo ""

    echo ""
    echo "Updating Linux ..."
    sudo apt-get install software-properties-common >/dev/null 2>&1
    sudo add-apt-repository ppa:ondrej/php >/dev/null 2>&1
    sudo apt-get -qq update >/dev/null 2>&1
    sudo apt-get -y upgrade >/dev/null 2>&1
    echo "... done updating Linux."


    echo ""
    echo "Installing common packages ..."
    sudo apt-get -y install nano vim curl build-essential python-software-properties net-tools git >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    echo "... done installing common packages."

    echo ""
    echo "Installing Nginx ..."
    sudo apt-add-repository -y ppa:nginx/development >/dev/null 2>&1
    sudo apt-get -y update >/dev/null 2>&1
    sudo rm /usr/share/nginx/html/index.html
    sudo apt-get -y install nginx >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    sudo chown www-data /usr/share/nginx/html -R >/dev/null 2>&1
    echo "... configuring Nginx ..."
    sudo systemctl enable nginx >/dev/null 2>&1
    sudo systemctl start nginx >/dev/null 2>&1
    # sudo systemctl status nginx
    echo "... done installing Nginx."

    echo ""
    echo "Installing Redis ..."
    sudo apt-get -y install redis >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    echo "... done installing Redis."

    echo ""
    echo "Installing SQLite ..."
    sudo apt-get -y install sqlite3 >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    # echo "... configuring and securing SQLite ..."
    # sudo systemctl enable sqlite3 >/dev/null 2>&1
    # sudo systemctl start sqlite3 >/dev/null 2>&1
    # sudo systemctl reload sqlite3 >/dev/null 2>&1
    # sudo systemctl status sqlite3
    echo "... done installing SQLite."

    echo ""
    echo "Installing MariaDB ..."
    sudo debconf-set-selections <<< "maria-db-server-10.1 mysql-server/root_password password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "maria-db-server-10.1 mysql-server/root_password_again password root" >/dev/null 2>&1
    sudo apt-get -y install mariadb-server >/dev/null 2>&1
    sudo apt-get -y -f install mariadb-client >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    echo "... configuring and securing MariaDB ..."
    sudo systemctl enable mysql >/dev/null 2>&1
    sudo systemctl start mysql >/dev/null 2>&1
    echo "DELETE FROM mysql.user WHERE User='';" | mysql -uroot -proot
    echo "DELETE FROM mysql.user WHERE User='root' AND Host NOT IN ('localhost', '127.0.0.1', '::1');" | mysql -uroot -proot
    echo "DROP DATABASE IF EXISTS test;" | mysql -uroot -proot
    echo "DELETE FROM mysql.db WHERE Db='test' OR Db='test\\_%';" | mysql -uroot -proot
    echo "FLUSH PRIVILEGES;" | mysql -uroot -proot
    sudo systemctl reload mysql >/dev/null 2>&1
    # sudo systemctl status mysql
    echo "... done installing MariaDB."

    echo ""
    echo "Installing PostgreSQL ..."
    sudo apt-get -y install postgresql-client postgresql-contrib >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    echo "... configuring and securing PostgreSQL ..."
    sudo systemctl enable postgresql >/dev/null 2>&1
    sudo systemctl start postgresql >/dev/null 2>&1
    # sudo systemctl status postgresql
    echo "... done installing PostgreSQL."

    echo ""
    echo "Installing PHP 7 ..."
    sudo apt-get -y install php7.4-fpm php7.4-mbstring php7.4-xml php7.4-xmlrpc php7.4-mysql php7.4-pgsql php7.4-sqlite3 php7.4-common php7.4-gd php7.4-cli php7.4-curl php7.4-intl php7.4-imagick php7.4-redis php7.4-zip php7.4-xdebug php7.4-mcrypt php7.4-memcache php7.4-msgpack php7.4-memcached php7.4-yaml php7.4-soap php7.4-json >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    sudo systemctl start php7.4-fpm >/dev/null 2>&1
    # sudo systemctl status php7.4-fpm
    echo "... configuring PHP 7 for Nginx ..."
    #sudo cp /vagrant/config/nginx-default.conf /etc/nginx/conf.d/default.conf >/dev/null 2>&1
    sudo nginx -t >/dev/null 2>&1
    sudo systemctl reload nginx >/dev/null 2>&1
    echo "... done installing PHP 7."

    echo ""
    echo "Installing PHP 8 ..."
    sudo apt-get -y install php8.1-fpm php8.1-mbstring php8.1-xml php8.1-xmlrpc php8.1-mysql php8.1-pgsql php8.1-sqlite3 php8.1-common php8.1-gd php8.1-cli php8.1-curl php8.1-intl php8.1-imagick php8.1-redis php8.1-zip php8.1-xdebug php8.1-mcrypt php8.1-memcache php8.1-msgpack php8.1-memcached php8.1-yaml php8.1-soap >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    sudo systemctl start php8.1-fpm >/dev/null 2>&1
    # sudo systemctl status php8.1-fpm
    echo "... configuring PHP 8 for Nginx ..."
    #sudo cp /vagrant/config/nginx-default.conf /etc/nginx/conf.d/default.conf >/dev/null 2>&1
    sudo nginx -t >/dev/null 2>&1
    sudo systemctl reload nginx >/dev/null 2>&1
    echo "... done installing PHP 8."

    echo ""
    echo "Setting up configuration ..."
    sudo mv /etc/nginx /etc/nginx-install
    sudo mv /etc/php /etc/php-install
    sudo cp -r /etc/nginx-preset /etc/nginx
    sudo cp -r /etc/php-preset /etc/php
    sudo nginx -t >/dev/null 2>&1
    sudo systemctl restart php7.4-fpm >/dev/null 2>&1
    sudo systemctl restart php8.1-fpm >/dev/null 2>&1
    sudo systemctl restart nginx >/dev/null 2>&1
    echo "... Configuration set."

    # echo ""
    # echo "Installing Composer ..."
    # sudo curl -sS https://getcomposer.org/installer | php >/dev/null 2>&1
    # sudo mv composer.phar /usr/local/bin/composer >/dev/null 2>&1
    # echo "... done installing Composer."

    # echo ""
    # echo "Installing PHPUnit ..."
    # sudo wget https://phar.phpunit.de/phpunit.phar >/dev/null 2>&1
    # sudo chmod +x phpunit.phar >/dev/null 2>&1
    # sudo mv phpunit.phar /usr/local/bin/phpunit >/dev/null 2>&1
    # echo "... done installing PHPUnit."

    echo ""
    echo "Installing phpMyAdmin ..."
    sudo debconf-set-selections <<< "maria-db-server-10.1 mysql-server/root_password password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "maria-db-server-10.1 mysql-server/root_password_again password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/dbconfig-install boolean true" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/app-password-confirm password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/admin-pass password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/app-pass password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/reconfigure-webserver multiselect none" >/dev/null 2>&1
    sudo apt-get -qq install phpmyadmin >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    echo "... configuring phpMyAdmin ..."
    sudo ln -sf /usr/share/phpmyadmin /usr/share/nginx/html/phpmyadmin >/dev/null 2>&1
    echo "GRANT ALL PRIVILEGES ON *.* TO 'phpmyadmin'@'localhost' WITH GRANT OPTION;" | mysql -uroot -proot
    echo "FLUSH PRIVILEGES;" | mysql -uroot -proot
    sudo systemctl reload mysql >/dev/null 2>&1
    echo "... done installing phpMyAdmin."

    echo ""
    echo "======================================="
    echo "|     Vagrant LEMP Setup Complete     |"
    echo "======================================="
    echo ""
    echo "http://lemp.test (10.12.0.202)"
    echo ""
    echo "Nginx - MariaDB - PostgreSQL - SQLite - Redis - PHP7 - PHP8"
    echo ""
    echo "phpMyAdmin"
    echo "http://lemp.test/phpmyadmin"
    echo "User: phpmyadmin"
    echo "Pass: root"
    echo ""
    echo "Hosts:"
    echo "# vagrant"
    echo "10.12.0.202	localdomain.local lemp.test"
    echo ""
    echo "======================================="
    echo ""
  SHELL
end
