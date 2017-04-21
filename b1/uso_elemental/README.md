# Uso elemental de vagrant

La documentación de Vagrant está disponible en:

[docs.vagrantup.com](http://docs.vagrantup.com)

## ¡Ayuda!

Una vez instalado vagrant, y como es recomendable siempre ante una
nueva aplicación, miramos la ayuda disponible para ver la sintaxis
elemental:
```
vagrat -h

Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
	-h, --help                       Print this help.
		
Common commands:
	box             manages boxes: installation, removal, etc.
	connect         connect to a remotely shared Vagrant environment
	destroy         stops and deletes all traces of the vagrant machine
	global-status   outputs status Vagrant environments for this user
	halt            stops the vagrant machine
	help            shows the help for a subcommand
	init            initializes a new Vagrant environment by creating a Vagrantfile
	login           log in to HashiCorp's Atlas
	package         packages a running vagrant environment into a box
	plugin          manages plugins: install, uninstall, update, etc.
	port            displays information about guest port mappings
	powershell      connects to machine via powershell remoting
	provision       provisions the vagrant machine
	push            deploys code in this environment to a configured destination
	rdp             connects to machine via RDP
	reload          restarts vagrant machine, loads new Vagrantfile configuration
	resume          resume a suspended vagrant machine
	share           share your Vagrant environment with anyone in the world
	snapshot        manages snapshots: saving, restoring, etc.
	ssh             connects to machine via SSH
	ssh-config      outputs OpenSSH valid configuration to connect to the machine
	status          outputs status of the vagrant machine
	suspend         suspends the machine
	up              starts and provisions the vagrant environment
	version         prints current and latest Vagrant version
																																	 
For help on any individual command run `vagrant COMMAND -h`

Additional subcommands are available, but are either more advanced
or not commonly used. To see all subcommands, run the command
`vagrant list-commands`.
```

Donde podemos ver que podemos obtener ayuda de un comando o de un
subcomando poniendo "-h" al final de la instrucción.

## Ejercicios

1. Añadir una primera imagen (Ubuntu Trusty de 64 bits).
```
vagrant box add ubuntu/trusty64
```
2. Añadir algunas imágenes más de sistemas operativos básicos (Debian
   Jessie y CentOS 7).
   
3. Añadir una imagen de un sistema configurado

4. Uso básico de vagrant: vagrant init, vagrant up, vagrant status,
   vagrant halt y  vagrant destroy
   
Creamos un directorio para cada proyecto, accedemos al proyecto y
creamos un fichero de configuración básico de vagrant con:
```
vagrant init -m ubuntu/trusty64
```
Levantamos la máquina con, que copiará la imagen como una nueva
máquina y la arrancará:
```
vagrant up
```
Accedemos a la máquina con:
```
vagrant ssh
```
Comprobamos en todo momento el estado de la máquina con:
```
vagrant status
```
Podemos parar la máquina con:
```
vagrant halt
```
Si de nuevo levantamos la máquina, se reutilizará la máquina
anteriormente creada y los cambios que se hubieran efectuado en
ella. Cuando no queramos volver a utilizar esa máquina la eliminamos
permanentemente con:
```
vagrant destroy
```


   
