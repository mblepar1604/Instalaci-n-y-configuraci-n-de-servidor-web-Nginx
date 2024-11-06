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
