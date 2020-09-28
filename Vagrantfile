BOX_IMAGE = "centos/8"
NODE_COUNT = 2
Vagrant.configure("2") do |config|
    config.vm.define "master" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "master"
      subconfig.vm.network :private_network, ip: "172.16.10.10"

      subconfig.vm.provider "virtualbox" do |vb|
        vb.name = "master"
        vb.cpus = 2
        vb.memory = "2048"
      end

    end
    
    (1..NODE_COUNT).each do |i|
      config.vm.define "node#{i}" do |subconfig|
        subconfig.vm.box = BOX_IMAGE
        subconfig.vm.hostname = "node#{i}"
        subconfig.vm.network :private_network, ip: "172.16.10.#{i + 10}"

        subconfig.vm.provider "virtualbox" do |vb|
          vb.name = "node#{i}"
          vb.cpus = 1
          vb.memory = "1024"
        end
      end
    end

    # Install avahi on all machines 
    # Chạy file install-docker-kube.sh sau khi nạp Box
    config.vm.provision "shell", path: "./install-docker-kube.sh" 
    # Chạy các lệnh shell
    config.vm.provision "shell", inline: <<-SHELL
      # Đặt pass 123 có tài khoản root và cho phép SSH
      echo "123" | passwd --stdin root
      sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
      systemctl reload sshd
    SHELL
    
  end

