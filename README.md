# linux-server-configuration

A baseline installation of a Linux server

1. The IP address and SSH port so your server can be accessed by the reviewer.
    - IP address: 3.112.16.204
    - SSH port: 2200

2. The complete URL to your hosted web application.
    - complete URL: http://xieruochen.info/

3. Software you installed and Configuration changes made.
    - software you installed: Apache and Wsgi
    - configuration changes: 
        - apache:
            ```
            <VirtualHost *:80>
                    ServerName http://3.112.16.204:5000/
                    ServerAdmin admin@mywebsite.com
                    WSGIScriptAlias / /var/www/FlaskApp/simple.wsgi
                    <Directory /var/www/FlaskApp/Simple/>
                            Order allow,deny
                            Allow from all
                    </Directory>
                    <Directory /var/www/FlaskApp/FlaskApp/>
                            Order allow,deny
                            Allow from all
                    </Directory>
                    Alias /static /var/www/FlaskApp/Simple/static
                    <Directory /var/www/FlaskApp/Simple/static/>
                            Order allow,deny
                            Allow from all
                    </Directory>
                    ErrorLog ${APACHE_LOG_DIR}/error.log
                    LogLevel warn
                    CustomLog ${APACHE_LOG_DIR}/access.log combined
            </VirtualHost>
            ```
        - wsgi: 
            ```
            #!/usr/bin/python
            import sys
            import logging
            logging.basicConfig(stream=sys.stderr)
            sys.path.insert(0,"/var/www/FlaskApp/")
            
            from Simple import app as application
            application.secret_key = 'Add your secret key'
            ```
        
4. Locate the SSH key you created for the grader user.
    - Public key which is putted in .ssh/authorized_keys: 
        ```
        ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDJiNBwzjPIpqXEBwKrmA1kOBE2GGnMRn4/pRLa/Gn5uf5OJgHT6sYOgO/rKljXAtq0H30o611GGhHoQD3Y5J8jIJyxryi4X2EK2aFjCraZaOKX+Y2MFMErtiTRHiguxVLFoIufToY2eLvGfFDRQYPDmFvUiTVSFAo6uiJKUF3DNj666g/NK09s9pdeg2azcUClMa5L69fc04fDbywHoSwBXJ9DEvWAL6CD0apNSKj9PWYKHLF4AejZnOJMDt4TSvRkxm6VvZ0fWPb3zmBa8bDU6PkLfrxPwyBLv65w0wEGeg2H1V6WkQa7Cwk3Pub2o26GznCjuaqCFvPuaJ6lX9BJ ruochenxie@RuochendeMacBook-Pro.local
        ```
    - Private key: 
        - For Udacity reviewer: in the "Notes to Reviewer" field
        - For common readers: you can email to me to get the Private key.
    
5. Essential software/packages
    - the base package shell(directly copy from Udacity Linux class)
        ```
        apt-get -qqy update

        # Work around https://github.com/chef/bento/issues/661
        # apt-get -qqy upgrade
        DEBIAN_FRONTEND=noninteractive apt-get -y -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" upgrade
    
        apt-get -qqy install make zip unzip postgresql
    
        apt-get -qqy install python3 python3-pip
        pip3 install --upgrade pip
        pip3 install flask packaging oauth2client redis passlib flask-httpauth
        pip3 install sqlalchemy flask-sqlalchemy psycopg2-binary bleach requests
    
        apt-get -qqy install python python-pip
        pip2 install --upgrade pip
        pip install flask packaging oauth2client redis passlib flask-httpauth
        pip install sqlalchemy flask-sqlalchemy psycopg2-binary bleach requests
    
        su postgres -c 'createuser -dRS vagrant'
        su vagrant -c 'createdb'
        su vagrant -c 'createdb news'
        su vagrant -c 'createdb forum'
        su vagrant -c 'psql forum -f /vagrant/forum/forum.sql'
    
        vagrantTip="[35m[1mThe shared directory is located at /vagrant\\nTo access your shared files: cd /vagrant[m"
        echo -e $vagrantTip > /etc/motd
    
        wget http://download.redis.io/redis-stable.tar.gz
        tar xvzf redis-stable.tar.gz
        cd redis-stable
        make
        make install
        ```
    - install Apache:
        - sudo apt-get install apache2
    - the deployment essential:
        - sudo apt-get install libapache2-mod-wsgi python-dev
        - sudo pip install virtualenv (optimal)
        - sudo pip install Flask
    - dealing with [MOTD](https://serverfault.com/questions/262751/update-ubuntu-10-04/262773#262773):
        - sudo apt-get install aptitude
    
6. Resource
    - Deploy flask on Ubuntu VPS: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
        - The step three should be done in the directory of /var/www/FlaskApp/FlaskApp
        - In the step four, the configuration should specify port which is 5000 in the example.
    - The directory problem: http://help.pythonanywhere.com/pages/NoSuchFileOrDirectory
    - Importerror: https://stackoverflow.com/questions/25026326/importerror-cannot-import-name-app
