NUM_WORKER=1
IP_NW="10.0.0."
IP_START=100
MASTER_IP=IP_NW + "#{IP_START}"
BOX_NAME="ubuntu/focal64"

Vagrant.configure("2") do |config|

    # Node specs
    config.vm.box = BOX_NAME
    config.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
    end

    # Install Microk8s & Docker on all nodes
    config.vm.provision "shell", inline: <<-EOF
        snap install microk8s --classic
        snap install docker
        microk8s.status --wait-ready
        microk8s.enable dns dashboard prometheus registry
        usermod -a -G microk8s vagrant
        echo "alias kubectl='microk8s.kubectl'" > /home/vagrant/.bash_aliases
        chown vagrant:vagrant /home/vagrant/.bash_aliases
        echo "alias kubectl='microk8s.kubectl'" > /root/.bash_aliases
        chown root:root /root/.bash_aliases
    EOF
  
    # Master node
    config.vm.define "master" do |master|
        master.vm.hostname = "master"
        master.vm.network "private_network", ip: MASTER_IP
        master.vm.provider "virtualbox" do |vb|
            vb.name = "master"
        end
        master.vm.provision "shell", inline: <<-EOF
            microk8s.add-node | grep #{MASTER_IP} | tee /vagrant/add_k8s.sh
            microk8s kubectl apply -f /vagrant/manifests/grafana-np.yaml
            microk8s kubectl apply -f /vagrant/manifests/prometheus-np.yaml
        EOF
        config.vm.provision "shell", run: "always", inline: "<<-EOF
            microk8s kubectl -n kube-system describe secret $(microk8s kubectl -n kube-system get secret | grep default-token | cut -d " " -f1) | grep token: | tr -s ' ' | cut -d ' ' -f 2 | tee /vagrant/token
            microk8s dashboard-proxy </dev/null &>/dev/null &"
        EOF
    end

    # Worker nodes
    (1..NUM_WORKER).each do |i|
        config.vm.define "worker#{i}" do |worker|
            worker.vm.box = BOX_NAME
            worker.vm.hostname = "worker#{i}"
            worker.vm.network "private_network", ip: IP_NW + "#{IP_START + i}"
            worker.vm.provider "virtualbox" do |v|
                v.name = "worker#{i}"
            end
            worker.vm.provision "shell", inline: <<-EOF
                bash -x /vagrant/add_k8s.sh
            EOF
        end
    end
end