Repositorio que conecta ansible y jenkins con una maquina vagrant y una maquina remota en AWS.
En la parte de Ansible hacemos:
 - Instalamos Nginx
 - Instalamos Jenkins
 - Hacemos un forward del puerto HOST 8080 --> 8080 GUEST donde esta nginx que a su vez nos redirige a jenkins
 - Hacemos un forward del puerto HOST 8081 --> 8080 GUEST donde esta jenkins