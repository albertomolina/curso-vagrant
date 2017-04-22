# Línea de comandos

El uso de vagrant exige utilizar la línea de comandos desde el
principio, lo que hemos procurado en este curso ha sido ir haciéndolo
de forma paulatina mediante algunos ejemplos para que resultase más
sencillo su aprendizaje y en esta sección veremos varios comandos que
no se han explicado aún.

LA lista de comandos más habituales de vagrant se puede obtener con un
simple "help" y la lísta completa de comandos se obiene con:

   <pre>
   vagrant list-commands
   Below is a listing of all available Vagrant commands and a brief
   description of what they do.
   
   <b>box</b>      manages boxes: installation, removal, etc.
   cap             checks and executes capability
   connect         connect to a remotely shared Vagrant environment
   destroy         stops and deletes all traces of the vagrant machine
   docker-exec     attach to an already-running docker container
   docker-logs     outputs the logs from the Docker container
   docker-run      run a one-off command in the context of a container
   global-status   outputs status Vagrant environments for this user
   halt            stops the vagrant machine
   help            shows the help for a subcommand
   init            initializes a new Vagrant environment by creating a Vagrantfile
   list-commands   outputs all available Vagrant subcommands, even non-primary ones
   login           log in to HashiCorp's Atlas
   package         packages a running vagrant environment into a box
   plugin          manages plugins: install, uninstall, update, etc.
   port            displays information about guest port mappings
   powershell      connects to machine via powershell remoting
   provider        show provider for this environment
   provision       provisions the vagrant machine
   push            deploys code in this environment to a configured destination
   rdp             connects to machine via RDP
   reload          restarts vagrant machine, loads new Vagrantfile configuration
   resume          resume a suspended vagrant machine
   rsync           syncs rsync synced folders to remote machine
   rsync-auto      syncs rsync synced folders automatically when files change
   share           share your Vagrant environment with anyone in the world
   snapshot        manages snapshots: saving, restoring, etc.
   ssh             connects to machine via SSH
   ssh-config      outputs OpenSSH valid configuration to connect to the machine
   status          outputs status of the vagrant machine
   suspend         suspends the machine
   up              starts and provisions the vagrant environment
   version         prints current and latest Vagrant version
   </pre>
   
