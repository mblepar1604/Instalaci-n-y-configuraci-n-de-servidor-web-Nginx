Vagrant.configure("2") do |config|

  config.vm.define "vm1" do |vm1|

    vm1.vm.box = "debian/bookworm64"

    # Le añadimos una red privada
    vm1.vm.network "private_network", ip: "192.168.57.11"

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

  end

end
