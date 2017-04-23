# Configuración integrada

Vagrant denomina aprovisionamiento (*provisioning*) a los procesos de
configuración de la máquina virtual una vez que esta ha finalizado el
arranque, procesos que pueden incluir la ejecución de comandos de
shell, la ejecución de un shell script completo o la utilización de
herramientas de la gestión de la configuración (*configuration
management systems*) como puppet, chef o ansible.

Este proceso se realiza típicamente la primera vez que se inicia un
entorno vagrant, cuando se invoca directamente desde la línea de
comandos con "vagrant provision" o cuando se recarga un entorno de
vagrant con la opción "--provision", en el resto de casos en los que
volvamos a iniciar un entorno no se volverá a configurar.

El campo de las herramientas de la gestión de la configuración es muy
amplio y su explicación detallada sale totalmente del ámbito de este
curso. En el caso de vagrant la solución que se ha encontrado es muy
razonable, ya que en lugar de desarrollar una nueva herramienta para
competir con las cuatro soluciones más utilizadas hoy en día: ansible, 
puppet, chef y salt, la gente de Hashicorp ha optado por una solución
muy adecuada y razonable: integrar 
## Ejemplos

1. Crea un entorno vagrant sencillo en el que tras arrancar una
   máquina se realice una actualización de la lista de paquetes y se
   efectúen las actualizaciones de aquellos paquetes necesarios
   mediante apt-get
   
   Creamos el fichero actualizar.sh ubicado en el mismo directorio que
   el Vagrantfile y con el siguiente contenido:
   
   ```
   #!/bin/bash
   apt-get update
   DEBIAN_FRONTEND=noninteractive apt-get -y upgrade
   ```
   Creamos el siguiente Vagrantfile:
   
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu/trusty64"
     config.vm.provision "shell", path: "actualizar.sh"
   end
	 
1. Crea un entorno idéntico al anterior, realizando las mismas tareas,
   pero ahora utilizando ansible, que debe estar instalado en la
   máquina anfitriona.
   
   Creamos un fichero site.yml de configuración de ansible para que se realice
   la actualización de paquetes:
   
   ```
   ---
   - hosts: all
     sudo: True
     tasks:
	   - name: Ensure system is updated
         apt: update_cache=yes upgrade=yes
   ```
   Y el correspondiente Vagrantfile sería:
   
   ```
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu/trusty64"
     config.vm.provision "ansible" do |ansible|
       ansible.playbook = "site.yml"
     end
   end
   ```
			 
1. Más que un ejercicio, el siguiente es un ejemplo de configuración
   integrada de vagrant con ansible, desplegando un clúster de
   balanceo de carga mediante un round-robin en un servidor DNS
   
   Clona el siguiente repositorio y sigue las instrucciones que se
   incluyen en el fichero README.md:
   
   https://github.com/albertomolina/ejemplo-ansible-vagrant
