# Plugins

La funcionalidad de Vagrant es extensible mediante plugins,
desarrollados por la propia Hashicorp o por terceros. La documentación
de Vagrant describe de forma detallada los conceptos básicos para el
desarrollo de los mismos: https://www.vagrantup.com/docs/plugins/

Siguiendo con la metodología de este curso, vamos a mostrar algunos
ejemplos del uso de plugins mediante algunos ejercicios.

## Ejercicios

1. Comprueba los plugins instalados en vagrant

   ```
   vagrant plugin list
   vagrant-share (1.1.7, system)
   ```
   
1. Instala el plugin de vagrant para utilizar AWS como proveedor
   (desarrollado también por M. Hashimoto).
   
   Este plugin se encuentra en Github: https://github.com/mitchellh/vagrant-aws
   
   La instalación del plugin es realmente sencilla, basta con hacer:
   
   ```
   vagrant plugin install vagrant-aws
   Installing the 'vagrant-aws' plugin. This can take a few minutes...
   Fetching: ipaddress-0.8.3.gem (100%)
   Fetching: formatador-0.2.5.gem (100%)
   Fetching: excon-0.55.0.gem (100%)
   Fetching: fog-core-1.43.0.gem (100%)
   Fetching: fog-xml-0.1.3.gem (100%)
   Fetching: fog-json-1.0.2.gem (100%)
   Fetching: trollop-2.1.2.gem (100%)
   Fetching: CFPropertyList-2.3.5.gem (100%)
   Fetching: rbvmomi-1.11.0.gem (100%)
   Fetching: fission-0.5.0.gem (100%)
   Fetching: inflecto-0.0.2.gem (100%)
   Fetching: xml-simple-1.1.5.gem (100%)
   Fetching: fog-digitalocean-0.3.0.gem (100%)
   Fetching: fog-xenserver-0.3.0.gem (100%)
   Fetching: fog-vsphere-1.9.1.gem (100%)
   Fetching: fog-voxel-0.1.0.gem (100%)
   Fetching: fog-vmfusion-0.1.0.gem (100%)
   Fetching: fog-terremark-0.1.0.gem (100%)
   Fetching: fog-storm_on_demand-0.1.1.gem (100%)
   Fetching: fog-softlayer-1.1.4.gem (100%)
   Fetching: fog-serverlove-0.1.2.gem (100%)
   Fetching: fog-sakuracloud-1.7.5.gem (100%)
   Fetching: fog-riakcs-0.1.0.gem (100%)
   Fetching: fog-radosgw-0.0.5.gem (100%)
   Fetching: fog-rackspace-0.1.5.gem (100%)
   Fetching: fog-profitbricks-3.0.0.gem (100%)
   Fetching: fog-powerdns-0.1.1.gem (100%)
   Fetching: fog-openstack-0.1.20.gem (100%)
   Fetching: fog-local-0.3.1.gem (100%)
   Fetching: fog-google-0.1.0.gem (100%)
   Fetching: fog-ecloud-0.3.0.gem (100%)
   Fetching: fog-dynect-0.0.3.gem (100%)
   Fetching: fog-dnsimple-1.0.0.gem (100%)
   Fetching: fog-cloudatcost-0.1.2.gem (100%)
   Fetching: fog-brightbox-0.11.0.gem (100%)
   Fetching: fog-aws-1.3.0.gem (100%)
   Fetching: fog-atmos-0.1.0.gem (100%)
   Fetching: fog-aliyun-0.1.0.gem (100%)
   Fetching: iniparse-1.4.2.gem (100%)
   Fetching: fog-1.40.0.gem (100%)
   ------------------------------
   Thank you for installing fog!
   
   IMPORTANT NOTICE:
   If there's a metagem available for your cloud provider,
   e.g. `fog-aws`,
   you should be using it instead of requiring the full fog collection
   to avoid
   unnecessary dependencies.
   
   'fog' should be required explicitly only if:
   - The provider you use doesn't yet have a metagem available.
   - You require Ruby 1.9.3 support.
   ------------------------------
   Fetching: vagrant-aws-0.7.2.gem (100%)
   Installed the plugin 'vagrant-aws (0.7.2)'!
   ```
   
   Tal como se indica en la documentación, es necesario instalar un
   box "dummy" para que no nos dé un error de formato el Vagrantfile: 
   
   ```
   vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box
   ```
   
   Se podría utilizar directamente poniendo las credenciales de acceso
   en el fichero Vagrantfile, pero parece más razonable utilizar un
   entorno de AWS estándar como el siguiente:
   
   ```
   /home/alberto/.aws
   ├── config
   └── credentials
   ```
   Siendo el contenido del fichero config:
   
   ```
   [default]
   region=eu-west-1
   ```
   Y del fichero credentials:
   
   ```
   [default]
   aws_access_key_id = ...
   aws_secret_access_key = ...
   ```

	Como último paso previo, cargamos en el agente ssh de nuestro
    entorno la clave privada ssh que queramos utilizar en AWS, por
    ejemplo:
	
	```
	ssh-add ~/.ssh/clave-aws.pem
	```
	Y utilizamos el siguiente fichero Vagrantfile que lanza una
	instancia de tipo m3.medium en la región escogida, le asigna una
	dirección IP pública y le instala rsync para utilizar el
	directorio compartido:
	
	```
	Vagrant.configure("2") do |config|
      config.vm.box = "dummy"
      config.ssh.keys_only = false
      config.vm.provider :aws do |aws, override|
        aws.keypair_name = "clave-aws"
        aws.ami = "ami-3291be54"
        override.ssh.username = "admin"
      end
   end
   ```
1. Utiliza el plugin vagrant-persistent-storage para incluir
   almacenamiento permanente sin necesidad de utilizar parámetros de
   VBoxManage en bruto.
   
   Este plugin está disponible en Github y no es "oficial": https://github.com/kusnier/vagrant-persistent-storage

   Lo instalamos de forma idéntica al caso anterior:
   
   ```
   vagrant plugin install vagrant-persistent-storage
   ```
   Y añadimos las siguientes líneas a nuestro fichero Vagrantfile
   (para un disco adicional de hasta 5 GiB, con sistema de ficheros
   ext4 y montado en el directorio /var/lib/mysql):
   
   ```
   ...
   config.persistent_storage.enabled = true
   config.persistent_storage.location = "tmp/disk.vdi"
   config.persistent_storage.size = 5000
   config.persistent_storage.mountname = 'mysql'
   config.persistent_storage.filesystem = 'ext4'
   config.persistent_storage.mountpoint = '/var/lib/mysql'
   config.persistent_storage.volgroupname = 'myvolgroup'
   ...
   ```
   
