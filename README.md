# Vagrant LEMP

Vagrant box for PHP development with Linux, Nginx, MariaDB, PHP 7, and phpMyAdmin

## Prerequisites

### Required

It is assumed you have Virtual Box and Vagrant installed. If not, then grab
the latest version of each at the links below:

* [Virtual Box and Virtual Box Extension Pack](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](https://www.vagrantup.com/downloads.html)

### Recommended

Once Vagrant is installed, or if it already is, it's highly recommended
that you install the following Vagrant plugin:

* [vagrant-hostupdater](https://github.com/cogitatio/vagrant-hostsupdater)

  ```bash
  vagrant plugin install vagrant-hostsupdater
  ```

---

## What's Included

* Ubuntu Server v20.04 LTS (Focal Fossa) 64bit
* Nginx
* PHP 7.4
* PHP 8.1
* MariaDB
* PostgreSQL
* SQLite
* Redis
* phpMyAdmin

---

## Installation

The first time you clone the repo and bring the box up, it may take several
minutes. If it doesn't explicitly fail/quit, then it is still working.

```bash
git clone https://github.com/alex-kalanis/vagrant-lemp.git
cd vagrant-lemp && vagrant up
```

Once the Vagrant box finishes and is ready, you can verify PHP is working at
[http://lemp.test/test.php](http://lemp.test/test.php).

* phpMyAdmin: [http://lemp.dev/phpmyadmin](http://lemp.dev/phpmyadmin)
  * user: phpmyadmin
  * pass: root

---

## Sequel Pro & MySQL Workbench Access

To login to the server using Sequel Pro or Workbench just use 
* host: lemp.test
* user: ubuntu
* NO password
* default port (22)

as SSH details

For the MySQL details use  
* host: 127.0.0.1 
* user: phpmyadmin and 
* password: root

NB Sequel Pro users: Use the SSH tab

---

## Caveats

It's necessary to have default access visible in Vagrantfile. So for
production you shall clone the whole repository, remove these passwords
locally and only then use it in production! I have no responsibility
if you do not do that!

## License

The MIT License (MIT)

Copyright (c) 2016 Mike Sprague  
Copyright (c) 2017 Petr Bolehovsky  
Copyright (c) 2022 Petr Plsek  

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
