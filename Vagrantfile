ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.define "inetRouter" do |node|
    node.vm.hostname = "inetRouter"
    node.vm.network "private_network", ip: "192.168.255.1", virtualbox__intnet: "router-net"
    
    node.vm.provision "shell", run: "always", inline: "sysctl -w net.ipv4.ip_forward=1"
    node.vm.provision "shell", run: "always", inline: "iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE"
  end

  config.vm.define "centralRouter" do |node|
    node.vm.hostname = "centralRouter"
    node.vm.network "private_network", ip: "192.168.255.2", virtualbox__intnet: "router-net"
    node.vm.provision "shell", run: "always", inline: "ip route add default via 192.168.255.1 || true"
    node.vm.provision "shell", run: "always", inline: "echo 'nameserver 8.8.8.8' > /etc/resolv.conf"
  end

  config.vm.define "centralServer" do |node|
    node.vm.hostname = "centralServer"
    node.vm.network "private_network", ip: "192.168.255.3", virtualbox__intnet: "router-net"
    node.vm.provision "shell", run: "always", inline: "ip route add default via 192.168.255.1 || true"
    node.vm.provision "shell", run: "always", inline: "echo 'nameserver 8.8.8.8' > /etc/resolv.conf"
  end

  config.vm.define "inetRouter2" do |node|
    node.vm.hostname = "inetRouter2"
    node.vm.network "private_network", ip: "192.168.56.10"
    node.vm.network "private_network", ip: "192.168.255.4", virtualbox__intnet: "router-net"
    node.vm.provision "shell", run: "always", inline: "sysctl -w net.ipv4.ip_forward=1"
    node.vm.provision "shell", run: "always", inline: "iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE"
    node.vm.provision "shell", run: "always", inline: "ip route add default via 192.168.255.1 dev eth2 || true"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.compatibility_mode = "2.0"
    ansible.groups = {
      "routers" => ["inetRouter", "inetRouter2"],
      "clients" => ["centralRouter"],
      "servers" => ["centralServer"]
    }
  end
end
