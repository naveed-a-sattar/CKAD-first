# -*- mode: ruby -*-
# vi:set ft=ruby sw=2 ts=2 sts=2:
# Define the number of master and worker nodes
# If this number is changed, remember to update setup-hosts.sh script with the new hosts IP details in /etc/hosts of each VM.

NUM_CONTROLPLANE_NODE = 1
NUM_WORKER_NODE = 1

IP_NW = "192.168.56."
CONTROLPLANE_IP_START = 3
NODE_IP_START = 15

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/jammy64"
  config.vm.box_check_update = false

  # Provision Master Nodes

  (1..NUM_CONTROLPLANE_NODE).each do |i|
      config.vm.define "kubeControlPlane#{i}" do |node|
        # Name shown in the GUI
        node.vm.provider "virtualbox" do |vb|
            vb.name = "kubeControlPlane#{i}"
            vb.memory = 2048
            vb.cpus = 2
        end
        node.vm.hostname = "kubeControlPlane#{i}"
        node.vm.network :private_network, ip: IP_NW + "#{CONTROLPLANE_IP_START + i}"
        node.vm.network "forwarded_port", guest: 22, host: "#{2710 + i}"
        node.vm.provision "setup-hosts", :type => "shell", :path => "setup-hosts.sh" do |s|
          s.args = ["enp0s8"]
        end
        node.vm.provision "setup-dns", type: "shell", :path => "update-dns.sh"
      end
  end

  # Provision Worker Nodes

  (1..NUM_WORKER_NODE).each do |i|
    config.vm.define "kubenode#{i}" do |node|
      node.vm.provider "virtualbox" do |vb|
          vb.name = "kubenode#{i}"
          vb.memory = 2048
          vb.cpus = 2
      end
      node.vm.hostname = "kubenode#{i}"
      node.vm.network :private_network, ip: IP_NW + "#{NODE_IP_START + i}"
      node.vm.network "forwarded_port", guest: 22, host: "#{2720 + i}"
      node.vm.provision "setup-hosts", :type => "shell", :path => "setup-hosts.sh" do |s|
        s.args = ["enp0s8"]
      end
      node.vm.provision "setup-dns", type: "shell", :path => "update-dns.sh"
    end
  end
end
