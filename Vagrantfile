BOX_NAME = "ubuntu/focal64"

Vagrant.configure("2") do |config|

  # HAProxy
  config.vm.define "haproxy" do |haproxy|
    haproxy.vm.box = BOX_NAME
    haproxy.vm.hostname = "haproxy"
    haproxy.vm.network "private_network", ip: "192.168.56.10", netmask: "255.255.255.0"
    
    haproxy.vm.provider "virtualbox" do |vb|
      vb.name = "haproxy"  
      vb.memory = 2048
      vb.cpus = 1
    end

    haproxy.vm.provision "shell", inline: <<-SHELL
        apt-get -y install haproxy
        cp /vagrant/haproxy.cfg /etc/haproxy/haproxy.cfg
        service haproxy restart
      SHELL
  end

  # Web servers
  (1..2).each do |i|
    
    config.vm.define "web-#{i}" do |node|

      node.vm.box = BOX_NAME
      node.vm.hostname = "web-#{i}"
      node.vm.network "private_network", ip: "192.168.56.1#{i}", netmask: "255.255.255.0"
      
      node.vm.provider "virtualbox" do |vb|
        vb.name = "web-#{i}"  
        vb.memory = 2048
        vb.cpus = 1
      end

      node.vm.provision "shell", inline: <<-SHELL
        apt-get -y install nginx
        service nginx start
        echo "<html><h1>$(hostname)</h1></html>" > /var/www/html/index.html
      SHELL

    end
  end

end
