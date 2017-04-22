# Línea de comandos

El uso de vagrant exige utilizar la línea de comandos desde el
principio, lo que hemos procurado en este curso ha sido ir haciéndolo
de forma paulatina mediante algunos ejemplos para que resultase más
sencillo su aprendizaje y en esta sección veremos varios comandos que
no se han explicado aún.

La lista de comandos más habituales de vagrant se puede obtener con un
simple "help" y la lísta completa de comandos se obiene con:

   <pre>
   vagrant list-commands
   Below is a listing of all available Vagrant commands and a brief
   description of what they do.
   
   <b>box</b>             manages boxes: installation, removal, etc.
   cap             checks and executes capability
   connect         connect to a remotely shared Vagrant environment
   <b>destroy</b>         stops and deletes all traces of the vagrant machine
   docker-exec     attach to an already-running docker container
   docker-logs     outputs the logs from the Docker container
   docker-run      run a one-off command in the context of a container
   <b>global-status</b>   outputs status Vagrant environments for this user
   <b>halt</b>            stops the vagrant machine
   <b>help</b>            shows the help for a subcommand
   <b>init</b>            initializes a new Vagrant environment by creating a Vagrantfile
   <b>list-commands</b>   outputs all available Vagrant subcommands, even non-primary ones
   login           log in to HashiCorp's Atlas
   <b>package</b>         packages a running vagrant environment into a box
   plugin          manages plugins: install, uninstall, update, etc.
   <b>port</b>            displays information about guest port mappings
   powershell      connects to machine via powershell remoting
   provider        show provider for this environment
   provision       provisions the vagrant machine
   push            deploys code in this environment to a configured destination
   rdp             connects to machine via RDP
   <b>reload</b>          restarts vagrant machine, loads new Vagrantfile configuration
   resume          resume a suspended vagrant machine
   rsync           syncs rsync synced folders to remote machine
   rsync-auto      syncs rsync synced folders automatically when files change
   share           share your Vagrant environment with anyone in the world
   snapshot        manages snapshots: saving, restoring, etc.
   <b>ssh</b>             connects to machine via SSH
   ssh-config      outputs OpenSSH valid configuration to connect to the machine
   <b>status</b>          outputs status of the vagrant machine
   suspend         suspends the machine
   <b>up</b>              starts and provisions the vagrant environment
   version         prints current and latest Vagrant version
   </pre>
Donde hemos resaltado las opciones que hemos visto anteriormente.   

Veamos algunas de estas opciones de nuevo con algunos ejemplos/ejercicios.

## Ejercicios

1. Suspende una máquina virtual que esté arrancada, comprueba el estado y reanúdala.

   Para suspenderla a disco:
   
   ```
   vagrant suspend
   ```
   Comprobamos su estado:
   
   ```
   vagrant status
   Current machine states:
   
   default                   saved (virtualbox)
   
   To resume this VM, simply run `vagrant up`.
   ```
   
   Y se reanuda con "vagrant up" o con "vagrant resume", este último
   no hace ningún tipo de comprobación sobre la máquina, el box,
   etc. simplemente reanuda la máquina desde su estado de suspensión.
   
2. Comprueba la configuración ssh de una máquina vagrant.

   ```
   vagrant ssh-config
   Host default
     HostName 127.0.0.1
	 User vagrant
	 Port 2222
	 UserKnownHostsFile /dev/null
	 StrictHostKeyChecking no
	 PasswordAuthentication no
	 IdentityFile /home/alberto/Prueba1/.vagrant/machines/default/virtualbox/private_key
     IdentitiesOnly yes
	 LogLevel FATAL
	 
   ```
1. Lanza una máquina vagrant, realiza alguna modificación, guarda una
   instantánea, realiza una segunda modificación y restaura la máquina desde la
   instantánea.
   
   Creamos una máquina, accedemos a ella e instalamos un paquete (por
   ejemplo apache2", redirigimos el puerto 8080/tcp de la anfitriona
   al 80/tco de la máquina virtual. Realizamos una instantánea
   (*snapshot*) de la máquina:
   
   ```
   vagrant snapshot save apache-limpio
   ==> apache-limpio: Snapshotting the machine as 'apache-limpio'...
   ==> apache-limpio: Snapshot saved! You can restore the snapshot at any time by
   ==> apache-limpio: using `vagrant snapshot restore`. You can delete it using
   ==> apache-limpio: `vagrant snapshot delete`.
   ```
   Podemos comprobar en el directorio de máquinas virtuales de
   VirtualBox el contenido del directorio Snapshots.
   
   Realizamos una modificación del index.html y restauramos la máquina
   al estado que tenía cuando se hizo la instantánea:
   
   ```
   vagrant snapshot restore apache-limpio
   ```
   
	   
	   
