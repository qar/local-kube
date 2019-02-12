# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.synced_folder "./shared", "/data"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "master-node"

    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.memory = "2048"

    vb.cpus = "2"
  end

  config.vm.provision "shell", inline: <<-SHELL
    cat /data/apt-key.gpg | sudo apt-key add
    sudo sh -c "echo 'deb http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial main' > /etc/apt/sources.list.d/kubernetes.list"
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm kubectl
    sudo apt-mark hold kubelet kubeadm kubectl

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
    sudo apt install -y docker-ce
    sudo sh -c 'echo "{ \"registry-mirrors\": [\"http://f1361db2.m.daocloud.io\"] }" > /etc/docker/daemon.json'
    sudo service docker restart

    sudo sh /data/pull-images.sh

    sudo kubeadm init --pod-network-cidr=192.168.0.0/16

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

    kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/rbac-kdd.yaml
    kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml
  SHELL
end
