Vagrant.configure("2") do |config|

  config.vm.define "vm1" do |vm1|

    vm1.vm.box = "debian/bookworm64"

    # Le añadimos una red privada
    vm1.vm.network "private_network", ip: "192.168.57.15"

    # Instalación del servicio Nginx y configuración de este
    vm1.vm.provision "shell", inline: <<-SHELL

    #  -------------- INSTALACION --------------  

      sudo apt-get update
      sudo apt-get install -y nginx
      sudo apt-get install -y git
      sudo apt-get install -y vsftpd
      sudo apt-get install -y ufw

      # Creación de la carpeta del sitio web
      sudo mkdir -p /var/www/mblesaweb/html
      sudo mkdir -p /var/www/martinbweb/html

      # Dentro de esa carpeta html, clonamos el siguiente repositorio
      cd /var/www/mblesaweb/html
      sudo git clone https://github.com/cloudacademy/static-website-example

      # Ajustamos los permisos de la carpeta
      sudo chown -R www-data:www-data /var/www/mblesaweb/html/static-website-example
      sudo chmod -R 755 /var/www/mblesaweb/html
      sudo chown -R vagrant:vagrant /var/www/martinbweb/html/
      sudo chmod -R 755 /var/www/martinbweb/html
      sudo chown -R vagrant:vagrant /var/www
      sudo chmod -R 755 /var/www


      # Pegamos el archivo webMartin
      sudo cp /vagrant/webMartin /etc/nginx/sites-available/mblesaweb
      sudo cp /vagrant/webBlesa /etc/nginx/sites-available/martinbweb

      # Creamos un archivo simbólico entre el archivo los sites y los sitios habilitados
      sudo ln -sf /etc/nginx/sites-available/mblesaweb /etc/nginx/sites-enabled/
      sudo ln -sf /etc/nginx/sites-available/martinbweb /etc/nginx/sites-enabled/

      # Pegamos el archivo hosts
      sudo cp /vagrant/hosts /etc

      # Verificamos y reiniciamos el servicio de Nginx
      sudo nginx -t
      sudo systemctl restart nginx

      # Creamos los certificados de seguridad
      sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
      -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt \
      -subj "/C=ES/ST=Madrid/L=Madrid/O=mblesaweb/CN=mblesaweb"

      # Pegamos el archivo vsftpd.conf
      sudo cp /vagrant/vsftpd.conf /etc

      # Reiniciamos el servicio
      sudo systemctl restart vsftpd

      #  -------------- AUTENTICACION --------------  

      # Guardamos nuestros usuario en el archivo htpasswd y creamos un password cifrado para estos
      sudo sh -c "echo -n 'martin:' > /etc/nginx/.htpasswd"
      sudo sh -c "openssl passwd -apr1 'martin' >> /etc/nginx/.htpasswd_martin"

      sudo sh -c "echo -n 'blesa:' >> /etc/nginx/.htpasswd"
      sudo sh -c "openssl passwd -apr1 'blesa' >> /etc/nginx/.htpasswd"

      # Guardamos dos usuarios para acceso exclusivo posteriormente
      sudo sh -c "echo -n 'martin:' > /etc/nginx/.htpasswd_martin"
      sudo sh -c "openssl passwd -apr1 'martin' >> /etc/nginx/.htpasswd_martin"

      sudo sh -c "echo -n 'martin:' > /etc/nginx/.htpasswd_martinpardo"
      sudo sh -c "openssl passwd -apr1 'martin' >> /etc/nginx/.htpasswd_martinpardo"

      sudo sh -c "echo -n 'pardo:' >> /etc/nginx/.htpasswd_martinpardo"
      sudo sh -c "openssl passwd -apr1 'pardo' >> /etc/nginx/.htpasswd_martinpardo"
      

      # Sacamos el archivo a la carpeta vagrant para mayor facilidad en la comprobación
      cp /etc/nginx/.htpasswd /vagrant
      cp /etc/nginx/.htpasswd_martin /vagrant
      cp /etc/nginx/.htpasswd_martinpardo /vagrant

      # Activamos el perfil para permitir trafico HTTPS
      sudo ufw allow ssh
      sudo ufw allow 'Nginx Full'
      sudo ufw delete allow 'Nginx HTTP'

      # Activamos el cortafuegos
      sudo ufw --force enable

      # Creamos la clave SSL y el certificado
      sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/martinbweb.com.key -out /etc/ssl/certs/martinbweb.com.crt -subj "/C=ES/CN=mblesaweb.com/"

      # Le damos los permisos
      sudo chmod 644 /etc/ssl/certs/martinbweb.com.crt
      sudo chmod 600 /etc/ssl/private/martinbweb.com.key
      sudo chown root:root /etc/ssl/certs/martinbweb.com.crt
      sudo chown root:root /etc/ssl/private/martinbweb.com.key


      # Reiniciamos el servicio Nginx
      sudo systemctl reload nginx

    SHELL

  end

end
