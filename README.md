# CONFIGURACION DE ARCHIVOS
## Configuracion del archivo vagrantfile
En esta sección estará todo lo relacionado a la configuración del archivo Vagrantfile.
### Configuración básica de vagrant
Hemos creado una configuración básica del archivo añadiendo el sistema y una provisión que instala el servicio Nginx.
La estructura ha quedado tal que así:

```
    Vagrant.configure("2") do |config|

        config.vm.define "vm1" do |vm1|

            vm1.vm.box = "debian/bookworm64"

            vm1.vm.provision "shell", inline: <<-SHELL
            sudo apt-get update
            sudo apt-get install -y nginx
            systemctl status nginx  
            SHELL

        end
    end
```