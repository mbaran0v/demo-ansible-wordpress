# -*- mode: ruby -*-
# vi: set ft=ruby :

require "yaml"

servers = YAML.load_file('ansible/group_vars/all')

DOMAIN = servers["vagrant"]["common"]["domain"]

Vagrant.configure(2) do |config|

    config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true

    servers["vagrant"]["instances"].each do |machine|

        machine_name = machine["name"] + "." + DOMAIN
        
        config.vm.define machine_name do |machine_config|

            machine_config.vm.hostname = machine_name
            machine_config.vm.box = machine["box"]

            if machine.has_key?("interfaces")
                interfaces = machine["interfaces"]

                interfaces.each do |int|
                    if int["type"] == "private_network"
                        machine_config.vm.network int["type"], ip: int["ip"], virtualbox__intnet: int["name"]
                    end
                end
            end

            if machine.has_key?("forward")
                machine["forward"].each do |port|
                    machine_config.vm.network "forwarded_port", guest: port["guest"], host: port["host"], protocol: port["proto"]
                end
            end
      
            machine_config.vm.provider "virtualbox" do |vb|
                vb.name = machine_name
                vb.customize ["modifyvm", :id, "--memory", machine["ram"]]
                vb.customize ["modifyvm", :id, "--cpus", machine["cpu"]]
            end

            config.vm.provision "ansible" do |ansible|
                ansible.groups = {
                    machine["group"] => machine_name
                }
                ansible.playbook = "ansible/main.yaml"
            end

        end # config.vm.define

    end # servers.each

end # Vagrant.configure
