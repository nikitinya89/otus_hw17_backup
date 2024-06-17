Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"

  ssh_pub_key = File.readlines("../id_rsa.pub").first.strip
  common_provision_script = <<-SHELL
    echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
    echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
    sudo sed -i 's/\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart sshd
  SHELL

  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.cpus = 2
  end

  config.vm.define "backup" do |backup|
    backup.vm.hostname = "backup"
    backup.vm.network "private_network", ip: "192.168.56.101"
    backup.vm.provision "shell", inline: common_provision_script
    backup.vm.disk :disk, name: "backup_disk", size: "3GB"
  end

  config.vm.define "client" do |client|
    client.vm.hostname = "client"
    client.vm.network "private_network", ip: "192.168.56.102"
    client.vm.provision "shell", inline: common_provision_script
  end

end
