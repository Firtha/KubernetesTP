Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/bionic64"

    config.vm.provider "virtualbox" do |v|
        v.name = "Docker-PlayGround"
        v.memory = 4096
    end

    config.vm.network "forwarded_port", guest: 8081, host: 8081

    config.vm.network "forwarded_port", guest: 8080, host: 8080
    
    config.vm.provision :shell, path: "provisionning.sh"
end