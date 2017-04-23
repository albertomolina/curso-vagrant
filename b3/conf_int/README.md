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
   
   ```
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu/trusty64"
     config.vm.provision "shell", path: "actualizar.sh"
   end
	 

   
