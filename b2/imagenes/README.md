# Gestión de imágenes de Vagrant (boxes)

Vagrant denomina box a un paquete que contiene la imagen de una máquina virtual con el formato adecuado para un determinado proveedor y algunos ficheros con metadatos. Aquí utilizaremos de manera indistinta y premeditada el término "box" o imagen. Cualquier usuario puede crearse sus propias imágenes de Vagrant aunque existe un sitio gestionado por Hashicorp donde pueden obtenerse multitud de imágenes:

[https://atlas.hashicorp.com/boxes/search](https://atlas.hashicorp.com/boxes/search)

*ADVERTENCIA:* Cualquier usuario puede subir boxes a Atlas

En los últimos años las diferentes distribuciones de GNU/Linux han
comenzado a distribuir imágenes de vagrant "oficiales" y son las que
más se utilizan, en particular las de Ubuntu.

## Ejercicios

1. Accede a atlas y busca imágenes para virtualbox utilizando
   diferentes términos e instálalas en tu equipo
   
1. Obtener un box de una imagen previamente instalada:

   ```
   vagrant box repackage centos/7 virtualbox 1703.01
   ```
   
1. Borrar la imagen de centos

   ```
   vagrant box remove centos/7
   
1. Instalar de nuevo el box de CentOS 7 desde el fichero ".box" en
   lugar de hacerlo desde Atlas:
   
   ```
   vagrant box add package.box --name centos/7
   ```
   
1. Comprobar si están actualizadas las imágenes:

   ```
   vagrant box outdated --global
   ```
   
   En el caso de imágenes desactualizadas se pueden eliminar con:
   
   ```
   vagrant box prune
   ```
   
1. Para actualizar el box correspondiente a un escenario se utiliza
   (sólo válido si se ha descargado de Atlas o similar):
   
   ```
   vagrant box update
   ```
   
1. Crear un box de Vagrant desde una máquina virtual de VirtualBox.

   Obtenermos el nombre de la máquina virtual:
   
   ```
   VBoxManage list vms
   ```
      La "empaquetamos" como un box de vagrant con:
   
   ```
   vagrant package --base "Nombre de la MV" --output maquina.box
   
1. Obtener un box de Vagrant desde una OVA. Descrito en
   https://github.com/crohr/ebarnouflant/issues/7
   
