## Instalar Homebrew

**Opción 1**

    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

**Opción 2**

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

**Añadimos Tap Brew**

    brew tap homebrew/core

**Auto-diagnóstico**

    brew doctor

## Instalar Apache
**Detenemos Apache por defecto de MacOS**

    sudo apachectl stop
    sudo launchctl unload /System/Library/LaunchDaemons/org.apache.httpd.plist 2>/dev/null

**Instalar httpd**

    brew install httpd

**Agregamos el servicio para auto-arranque**

    sudo brew services start httpd

## Instalar PHP

    brew install php


## Instalar MySQL

    brew install mysql

**Agregamos el servicio para auto-arranque**

    brew services start mysql

**Agregamos contraseña al usuario Root**

    mysqladmin -u root password 'tucontraseña'

## Instalar PHPMyAdmin

    brew install phpmyadmin


## Configurar Apache
Realizar un Backup httpd.conf, luego editamos 

    vi /usr/local/etc/httpd/httpd.conf

Desmarcamos y modificamos `ServerName`
  
    ServerName localhost

Modificamos el puerto de escucha de 8080 a 80

    Listen 80

Agregarmos la dirección de la carpeta web. Ejemplo:

    DocumentRoot "/users/tu-usuario/www"
    <Directory "/users/tu-usuario/www">
    	Options Indexes FollowSymLinks Includes ExecCGI
            AllowOverride All
            Require all granted
    </Directory>

Configuramos el usuario y el grupo para permitir acceder al directorio donde se encontrará tu carpeta `www` o `localhost`

    User tu-usuario
    Group staff

Configuramos los tipo de archivo index

    <IfModule dir_module>
        DirectoryIndex index.php index.html
    </IfModule>

Desmarcamos los siguientes módulos:

    LoadModule authz_core_module libexec/apache2/mod_authz_core.so
    LoadModule authz_host_module libexec/apache2/mod_authz_host.so
    LoadModule userdir_module libexec/apache2/mod_userdir.so
    LoadModule include_module libexec/apache2/mod_include.so
    LoadModule rewrite_module libexec/apache2/mod_rewrite.so
    LoadModule socache_shmcb_module lib/httpd/modules/mod_socache_shmcb.so
    LoadModule ssl_module lib/httpd/modules/mod_ssl.so
    LoadModule vhost_alias_module lib/httpd/modules/mod_vhost_alias.so
 
Agregamos el módulo de PHP

    LoadModule php7_module /usr/local/opt/php/lib/httpd/modules/libphp7.so

Agregamos la siguiente línea para que Apache interprete archivos php

    <FilesMatch \.php$>
        SetHandler application/x-httpd-php
    </FilesMatch>

Agregramos la siguiente línea para dar acceso a PHPMyAdmin desde `localhost`

    Alias /phpmyadmin /usr/local/share/phpmyadmin
    <Directory /usr/local/share/phpmyadmin/>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
        <IfModule mod_authz_core.c>
            Require all granted
        </IfModule>
        <IfModule !mod_authz_core.c>
            Order allow,deny
            Allow from all
        </IfModule>
    </Directory>

Reiniciamos Apache

    sudo apachectl -k restart
    
Permite validar si el archivo de configuración de Apache está correcto

    sudo apachectl configtest

<br>
Ahora, ponemos por defecto en nuestra terminal la versión de PHP instalada con brew

Si utilizas Bash en terminal:

    echo 'export PATH="/usr/local/opt/php@7.4/bin:$PATH"' >> ~/.bash_profile
    echo 'export PATH="/usr/local/opt/php@7.4/sbin:$PATH"' >> ~/.bash_profile
    source ~/.bash_profile

Si utilizas ZSH en terminal:

    echo 'export PATH="/usr/local/opt/php@7.4/bin:$PATH"' >> ~/.zshrc
    echo 'export PATH="/usr/local/opt/php@7.4/sbin:$PATH"' >> ~/.zshrc
    source ~/.zshrc

Comprobamos

    which php
    
  Respuesta:

    /usr/local/opt/php@7.4/bin/php

