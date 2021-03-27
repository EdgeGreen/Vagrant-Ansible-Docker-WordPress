# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_API_VERSION = "2"

# System install
$system_install_script = <<-SCRIPT
apt-get update -y
apt install python3-pip -y
apt install sshpass -y
pip3 install ansible
cd /home/vagrant/ansible/
ansible-playbook play.yml
hostname -I
SCRIPT

Vagrant.configure(VAGRANT_API_VERSION) do |config|
    config.vm.box = "generic/ubuntu2004"  
    config.vm.provider "hyperv"
    config.vm.boot_timeout = 600
    config.vm.graceful_halt_timeout = 600  

    config.vm.hostname = "wordpress"
    config.vm.synced_folder "./ansible", "/home/vagrant/ansible/", disabled: false

    # SSH Configuration
	config.ssh.username = "vagrant"
	config.ssh.password = "vagrant"
    config.ssh.insert_key = "false"
	
    # Sytem install (root)
    config.vm.provision "shell",
        inline: $system_install_script,
        privileged: true 
     
    # Hyper-V
    config.vm.network "forwarded_port", guest: 8086, host: 8086
    config.vm.provider "hyperv" do |hv, override|
        hv.linked_clone = true
		
        hv.vmname = config.vm.hostname
        hv.memory = 2048
        hv.maxmemory = 4096
        hv.cpus = 4
		
        hv.vm_integration_services = {
            guest_service_interface: true,
            time_synchronization: true
        }
    #-----------------------------------------------OPTIONAL------------------------------------------------------------------    
    # LocaL ansible provisioner configuration
    # config.vm.provision "ansible_local" do |ansible|
    # ansible.become = true    
    # ansible.provisioning_path = "/home/vagrant/ansible/"
    # ansible.install_mode = "pip3"
    # ansible.version = "2.9.6"
    # ansible.compatibility_mode = "2.0"
    # ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
    # ansible.inventory_path = "hosts"
    # ansible.playbook = "play.yml"
    # ansible.limit = "all"
    #  end

    # Ansible provisioner configuration
    # config.vm.provision "ansible", run: "always" do |ansible|
    # ansible.become = true
    # ansible.playbook = "play.yml"
	# ansible.extra_vars = {
    #     ansible_connection: "ssh",
    #     ansible_user: "vagrant",
    #     ansible_ssh_pass: "vagrant",
    #   }
    #  end
    #-------------------------------------------------------------------------------------------------------------------------
    end  
end
