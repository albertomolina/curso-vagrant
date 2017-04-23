# Entornos multi-máquina

Se puede extender la configuración de un escenario virtual con vagrant
a aquellos casos en los que haya más de una máquina virtual y esto se
hace con estructuras como la siguiente en el Vagrantfile:

```
Vagrant.configure("2") do |config|
  config.vm.define "maquina1" do |m1|
    m1.vm.box = "ubuntu/trusty64"
  end
  config.vm.define "maquina2" do |m2|
    m2.vm.box = "ubuntu/trusty64"
  end
end
```
Al existir más de una máquina virtual, muchas de las instrucciones que
hemos visto hasta ahora deben incluir el nombre de la máquina como
parámetro, por ejemplo si hiciéramos:

```
vagrant ssh
This command requires a specific VM name to target in a multi-VM environment.
```
Que deberíamos cambiar por algo como:

```
vagrant ssh maquina1
```

## Redes

Cuando se utilizan entornos multi-máquinas es muy habitual utilizar
redes privadas y configurar la comunicación entre las máquinas del
escenario mediante direcciones IP estáticas.

## Ejercicios

1. Crea un escenario para una aplicación web en la que ubicamos en una
   máquina el servidor web conectado a una red pública exterior y una
   máquina con la base de datos en una máquina no accesible desde el
   exterior. Para que ambas máquinas estén interconectadas entre sí,
   crea la red privada 10.0.100.0/24 a la que estarán conectadas ambas
   máquinas.
   
   ```
   Vagrant.configure("2") do |config|
     config.vm.define "web" do |nodo1|
       nodo1.vm.box = "ubuntu/trusty64"
       nodo1.vm.hostname = "web"
       nodo1.vm.network "public_network", bridge: "eth0"
       nodo1.vm.network "private_network", ip: "10.0.100.101"
     end
     config.vm.define "db" do |nodo2|
       nodo2.vm.box = "ubuntu/trusty64"
       nodo2.vm.hostname = "db"
       nodo2.vm.network "private_network", ip: "10.0.100.102"
     end
   end
   ```
	 
