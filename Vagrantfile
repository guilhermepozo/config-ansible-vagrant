# DECLARAR ARRAY DE OBJETOS COM INFORMAÇÕES DAS MÁQUINAS VIRTUAIS QUE SERÃO CRIADAS 
hosts = [{name: "workspace", ip:"192.168.56.65"},{name: "web", ip:"192.168.56.66"},{name: "database",ip:"192.168.56.67"}]

# DECLARAR CAMINHO DA CHAVE PÚBLICA
pubkeypath = "shared\\ssh\\public"

Vagrant.configure("2") do |config|
    # LOOP PARA CRIAR MÁQUINAS DECLARADAS NO OBJETO "hosts" (LINHA 2)
    hosts.each do |host|
        config.vm.define host[:name] do |node|  
            # VARIÁVEL AUXILIAR COM O NOME, SERÁ USADA PARA REALIZAR AS CONDIÇÕES
            name = host[:name]

            # BOX USADA COMO BASE - https://app.vagrantup.com/boxes/search
            node.vm.box ="ubuntu/bionic64"
            
            # CONFIGURANDO HOSTNAME COM NOME DECLARADO ANTERIORMENTE (OBJETO hosts - LINHA 2) 
            node.vm.hostname = host[:name]
            
            # REDIRECT DE PORTAS CASO NOME DA MÁQUINA SEJA "web"
            node.vm.network "forwarded_port", guest: 80, host: 7070 if name == "web"

            # CRIANDO REDE PRIVADA E ATRIBUINDO IP DECLARADO ANTERIORMENTE (ARRAY DE OBJETOS "hosts" - LINHA 2) 
            node.vm.network "private_network", ip: host[:ip]

            # CONFIGURANDO NOME COM NOME DECLARADO ANTERIORMENTE (ARRAY DE OBJETOS "hosts" - LINHA 2) 
            node.vm.provider :virtualbox do |vb|
                vb.name = host[:name]
            end

            # CRIANDO PASTA COMPARTILHADA ENTRE SUA MÁQUINA E MÁQUINA VIRTUAL CASO NOME DA MÁQUINA SEJA "workspace"
            config.vm.synced_folder "shared/", "/home/vagrant/shared" if name == "workspace"

            # INSERINDO CHAVE PÚBLICA NO SISTEMA OPERACIONAL PARA SSH 
            config.vm.provision "shell" do |s|
                # LER ARQUIVO DE CHAVE PUBLICA (RUBY)
                ssh_pub_key = File.readlines(pubkeypath).first.strip
                s.inline = <<-SHELL
                        echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
                        echo #{ssh_pub_key} >> /root/.ssh/authorized_keys 
                SHELL
            end            
        end        
    end
    # EXECUTAR AÇÕES DE PROVISIONAMENTO NA MÁQUINA "workspace"
    config.vm.define "workspace" do |node|
        # EXECUTAR AÇÕES DE PROVISIONAMENTO DO TIPO "shell"
        node.vm.provision "shell" do |s|
            s.inline = <<-SHELL
                # ATUALIZANDO PACOTES DO SISTEMA OPERACIONAL
                apt update
                # INSTALAÇÃO DO ANSIBLE
                apt install software-properties-common
                apt-add-repository --yes --update ppa:ansible/ansible
                apt install ansible -y                
                # IGNORAR CONFIRMAÇÃO MANUAL NO TERMINAL DE CONFIAÇA DE CHAVES NO MOMENTO DO SSH 
                echo "export ANSIBLE_HOST_KEY_CHECKING=0" >> /home/vagrant/.bashrc
                # COPIAR CHAVE PRIVADA PARA LOCAL PADRÃO SSH
                cp /home/vagrant/shared/ssh/private /home/vagrant/.ssh/
                # COPIAR ARQUIVO DE HOSTS PARA LOCAL PADRÃO DO ANSIBLE
                cat /home/vagrant/shared/ansible/inventory/hosts >> /etc/ansible/hosts
                # ALTERAR PERMISSIONAMENTO DA CHAVE PRIVADA PARA SSH
                chmod 644 /home/vagrant/.ssh/private
            SHELL
        end 
    end
end