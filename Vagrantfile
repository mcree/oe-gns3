# Install vagrant-disksize to allow resizing the vagrant box disk.
unless Vagrant.has_plugin?("vagrant-disksize")
    raise  Vagrant::Errors::VagrantError.new, "vagrant-disksize plugin is missing. Please install it using 'vagrant plugin install vagrant-disksize' and rerun 'vagrant up'"
end

unless Vagrant.has_plugin?("vagrant-vbguest")
    raise  Vagrant::Errors::VagrantError.new, "vagrant-vbguest plugin is missing. Please install it using 'vagrant plugin install vagrant-vbguest' and rerun 'vagrant up'"
end


Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = true

     # Customize the amount of memory on the VM:
     vb.memory = "1536" # 1.5G
  end

  config.vm.provision "shell", inline: <<-SHELL
    DEBIAN_FRONTEND=noninteractive apt-get -y install python3 python3-distutils
    update-alternatives --install /usr/bin/python python /usr/bin/python3.6 10
  SHELL

  #config.vm.network "private_network", type: "dhcp"
  config.vm.network "private_network", virtualbox__intnet: "LAN1", :mac => "4e3fe4e80001", auto_config: false
  config.vm.network "private_network", virtualbox__intnet: "LAN4", :mac => "4e3fe4e80004", auto_config: false
  config.vm.network "private_network", virtualbox__intnet: "LAN5", :mac => "4e3fe4e80005", auto_config: false

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = ".build/playbook.yml"
    ansible.become = true
    ansible.compatibility_mode = "2.0"
    ansible.version = "2.9.4"
    ansible.install = true
    ansible.install_mode = "pip"
  end

end