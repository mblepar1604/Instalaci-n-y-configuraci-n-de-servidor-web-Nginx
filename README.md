# Instalación y configuración de Nginx
## Configuracion de archivos
En esta sección estará todo lo relacionado a la configuración de archivos.
### Configuración básica de vagrantfile
1. Hemos creado una **configuración básica** del archivo añadiendo el sistema y una provisión que instala el servicio **Nginx**.
La estructura ha quedado tal que así:

```
Vagrant.configure("2") do |config|

  config.vm.define "vm1" do |vm1|

    vm1.vm.box = "debian/bookworm64"

    # Le añadimos una red privada
    vm1.vm.network "private_network", ip: "192.168.57.11"

    # Instalación del servicio Nginx
    vm1.vm.provision "shell", inline: <<-SHELL

      sudo apt-get update
      sudo apt-get install -y nginx
      systemctl status nginx

    SHELL

  end
  
end
```

### Configuración inicial de los archivos de Nginx
1. Hemos creado una carpeta llamada **mblesaweb** dentro de var/www/ con una carpeta **html** dentro
2. Hemos clonado dentro de **html** el repositorio de git: _https://github.com/cloudacademy/static-website-example_
3. Hemos modificado los permisos de las carpetas de tal forma que: 
- El propietario de la carpeta **html** y de todo el contenido de ella sea _www-data_
- Le daremos los permisos adecuados para evitar errores de acceso **no autorizados**
```
# Instalación del servicio Nginx y configuración de este
    vm1.vm.provision "shell", inline: <<-SHELL

      sudo apt-get update
      sudo apt-get install -y nginx
      systemctl status nginx

      # Creación de la carpeta del sitio web
      sudo mkdir -p /var/www/mblesaweb/html

      # Dentro de esa carpeta html, clonamos el siguiente repositorio
      cd /var/www/mblesaweb/html
      sudo git clone https://github.com/cloudacademy/static-website-example

      # Ajustamos los permisos de la carpeta
      sudo chown -R www-data:www-data /var/www/mblesaweb/html
      sudo chmod -R 755 /var/www/mblesaweb

    SHELL
```