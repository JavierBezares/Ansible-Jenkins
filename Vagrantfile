# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.define "ubuntuJen" do |one|
    one.vm.box = "ubuntu/xenial64"
    #one.vm.boot_timeout = 60 #si tarda mas de 1 min en hacer check de updates de la box pasa a otra cosa
    one.vm.network "private_network", ip:"123.123.123.5"
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.name = "ubuntuJen"
      vb.cpus = "1"
    end
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provision/install.yml"
      ansible.host_key_checking = false #acepta el certificado publico de la maquina remota sin preguntar
      ansible.sudo = true
      ansible.tags = ['common', 'jenkins'] # indica los tags de ejecucion
    end

  # nuestro puerto local 8080 nos lleva al puerto 80 del ubuntu virtualizado donde esta nginx
  # lo hemos redirigido de forma que el puerto virtual 80 de nginx apunte al puerto virtual 8080 donde esta jenkins
  # LOCAL 8080 --> 80 VIRT NGINX
  # 80 VIRT NGINX ---> VIRT LOCAL 8080 JENKINS
  # Al final nuestro local 8080 nos lleva al virtual 8080 donde esta JENKINS
  config.vm.network "forwarded_port", host: 8080, guest: 80, auto_correct: true 
  #si vamos a localhost:8081 nos lleva a la maquina vagrant ubuntu al puerto 8080 donde esta jenkins
  # LOCAL 8081 ---> VIRT LOCAL 8080 JENKINS
  config.vm.network "forwarded_port", host: 8081, guest: 8080, auto_correct: true 


  end
  

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  #

  config.vm.provision "shell", inline: <<-SHELL
    export http_proxy=http://proxy.gfi.es:8000
    export https_proxy=http://proxy.gfi.es:8000
    apt-get install -y python2.7
    ln -s /usr/bin/python2.7 /usr/bin/python
    touch .ssh/authorized_keys
    echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDaRx65vdIS4zH9ywiGIVblEZEcDpgPd0Sc3A3icNi3amD873Vs08QraQG0aV1vODjpo+ogfWFZ4tt4VZ6XmRuaXJGacTCkFk3unFmV5OxzAjBYtxQr/wJIAMCx9lzHXAUvwOTrVcKq8BHi1I/AILZcvUil1Z3/LUwzSm32aNGtRjhog/ElsyZfDGLU+vrHX6NE8aYACRw+ilbmKySTzGSAhssmgpN9LvYPaSddJpBR0DvYKOwPbMd5YeH4WewMSiNba18q1K8EuucxhgFb2Ywle+28yWx73fluOihpTrkuFTR7tcwnQf8tR5/HWFlPNm7GDwGZ+NIfUtHgC6vCHe/9 javier@localhost.localdomain" >> ~/.ssh/authorized_keys
    echo "# Package generated configuration file
# See the sshd_config(5) manpage for details

# What ports, IPs and protocols we listen for
Port 22
# Use these options to restrict which interfaces/protocols sshd will bind to
#ListenAddress ::
#ListenAddress 0.0.0.0
Protocol 2
# HostKeys for protocol version 2
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
#Privilege Separation is turned on for security
UsePrivilegeSeparation yes

# Lifetime and size of ephemeral version 1 server key
KeyRegenerationInterval 3600
ServerKeyBits 1024

# Logging
SyslogFacility AUTH
LogLevel INFO

# Authentication:
LoginGraceTime 120
PermitRootLogin yes
StrictModes yes

RSAAuthentication yes
PubkeyAuthentication yes
#AuthorizedKeysFile %h/.ssh/authorized_keys

# Don't read the user's ~/.rhosts and ~/.shosts files
IgnoreRhosts yes
# For this to work you will also need host keys in /etc/ssh_known_hosts
RhostsRSAAuthentication no
# similar for protocol version 2
HostbasedAuthentication no
# Uncomment if you don't trust ~/.ssh/known_hosts for RhostsRSAAuthentication
#IgnoreUserKnownHosts yes

# To enable empty passwords, change to yes (NOT RECOMMENDED)
PermitEmptyPasswords no

# Change to yes to enable challenge-response passwords (beware issues with
# some PAM modules and threads)
ChallengeResponseAuthentication no

# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes

# Kerberos options
#KerberosAuthentication no
#KerberosGetAFSToken no
#KerberosOrLocalPasswd yes
#KerberosTicketCleanup yes

# GSSAPI options
#GSSAPIAuthentication no
#GSSAPICleanupCredentials yes

X11Forwarding yes
X11DisplayOffset 10
PrintMotd no
PrintLastLog yes
TCPKeepAlive yes
#UseLogin no

#MaxStartups 10:30:60
#Banner /etc/issue.net

# Allow client to pass locale environment variables
AcceptEnv LANG LC_*

Subsystem sftp /usr/lib/openssh/sftp-server

# Set this to 'yes' to enable PAM authentication, account processing,
# and session processing. If this is enabled, PAM authentication will
# be allowed through the ChallengeResponseAuthentication and
# PasswordAuthentication.  Depending on your PAM configuration,
# PAM authentication via ChallengeResponseAuthentication may bypass
# the setting of "PermitRootLogin without-password".
# If you just want the PAM account and session checks to run without
# PAM authentication, then enable this but set PasswordAuthentication
# and ChallengeResponseAuthentication to 'no'.
UsePAM yes
" > /etc/ssh/sshd_config

   SHELL
end