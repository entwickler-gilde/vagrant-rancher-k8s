$default_bridge = "en0: Ethernet"

$master_script = <<SCRIPT
	yum update -y
	
	yum install -y deltarpm epel-release
	yum install -y htop nano  
	yum install -y yum-utils device-mapper-persistent-data lvm2
	yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
	
	yum makecache fast
	yum install -y docker-ce-17.09.1.ce-1.el7.centos.x86_64

	echo `hostname`
	echo `ip addr show | grep eth1`
	
	groupadd docker
	usermod -aG docker vagrant
	
	echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf
	echo "net.ipv6.conf.all.forwarding=1" >> /etc/sysctl.conf
	echo "net.ipv6.bindv6only=0" >> /etc/sysctl.conf
	
	sysctl -w "net.ipv4.ip_forward=1"

	service docker start
	chkconfig docker on
	
	docker run -d --restart=always -p 8080:8080 rancher/server:preview

SCRIPT

$slave_script = <<SCRIPT
	yum update -y
	
	yum install -y deltarpm epel-release
	yum install -y htop nano  
	yum install -y yum-utils device-mapper-persistent-data lvm2
	yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
	
	yum makecache fast
	yum install -y docker-ce-17.09.1.ce-1.el7.centos.x86_64

	echo `hostname`
	echo `ip addr show | grep eth1`
	
	groupadd docker
	usermod -aG docker vagrant
	
	echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf
	echo "net.ipv6.conf.all.forwarding=1" >> /etc/sysctl.conf
	echo "net.ipv6.bindv6only=0" >> /etc/sysctl.conf
	
	sysctl -w "net.ipv4.ip_forward=1"

	service docker start
	chkconfig docker on

SCRIPT

Vagrant.configure("2") do |config|
 
  config.vm.define :master do |master|
    master.vm.box = "bento/centos-7"    
    master.vm.provider :virtualbox do |v|
		v.name = "k8s-master"
      	v.customize ["modifyvm", :id, "--memory", "2048"]
  		v.memory = 2048
  		v.cpus = 2
    end  
  	master.vm.network "public_network", bridge: $default_bridge, use_dhcp_assigned_default_route: true    
  	master.vm.hostname = "k8s-master"
    master.vm.provision :shell, :inline => $master_script    
  end

  config.vm.define :node1 do |node1|
    node1.vm.box = "bento/centos-7"    
    node1.vm.provider :virtualbox do |v|
      	v.name = "k8s-node1"
      	v.customize ["modifyvm", :id, "--memory", "4096"]
  		v.memory = 4096
  		v.cpus = 2
    end  
  	node1.vm.network "public_network", bridge: $default_bridge, use_dhcp_assigned_default_route: true    
  	node1.vm.hostname = "k8s-node1"
    node1.vm.provision :shell, :inline => $slave_script    
  end

  config.vm.define :node2 do |node2|
    node2.vm.box = "bento/centos-7"    
    node2.vm.provider :virtualbox do |v|
      	v.name = "k8s-node2"
      	v.customize ["modifyvm", :id, "--memory", "4096"]
  		v.memory = 4096
  		v.cpus = 2
    end 
  	node2.vm.network "public_network", bridge: $default_bridge, use_dhcp_assigned_default_route: true
	node2.vm.hostname = "k8s-node2"        
   node2.vm.provision :shell, :inline => $slave_script    
  end

  config.vm.define :node3 do |node3|
    node3.vm.box = "bento/centos-7"    
    node3.vm.provider :virtualbox do |v|
      	v.name = "k8s-node3"
      	v.customize ["modifyvm", :id, "--memory", "4096"]
  		v.memory = 4096
  		v.cpus = 2
    end  
  	node3.vm.network "public_network", bridge: $default_bridge, use_dhcp_assigned_default_route: true   
	node3.vm.hostname = "k8s-node3"   
   node3.vm.provision :shell, :inline => $slave_script         
  end

end    