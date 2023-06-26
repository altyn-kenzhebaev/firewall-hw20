# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter1 => {
        :box_name => "centos/7",
        :nets => {
                  :net2 => {
                    :ip => '192.168.255.1', 
                    :adapter => 2, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "router1-net-central"
                  },
                 }
  },

  :inetRouter2 => {
        :box_name => "centos/7",
        :nets => {
                  :net2 => {
                    :ip => '192.168.255.5', 
                    :adapter => 2, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "router2-net-central"
                  },
                 }
  },

  :centralRouter => {
        :box_name => "centos/7",
        :nets => {
                  :net2 => {
                    :ip => '192.168.255.2', 
                    :adapter => 2, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "router1-net-central"
                  },
                  :net3 => {
                    :ip => '192.168.255.6',
                    :adapter => 3,
                    :netmask => "255.255.255.252",
                    :virtualbox__intnet => "router2-net-central"
                  },
                  :net4 => {
                    :ip => '192.168.0.1', 
                    :adapter => 4, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "router-central"
                  },
                }
  },
  
  :centralServer => {
        :box_name => "centos/7",
        :nets => {
                  :net2 => {
                    :ip => '192.168.0.2', 
                    :adapter => 2, 
                    :netmask => "255.255.255.252", 
                    :virtualbox__intnet => "router-central"
                  },                  
                }
  },

}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        boxconfig[:nets].each do |netname, netconf|
          box.vm.network "private_network", 
          ip: netconf[:ip], 
          adapter: netconf[:adapter], 
          netmask: netconf[:netmask], 
          virtualbox__intnet: netconf[:virtualbox__intnet]
        end

        if boxname.to_s  == 'inetRouter2'
          box.vm.network "forwarded_port", guest: 8080, host: 8080, host_ip: "127.0.0.1"
        end

        box.vm.provision "ansible" do |ansible|
          ansible.playbook = 'ansible/' + boxname.to_s + '.yml'
          ansible.compatibility_mode = "2.0"
        end

      end

  end

end
