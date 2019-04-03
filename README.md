# linux-server-configuration

A baseline installation of a Linux server

1. The IP address and SSH port so your server can be accessed by the reviewer.
    - IP address: 3.112.16.204
    - SSH port: 2200

2. The complete URL to your hosted web application.
    - complete URL: http://3.112.16.204/

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
    - Public key: 
        ```
        ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDJiNBwzjPIpqXEBwKrmA1kOBE2GGnMRn4/pRLa/Gn5uf5OJgHT6sYOgO/rKljXAtq0H30o611GGhHoQD3Y5J8jIJyxryi4X2EK2aFjCraZaOKX+Y2MFMErtiTRHiguxVLFoIufToY2eLvGfFDRQYPDmFvUiTVSFAo6uiJKUF3DNj666g/NK09s9pdeg2azcUClMa5L69fc04fDbywHoSwBXJ9DEvWAL6CD0apNSKj9PWYKHLF4AejZnOJMDt4TSvRkxm6VvZ0fWPb3zmBa8bDU6PkLfrxPwyBLv65w0wEGeg2H1V6WkQa7Cwk3Pub2o26GznCjuaqCFvPuaJ6lX9BJ ruochenxie@RuochendeMacBook-Pro.local
        ```
    - Private key: in the "Notes to Reviewer" field