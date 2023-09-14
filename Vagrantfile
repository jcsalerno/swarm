Vagrant.configure("2") do |config|
  (1..3).each do |i|
      config.vm.define "swarm-#{i}" do |swarm|
          swarm.vm.box = "ubuntu/bionic64"
          swarm.vm.hostname = "node-#{i}"
          swarm.vm.network "private_network", ip: "172.89.0.1#{i}"

          swarm.ssh.insert_key = false
          swarm.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/id_rsa']
          swarm.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"

          config.vm.provision "shell", inline: <<-SHELL
            apt-get update
            apt-get install \
              ca-certificates \
              curl \
              gnupg \
              lsb-release -y
            curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
            echo \
              "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
              $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
            apt-get update
            apt-get install docker-ce docker-ce-cli containerd.io -y
            usermod -a -G docker vagrant
          SHELL
      end
  end
end