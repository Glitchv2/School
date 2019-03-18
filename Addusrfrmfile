Vagrant.configure("2") do ||config||
	config.vm.define "pihole" do |pihole|
		pihole.vm.box="ubuntu/xenial64"
		pihole.vm.network :forwarded_port, guest: 22, host: 10100, id: "ssh"
		pihole.vm.hostname = "pihole"
		config.vm.network "private_network", ip: "192.168.10.2"
		pihole.vm.provision "shell", privileged: true, inline: <<-SHELL
			sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
			sudo mkdir /etc/pihole/
			sudo apt-get update -y
			sudo apt-get install -y netcat unzip idn2 sqlite3 dns-root-data lighttpd php5-common php5-cgi php5-sqlite
			sudo echo -e "WEBPASSWORD=a215bae8b5ec659b0980a76dlkds09644731cd439cab41494447a8705c22b3aa41c\nPIHOLE_INTERFACE=enp0s3\nIPV4_ADDRESS=192.168.10.1/24\nQUERY_LOGGING=true\nINSTALL_WEB=true\nDNSMASQ_LISTENING=single\nPIHOLE_DNS_1=8.8.8.8\nPIHOLE_DNS_2=1.1.1.1\nDNS_FQDN_REQUIRED=true\nDNS_BOGUS_PRIV=true\nDNSSEC=true\nTEMPERATUREUNIT=C\nWEBUIBOXEDLAYOUT=traditional\nAPI_EXCLUDE_DOMAINS=\nAPI_EXCLUDE_CLIENTS=\nAPI_QUERY_LOG_SHOW=all\nAPI_PRIVACY_MODE=false" >> /etc/pihole/setupVars.conf
			
			sudo curl -L https://install.pi-hole.net | sudo bash /dev/stdin --unattended
		
			SHELL
	end
		
	config.vm.define "client1" do |client1|
		client1.vm.box="ubuntu/xenial64"
		client1.vm.network :forwarded_port, guest: 22, host: 10101, id: "ssh"
		config.vm.network "private_network", ip: "192.168.10.100", dns: "192.168.10.2"
		client1.vm.provision "shell", privileged: true, inline: <<-SHELL
		sudo apt-get update
		sudo apt-get install -y lynx
		clear
		lynx -accept_all_cookies 192.168.10.2/admin

		SHELL
	end
	
end
