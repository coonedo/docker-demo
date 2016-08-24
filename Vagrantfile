Vagrant.configure(2) do |config|
  config.vm.define "docker" do |docker|
    docker.vm.box = "ubuntu/trusty64"
    docker.vm.network "private_network", ip: "192.168.0.244"
    docker.vm.hostname = "docker.demo.com"
    docker.vm.provider "virtualbox" do |vb|
      vb.memory = 2048 
      vb.cpus = 2
    end 
  end
end
