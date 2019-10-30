hosts = [{name: "workspace", ip:"192.168.56.65"},{name: "web", ip:"192.168.56.66"},{name: "database",ip:"192.168.56.67"}]

pubkeypath = "shared\\ssh\\public"

Vagrant.configure("2") do |config|

    hosts.each do |host|
        config.vm.define host[:name] do |node|      
            name = host[:name]
      
            node.vm.hostname = host[:name]
            
            node.vm.network "forwarded_port", guest: 80, host: 7070 if name == "web"

            node.vm.box ="ubuntu/bionic64"

            node.vm.network "private_network",
                ip: host[:ip],
                netmask:"255.255.255.0"

            node.vm.provider :virtualbox do |vb|
                vb.name = host[:name]
            end
            config.vm.synced_folder "shared/", "/home/vagrant/shared" if name == "workspace"

            config.vm.provision "shell" do |s|
                ssh_pub_key = File.readlines(pubkeypath).first.strip
                s.inline = <<-SHELL
                        echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
                        echo #{ssh_pub_key} >> /root/.ssh/authorized_keys 
                SHELL
            end            
        end        
    end
    config.vm.define "workspace" do |node|
        node.vm.provision "shell" do |s|
            ssh_pub_key = File.readlines(pubkeypath).first.strip
            s.inline = <<-SHELL
                sudo apt update
                sudo apt install software-properties-common
                sudo apt-add-repository --yes --update ppa:ansible/ansible
                sudo apt install ansible -y
                
                echo "export ANSIBLE_HOST_KEY_CHECKING=0" >> /home/vagrant/.bashrc
                cp /home/vagrant/shared/ssh/private /home/vagrant/.ssh/
                cat /home/vagrant/shared/ansible/inventory/hosts >> /etc/ansible/hosts
                sudo chmod 644 /home/vagrant/.ssh/private
            SHELL
        end 
    end
end