Repositorio que conecta ansible y jenkins con una maquina vagrant y una maquina remota en AWS.

En Vagrant hacemos:
 - Instalamos una box de ubuntu 16
 - Llamamos a Ansible para provisionar la maquina ubuntu
 - Hacemos un forward del puerto HOST 8080 --> 8080 GUEST donde esta nginx que a su vez nos redirige a jenkins
 - Hacemos un forward del puerto HOST 8081 --> 8080 GUEST donde esta jenkins
 - Creamos las variables globales del proxy
 - Instalamos python 2.7 necesario para usar ansible (hacemos un link con la carpeta de python)
 - Copiamos key en la carpeta de ubuntu authorized keys (previamente generamos key en host)
 - Cambiamos sshd_config para permitir RootLogin por ssh

En la parte de Ansible hacemos:
 - Instalamos Nginx
 - Instalamos Jenkins
 - Podemos conectar con vagrant o con AWS dependiendo de que host queramos


En AWS tenemos:
 - Una maquina ubuntu instalada
 