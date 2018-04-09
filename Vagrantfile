
Vagrant.configure(2) do |config|
	config.vm.define "webserver" do |ws|
		ws.vm.box = "ubuntu/xenial64"
		ws.vm.define "m239-ws-0330.1608"
		ws.vm.hostname = "m239ws"
		ws.vm.network "public_network",ip:"10.71.10.15",default_gateway:"10.71.10.254"
		ws.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
		ws.vm.synced_folder "./web", "/var/www/html"
		ws.vm.provider "virtualbox" do |vb|
			vb.name = "m239-ws-0330.1608"
			vb.gui = false
			vb.memory = "1536"
			vb.cpus = "2"
		end
		ws.vm.provision "shell", inline: <<-SHELL
			sudo apt -y update && sudo apt -y upgrade
			sudo apt -y install ufw unzip nginx
			sudo service nginx restart
			cd /var/www/html && wget https://is.gd/ofohux && unzip download
			sudo groupadd admin && sudo useradd admin -g admin -m -s /bin/bash
			sudo chpasswd <<<admin:Admin_123
		#	sudo service nginx stop
		#		sudo apt -y install git
		#		sudo git clone https://github.com/certbot/certbot /var/www/letsencr
		#		cd /var/www/letsencr && ./letsencrypt-auto certonly --standalone
		#	sudo service nginx restart
			sudo ufw enable
			sudo ufw default deny incoming
			sudo ufw default allow outgoing
			sudo ufw allow 22/tcp
			sudo ufw allow 2222/tcp
			sudo ufw allow 80/tcp
			sudo ufw allow 8080/tcp
			sudo ufw allow 443/tcp
			sudo ufw allow 8081/tcp
			sudo ufw --force disable && sudo ufw --force enable
			sudo reboot now
		SHELL
	end
end
