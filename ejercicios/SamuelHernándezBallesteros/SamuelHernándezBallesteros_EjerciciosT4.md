##Tema 4: Virtualización ligera usando contenedores

#Ejercicio 1
Instala LXC en tu versión de Linux favorita. Normalmente la versión en desarrollo, disponible tanto en GitHub como en el sitio web está bastante más avanzada; para evitar problemas sobre todo con las herramientas que vamos a ver más adelante, conviene que te instales la última versión y si es posible una igual o mayor a la 1.0.

Instalamos LXC:

sudo apt-get install lxc lxc-templates

Módulos compatibles:

![Tema4EJ1.png](http://i61.tinypic.com/2dt1d4.jpg)

#Ejercicio 2
Comprobar qué interfaces puente se han creado y explicarlos.

Se ha creado el siguiente interfaz puente:

![Tema4EJ2.png](http://i62.tinypic.com/152g5cy.jpg)

#Ejercicio 3
Crear y ejecutar un contenedor basado en Debian.

Crear y ejecutar un contenedor basado en otra distribución, tal como Fedora. Nota En general, crear un contenedor basado en tu distribución y otro basado en otra que no sea la tuya. Fedora, al parecer, tiene problemas si estás en Ubuntu 13.04 o superior, así que en tal caso usa cualquier otra distro.

Creamos el contenedor:

sudo lxc-create -t debian -n mi_contenedor_debian

Lo iniciamos:

sudo lxc-start -n minube_debian

![Tema4EJ3.png](http://i61.tinypic.com/36hs2.jpg)

#Ejercicio 4

Instalar lxc-webpanel y usarlo para arrancar, parar y visualizar las máquinas virtuales que se tengan instaladas.

Desde el panel restringir los recursos que pueden usar: CPU shares, CPUs que se pueden usar (en sistemas multinúcleo) o cantidad de memoria.

Instalamos lxc-webpanel en modo administrador:

wget http://lxc-webpanel.github.io/tools/install.sh -O - | bash

Una vez instalado accedemos desde el navegador con http://localhost:5000 y accedemos al panel con el usuario y contraseña correspondiente.

Elegimos la máquina virtual y ya podemos restringuir los recursos que queramos.

![Tema4EJ3.png](http://i59.tinypic.com/23w1w92.jpg)


#Ejercicio 5
Comparar las prestaciones de un servidor web en una jaula y el mismo servidor en un contenedor. Usar nginx.

#Ejercicio 6
Instalar juju.

Seguimos la instrucciones de la sesión:

sudo add-apt-repository ppa:juju/stable

sudo apt-get update && sudo apt-get install juju-core

juju init

Editamos el archivo enviroments.yaml para poder trabajar con contenedores LXC y añadimos default: local.

Creamos el táper

juju bootstrap

Usándolo, instalar MySQL en un táper.

Deployamos mysql

![Tema4EJ6.png](http://i58.tinypic.com/35aueti.jpg)


#Ejercicio 7
Destruir toda la configuración creada anteriormente

juju destroy-service mysql

sudo juju destroy-environment local

Volver a crear la máquina anterior y añadirle mediawiki y una relación entre ellos.

Hacemos lo anterior
juju bootstrap
juju deploy mysql

Crear un script en shell para reproducir la configuración usada en las máquinas que hagan falta.

El script tendría lo siguiente:
juju switch local
sudo juju bootstrap
juju deploy mysql
echo "Proceso finalizado"

![Tema4EJ7.png](http://i57.tinypic.com/k0sh0p.jpg)

#Ejercicio 8
Instalar libvirt. Te puede ayudar esta guía para Ubuntu.

Usamos el siguiente comando:

sudo apt-get install kvm libvirt-bin virtinst

Comprobamos si es compatible:

samuel@samuel-virtual-machine:/root/.juju$ kvm-ok
INFO: Your CPU does not support KVM extensions
INFO: For more detailed results, you should run this as root
HINT:   sudo /usr/sbin/kvm-ok

En mi caso no es compatible.

Comprobamos si se puede ejcutar virsh

![Tema4EJ8.png](http://i57.tinypic.com/200xqhw.jpg)

#Ejercicio 9
Instalar un contenedor usando virt-install.

Instalamos virt-viewer con el comando:

sudo apt-get install virt-viewer

Después debemos descargar la imagen de alguna de las distribuciones y utilizar el siguiente comando para crear el contenedor donde se indica la distribución, la ram, etc..:

sudo virt-install -r 1024 --accelerate -n Fedora14 \
  -f /path/to/guest.img \
  --cdrom Fedora-14-x86_64-Live.iso

#Ejercicio 10
Instalar docker.

Seguimos el [tutorial](http://docs.docker.com/installation/ubuntulinux/#ubuntu-trusty-1404-lts-64-bit):

sudo apt-get install docker.io

Añadimos la llave del repositorio de Docker: sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

Ejecutamos la siguiente orden para añadir el repositorio:
sudo sh -c "echo deb https://get.docker.com/ubuntu docker main\ /etc/apt/sources.list.d/docker.list"

Actualizamos los repositorios e instalamos con:

sudo apt-get update & sudo apt-get install lxc-docker

Una vez instalado lo ejecutamos con:

sudo docker -d &


#Ejercicio 11
Instalar a partir de docker una imagen alternativa de Ubuntu y alguna adicional, por ejemplo de CentOS.

Descargamos la imagen de ubuntu con la orden:

	sudo docker pull ubuntu

![EJ11-1](http://i60.tinypic.com/34tep0o.jpg)

Hacemos lo mismo con CentOS

	sudo docker pull centos

Buscar e instalar una imagen que incluya MongoDB

La imagen de mongodb la instalamos con la orden:

	sudo docker pull dockerfile/mongodb

Ejemplo de ejecución en el contenedor CentOS

![EJ11-2](http://i58.tinypic.com/2nvcj8x.jpg)


#Ejercicio 12
Crear un usuario propio e instalar nginx en el contenedor creado de esta forma.

Entramos al contenedor de ubuntu anteriormente creado con la orden:

	sudo docker run -i -t ubuntu /bin/bash

Añadimos un nuevo usuario con posibilidad de ejecutar sudo:

	useradd -d /home/nuevousuario -m nuevousuario

	adduser nuevousuario sudo
    
	passwd nuevousuario
    
    su nuevousuario
    
Actualizamos los repositorios e instalamos nginx

	sudo apt-get update && sudo apt-get install nginx

Iniciamos el servicio

	sudo service nginx start

![EJ12-1](http://i62.tinypic.com/m8fdso.jpg)

#Ejercicio 13
Crear a partir del contenedor anterior una imagen persistente con commit.

Con el contenedor funcionando en otra terminal listamos los dockers con la orden:

	sudo docker ps
    
![Ejercicio13-1](http://i61.tinypic.com/2ds0cxv.jpg)

Guardamos la ID y escribimos:

	sudo docker commit <ID> nuevonombre

![Ejercicio13-2](http://i61.tinypic.com/ou1swk.jpg)

El resultado será otra ID

Podemos ver las imagenes que hemos creado con:

	sudo docker images

![Ejercicio14-3](http://i60.tinypic.com/34iimag.jpg)

#Ejercicio 14
Crear una imagen con las herramientas necesarias para el proyecto de la asignatura sobre un sistema operativo de tu elección.

Nos registramos en la página web de [Docker](https://hub.docker.com/account/signup-with-github/)

Una vez registrados pulsamos sobre Automated Build

Elegimos el repositorio de Github o Bitbucket en el que queremos que se haga el despliegue.

Cuando se haya creado debemos de crear un Dockerfile que indique que es lo que se debe instalar.

    FROM ubuntu
    MAINTAINER hp007 <shball101@gmail.com> Version: 1.0
    RUN apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion pkg-config
    RUN gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3 
    RUN \curl -sSL https://get.rvm.io | bash -s stable --rails
    RUN curl -sL https://deb.nodesource.com/setup | sudo bash -
    RUN sudo apt-get install -y nodejs

