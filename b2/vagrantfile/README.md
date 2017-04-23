# Vagrantfile

La configuración de un escenario concreto se realiza de forma bastante
simple mediante modificaciones en este fichero, que está escrito en
formato Ruby.

Realmente la configuración que se aplica es la aplicación en serie de
varios Vagranfiles, tal como se explica en
[Load Order and Merging](https://www.vagrantup.com/docs/vagrantfile/#load-order-and-merging),
aunque lo más habitual es que se cargue el Vagrantfile que incluye el
box y el que exista en el directorio de trabajo, siendo este último el
que se modifica en la mayoría de los casos.

## Modificaciones de la máquina virtual

Se configuran en el espacio de nombres "config.vm", prefijo que
antecede a los parámetros en este caso, por ejemplo para modificar el
hostname de la máquina utilizaríamos:

   ```
   ...
   config.vm.hostname = "openwebianrs"
   ...
   ```
   
El resto de parámetros que se pueden modificar los encontramos en la
documentación de Vagrant:
[Machine Settings](https://www.vagrantup.com/docs/vagrantfile/machine_settings.html),
dejando en nuestro caso para secciones posterior algunos de los
aspectos que necesitan más desarrollo como la configuración de la red
o la configuración integrada de la máquina virtual mediante shell
scripts o mediante aplicaciones como ansible o puppet.

Parámetros relativos a las características de hardware de la máquina
virtual dependen del proveedor en Vagrant y en el caso de VirtualBox
se definen mediante una subsección, pero como en casos anteriores,
consideramos más interesante hacerlo mediante algunos ejercicios.

## Ejercicios

1. Realiza las modificaciones apropiadas en un Vagrantfile para
   cambiar el nombre de la máquina virtual, la memoria RAM asignada y
   el número de núcleos virtuales.
   
   ```
   ...
   config.vm.provider "virtualbox" do |vb|
     vb.name = "nombre"
	 vb.memory = "512"
     vb.cpus = 2
   end
   ....
   ```
2. Como la forma habitual de gestionar máquinas virtuales en Vagrant
   es mediante la línea de comandos y accediendo a ellas a través de
   ssh, no tiene mucho sentido que se arranque una interfaz gráfica,
   pero en algunas ocasiones es conveniente. Modifica un fichero
   Vagrantfile para que se inicie la interfaz gráfica de usuario al
   levantar la máquina.
   
   ```
   ...
   config.vm.provider "virtualbox" do |vb|
     vb.gui = true	 
   end
   ...
   ```
   
3. Aprovisionamiento ligero (*thin provisioning*) es una técnica muy
   utilizada en diferentes sistemas de virtualización y consiste en
   crear un disco de imagen de máquina virtual que incluya sólo las
   modificaciones respecto a una imagen base, consiguiendo un ahorro
   significativo de espacio en disco a costa de una pequeña
   penalización en rendimiento. Configura un Vagrantfile para que se
   realice aprovisionamiento ligero.
   
   ```
   ....
   config.vm.provider "virtualbox" do |vb|
     vb.name = "ligera"
     vb.linked_clone = true
   end
   ...
   ```
   
4. Aunque la configuración completa de las redes lo dejamos para una
   sección posterior, una funcionalidad muy útil y sencilla es la
   redirección de puertos de la red por defecto que utiliza Vagrant
   (red interna con NAT). Configura un Vagrantfile para que las
   peticiones al puerto 8080/tcp de la máquina anfitriona se redirijan al
   puerto 80/tcp de la máquina virtual.
   
   La documentación completa se encuentra en
   [Forwarded Ports](https://www.vagrantup.com/docs/networking/forwarded_ports.html).
   
   ```
   ...
   config.vm.network "forwarded_port", guest: 80, host: 8080
   ...
   ```
   
   Podemos ver en todo momento los puertos que se har redireccionado
   con la instrucción:
   
   ```
   vagrant port
   ```
   Estos cambios se pueden realizar sobre una máquina ya funcionando y
   para que se apliquen se utiliza la opción:
   
   ```
   vagrant reload
   ```
												   
	En algunas ocasiones Vagrant no ofrece directamente la posibilidad
	de hacer cierta configuración específica en la máquina virtual,
	por lo que pierde parte de su atractivo, sin embargo esto puede
	solucionarse utilizando comandos "en bruto", en el caso de
	utilizar VirtualBox, incluyendo en en fichero Vagrantfile comandos
	de VBoxManage, como en el siguiente ejercicio.
	
1. Configura una máquina virtual que tenga un disco adicional de 500
   GiB.
   
   Editamos el fichero Vagrantfile e incluímos las líneas:
   
   ```
   ....
   config.vm.provider "virtualbox" do |vb|
	     file_to_disk = 'tmp/disk.vdi'
		 unless File.exist?(file_to_disk)
	       vb.customize ['createhd', '--filename', file_to_disk, '--size', 500 * 1024]
       end
	       vb.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
   end
   ...
   ```
   En el directorio de trabajo podremos ver que se ha creado un
   fichero en formato vdi::
   
   ```
   ls -hl tmp/
   total 2,0M
   -rw------- 1 alberto alberto 3,0M abr 23 10:42 disk.vdi
   ```
   Y desde la máquina virtual veremos un disco adicional de 500GiB:
   
   ```
   lsblk
   NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
   sda      8:0    0    40G  0 disk 
   └─sda1   8:1    0    40G  0 part /
   sdb      8:16   0   500G  0 disk
   ```

   **NOTA:** Este tipo de configuraciones en las que se pone de forma
     explícita las características de la máquina virtual no son ni
     mucho menos generales, en el caso anterior se está poniendo de
     forma concreta el puerto SATA al que conectar el disco y el
     nombre del controlador SATA, características que pueden variar de
     una máquina virtual a otra. Suele ser conveniente obtener
     previamente información de las características de la máquina
     virtual, que en el caso de VirtualBox se puede hacer con el
     siguiente comando de VBoxManage:
	 
   ```
   VBoxManage showvminfo NOMBREDELAMV
   ```
   
   
	 
