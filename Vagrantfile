#Maxim Kasimov monitoring practice
ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|

  config.vm.provision "file", source: "files/test.pub", destination: "/home/vagrant/.ssh/"

  config.vm.define "controlnode" do |controlnode|
    controlnode.vm.box = "ubuntu/focal64"
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.50.40"
    controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlnode.vm.synced_folder "./files","/home/vagrant/files/"
    controlnode.vm.provision "file", source: "files/test", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      sudo add-apt-repository ppa:ansible/ansible
      sudo apt update -y && sudo apt -y install ansible
      chmod 600 /home/vagrant/.ssh/test
      chmod 644 /home/vagrant/.ssh/test.pub
    SHELL
  end

  config.vm.define "server" do |server|
    server.vm.box = "ubuntu/focal64"
    server.vm.hostname = "server"
    server.vm.network "private_network", ip: "192.168.50.41"
    server.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/test.pub
      cat /home/vagrant/.ssh/test.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end
end