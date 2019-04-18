# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_version = "20190411.0.0"

  # set up the 3 node Consul servers
  (1..3).each do |i|
    config.vm.define "consul#{i}" do |c1|
      c1.vm.hostname = "c#{i}"
      c1.vm.network "private_network", ip: "10.100.1.1#{i}"
      c1.vm.provision "shell", path: "setupConsulServer.sh"
    end
  end
  
  # Consul UI would be available at port 8500 of any of the servers e.g., http://10.100.1.11:8500 
  # UI will only work if a leader is elected successfully

  # set up the 2 node Vault servers
  (1..2).each do |i|
    config.vm.define "vault#{i}" do |v1|
      v1.vm.hostname = "v#{i}"
      v1.vm.network "private_network", ip: "10.100.2.1#{i}"
      v1.vm.provision "shell", path: "setupConsulClient.sh"

      v1.vm.provision "shell", path: "setupVaultServer.sh"
    end
  end

end
