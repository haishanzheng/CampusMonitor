# -*- mode: ruby -*-
# vi: set ft=ruby :

$controller_shell_provision_script = <<-SCRIPT
sudo mkdir -p /vagrantkeys

SCRIPT

for hostname in ["cm-zabbix", "cm-zabbix-proxy", "cm-icinga", "cm-smokeping", "cm-smokeping-slave"]
  $known_host = <<-SCRIPT
sudo cp /vagrant/.vagrant/machines/#{hostname}/virtualbox/private_key /vagrantkeys/#{hostname}-private_key
SCRIPT

  $controller_shell_provision_script += $known_host
end

$controller_shell_provision_script += <<-SCRIPT

sudo chown vagrant. /vagrantkeys/*key
sudo chmod 0700 /vagrantkeys/*key

SCRIPT

for ipaddress in ["192.168.3.11", "192.168.3.12", "192.168.3.13", "192.168.3.14", "192.168.3.15"]
  $known_host = <<-SCRIPT
sudo -u vagrant ssh-keygen -R #{ipaddress}
sudo -u vagrant bash -c "ssh-keyscan -H #{ipaddress} >> /home/vagrant/.ssh/known_hosts"
SCRIPT

  $controller_shell_provision_script += $known_host
end

# puts $controller_shell_provision_script
# exit


Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.define "cm-zabbix" do |cfg|
    cfg.vm.network "private_network", ip: "192.168.3.11"

    cfg.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "1024"
      vb.name = "cm-zabbix"
    end
  end

  config.vm.define "cm-zabbix-proxy" do |cfg|
    cfg.vm.network "private_network", ip: "192.168.3.12"

    cfg.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "1024"
      vb.name = "cm-zabbix-proxy"
    end
  end

  config.vm.define "cm-icinga" do |cfg|
    cfg.vm.network "private_network", ip: "192.168.3.13"

    cfg.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "1024"
      vb.name = "cm-icinga"
    end
  end

  config.vm.define "cm-smokeping" do |cfg|
    cfg.vm.network "private_network", ip: "192.168.3.14"

    cfg.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "1024"
      vb.name = "cm-smokeping"
    end
  end

  config.vm.define "cm-smokeping-slave" do |cfg|
    cfg.vm.network "private_network", ip: "192.168.3.15"

    cfg.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "1024"
      vb.name = "cm-smokeping-slave"
    end
  end

  # define a controller to run ansible
  config.vm.define "cm-controller", primary: true do |cfg|
    cfg.vm.network "forwarded_port", guest: 22, host: 3022, host_ip: "127.0.0.1"
    cfg.vm.network "private_network", ip: "192.168.3.10"

    cfg.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "1024"
      vb.name = "cm-controller"
    end

    config.vm.provision "shell", inline: $controller_shell_provision_script

    config.vm.provision "ansible_local" do |ansible|
      ansible.verbose = true
      ansible.limit = "all"
      ansible.provisioning_path = '/vagrant/ansible'
      ansible.playbook = "site.yml"
      ansible.inventory_path = "staging.ini"
    end
  end
end
