Vagrant.configure("2") do |config|
  
  # ----------------------------------------------------
  # 1. NÓ DE CONTROLE (Onde vais instalar e rodar o Ansible)
  # ----------------------------------------------------
  config.vm.define "control" do |subconfig|
    subconfig.vm.box = "generic/rocky9"
    subconfig.vm.hostname = "control.lab.com"
    subconfig.vm.network "private_network", ip: "192.168.56.10"
    
    subconfig.vm.provider "virtualbox" do |v|
      v.name = "rh294-control"
      v.memory = 2048 # 2GB para o nó de controle rodar o ansible-navigator à vontade
      v.cpus = 2
    end

    # Criar o utilizador 'automation' e configurar o sudo sem senha automaticamente
    subconfig.vm.provision "shell", inline: <<-SHELL
      useradd -m -G wheel automation
      echo "automation:redhat" | chpasswd
      echo "automation ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/automation
    SHELL
  end

  # ----------------------------------------------------
  # 2. NÓS GERENCIADOS (Os alvos da automação)
  # ----------------------------------------------------
  (1..2).each do |i|
    config.vm.define "node#{i}" do |subconfig|
      subconfig.vm.box = "generic/rocky9"
      subconfig.vm.hostname = "node#{i}.lab.com"
      subconfig.vm.network "private_network", ip: "192.168.56.#{10 + i}"
      
      subconfig.vm.provider "virtualbox" do |v|
        v.name = "rh294-node#{i}"
        v.memory = 1024 # 1GB é suficiente para os nós gerenciados
        v.cpus = 1
      end

      # Criar o mesmo utilizador e configurar o sudoers
      subconfig.vm.provision "shell", inline: <<-SHELL
        useradd -m -G wheel automation
        echo "automation:redhat" | chpasswd
        echo "automation ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/automation
      SHELL
    end
  end

end