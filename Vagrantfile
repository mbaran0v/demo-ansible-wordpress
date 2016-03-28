# -*- mode: ruby -*-
# vi: set ft=ruby :

DOMAIN = "test.lab"

servers = [
    {
        :name => "lb01." + DOMAIN,
        :eth1 => "10.73.0.11",
        :tpl => "centos/7",
        :ram => 1024,
        :cpu => 2,
        :group => "frontend",
        :forward => { 8037 => 80 }
    },
    {
        :name => "db01." + DOMAIN,
        :eth1 => "10.73.0.31",
        :tpl => "centos/7",
        :ram => 1024,
        :cpu => 2,
        :group => "database"
    },
    {
        :name => "app01." + DOMAIN,
        :eth1 => "10.73.0.21",
        :tpl => "centos/7",
        :ram => 1024,
        :cpu => 2,
        :group => "backend"
    },
    {
        :name => "app02." + DOMAIN,
        :eth1 => "10.73.0.22",
        :tpl => "centos/7",
        :ram => 1024,
        :cpu => 2,
        :group => "backend"
    }
]

Vagrant.configure(2) do |config|

    config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true

    servers.each do |machine|
        
        config.vm.define machine[:name] do |node|
        
            node.vm.box = machine[:tpl]
            node.vm.hostname = machine[:name]
      
            node.vm.network "private_network", ip: machine[:eth1], virtualbox__intnet: DOMAIN

            if machine.has_key?(:forward)
                machine[:forward].each do |host_port,guest_port|
                    node.vm.network "forwarded_port", guest: guest_port, host: host_port
                end
            end
      
            node.vm.provider "virtualbox" do |vb|
                vb.name = machine[:name]
                vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
                vb.customize ["modifyvm", :id, "--cpus", machine[:cpu]]
            end

            config.vm.provision "ansible" do |ansible|
                ansible.groups = {
                    machine[:group] => machine[:name]
                }
                ansible.playbook = ".ansible/main.yaml"
            end

        end # config.vm.define

    end # servers.each

end # Vagrant.configure
