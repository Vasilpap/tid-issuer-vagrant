Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.vm.box = "ubuntu/jammy64"

  config.vm.define "control", primary: true do |h|
    h.vm.hostname = "control"
    h.vm.network "private_network", ip: "192.168.56.10"
    h.vm.provision "shell", path: "scripts/key.sh"
  end

  config.vm.define "api-server" do |h|
    h.vm.hostname = "api-server"
    h.vm.network "private_network", ip: "192.168.56.101"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

   config.vm.define "front-server" do |h|
    h.vm.hostname = "front-server"
    h.vm.network "private_network", ip: "192.168.56.103"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

 config.vm.define "infra-server" do |h|
   h.vm.hostname = "infra-server"
   h.vm.network "private_network", ip: "192.168.56.102"
   h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
 end


end
