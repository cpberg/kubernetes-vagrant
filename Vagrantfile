IMAGE_NAME = "centos/8"
N=2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false 
    	
    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end

    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.50.10"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "k8deploy/master-playbook.yml"
            ansible.become = true
            ansible.extra_vars = {
                node_ip: "192.168.50.10",
            }
        end
    end
    (1..N).each do |i|
        config.vm.define "k8s-slave"+i.to_s do |slave|
            slave.vm.box = IMAGE_NAME
            slave.vm.network "private_network", ip: "192.168.50.#{i + 10}"
            slave.vm.hostname = "k8s-slave"+i.to_s
            slave.vm.provision "ansible" do |ansible|
                ansible.playbook = "k8deploy/slave-playbook.yml"
                ansible.extra_vars = {
                    slave_ip: "192.168.50.#{i + 10}",
                }
            end
        end
    end
end

