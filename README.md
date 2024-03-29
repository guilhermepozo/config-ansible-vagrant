#  Introdução ao Ansible e Vagrant

# Objetivo

Cobrir os conhecimentos básicos de Ansible e Vagrant visando dar liberdade para testa e praticar novas técnologias de forma rápida e segura em ambiente local. 

# Público Alvo

Profisionais de TI das diversas áreas e disciplinas que precisam executar tarefas administrativas, bem como aqueles que estão iniciando e tendo o primeiro contato com automação de infraestrutura.

# Sobre

Conteúdo criado para incentivar o estudo e o compartilhamento de conhecimento dentro e fora da empresa, despertar o interesse em procurar ferramentas e processos mais eficientes que possam melhorar a forma como trabalhamos. Com isso iremos gastar mais tempo planejando e menos tempo executando tarefas manuais repetitivas suscetíveis a falhas, diminuindo o risco e aumentando qualidade nor serviços e entrega

# Requisitos

- Computador com acesso à internet.
- 4 GB de memória RAM.
- Editor de texto (Visual Studio Code, Atom, Notepad ++ ...)
- Código exemplo do projeto, disponível em: https://github.com/guilhermepozo/config-ansible-vagrant

# Como executar

Na máquina host do virtualbox e vagrant:
    
    git clone https://github.com/guilhermepozo/config-ansible-vagrant.git

    cd config-ansible-vagrant

    vagrant up

    <!-- Conectando na máquina virtual workspace -->

    vagrant ssh workspace
  
Depois de conectar na máquina virtual **workspace**:

    ansible-playbook shared/ansible/playbook-implementation/config.yml -v

Após a execução do playbook, teste a instalação do Wordpress em http://localhost:7070/

# Observações

O par de chaves presentes em: *sared/ssh* são usadas para estabelecer uma relação de confiança entre as máquinas virtuais, somente para demonstração, caso ache necessário gere outro par.

# Considerações

Os assuntos cobertos são suficientes pra dar introdução aos principios básicos de Ansible: automações simples, diretas e não reutilzátveis.

Assuntos como: Roles, Condições, Vault, Lookups, Async, Facts serão tratados em outro momento.

Sinta-se livre para contrubuir com o repositório, implementar melhorias e submeter requests. 

Alguns pontos podem ser melhorados sem utilizar assuntos não cobertos, o diretório playbook-implementation demonstra a forma mais simples possível de criar um playbook em um único arquivo.

Outras versões desse mesmo cenário devem ser criadas seguindo melhoras práticas, como por exemplo: roles-implementation, seguindo o padrão de roles.

Agradeço á Sonda por fomentar a transformação digital olhando pra dentro de casa, 
incentivando o estudo e o compatilhamento de conhecimento, entendendo que a mudança
cultural focada em pessoas é o ponto chave.

# Próximos Passos

- [ ] Criar playbooks mais inteligentes que tratem erros e condições.
- [ ] Implementar utilizando roles.
- [ ] Criar material avançado contendo assuntos ainda não abordados do Ansible.
