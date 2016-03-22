# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox"
  config.vm.provider "vmware_fusion"
  config.vm.box = "dockpack/centos6"
  config.vm.box_url = "https://atlas.hashicorp.com/dockpack/boxes/centos6"
  config.vm.box_check_update = true
  config.ssh.forward_agent = false
  config.ssh.insert_key = false

  # Timeouts
  config.vm.boot_timeout = 900
  config.vm.graceful_halt_timeout=100

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "vagrant.yml"
    ansible.verbose = "vv"
    ansible.host_key_checking = "false"
  end

  config.vm.define :example,  primary: true do |example_config|

    # This host only network for use of Apache
    example_config.vm.network "private_network", ip: "192.168.10.10", :netmask => "255.255.255.0",  auto_config: true
    # To access this host use: 'vagrant ssh dev'
    example_config.vm.network "forwarded_port", id: 'ssh', guest: 22, host: 2222, auto_correct: true
    example_config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024", "--natnet1", "172.16.1/24"]
      vb.customize ["modifyvm", :id, "--ioapic", "on"  ]
      vb.name = "example"
      vb.gui = false
    end

  end
end
