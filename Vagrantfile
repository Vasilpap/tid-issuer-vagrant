Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.vm.box = "ubuntu/jammy64"

  scenario = ENV.fetch("TID_SCENARIO", "split")
  unless ["split", "docker-env"].include?(scenario)
    raise "Unsupported TID_SCENARIO='#{scenario}'. Use 'split' or 'docker-env'."
  end

  split_enabled = scenario == "split"
  docker_env_enabled = scenario == "docker-env"

  config.vm.define "control", primary: true do |h|
    h.vm.hostname = "control"
    h.vm.network "private_network", ip: "192.168.56.10"
    h.vm.provider "virtualbox" do |vb|
      vb.cpus = 1
      vb.memory = 1024
    end
    h.vm.provision "shell", path: "scripts/key.sh"
  end

  config.vm.define "api-server", autostart: split_enabled do |h|
    h.vm.hostname = "api-server"
    h.vm.network "private_network", ip: "192.168.56.101"
    h.vm.provider "virtualbox" do |vb|
      vb.cpus = 3
      vb.memory = 3072
    end
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "front-server", autostart: split_enabled do |h|
    h.vm.hostname = "front-server"
    h.vm.network "private_network", ip: "192.168.56.103"
    h.vm.provider "virtualbox" do |vb|
      vb.cpus = 1
      vb.memory = 1024
    end
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "infra-server", autostart: split_enabled do |h|
    h.vm.hostname = "infra-server"
    h.vm.network "private_network", ip: "192.168.56.102"
    h.vm.provider "virtualbox" do |vb|
      vb.cpus = 2
      vb.memory = 3072
    end
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "docker-env", autostart: docker_env_enabled do |h|
    h.vm.hostname = "docker-env"
    h.vm.network "private_network", ip: "192.168.56.103"
    h.vm.provider "virtualbox" do |vb|
      vb.cpus = 4
      vb.memory = 6144
    end
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

end
