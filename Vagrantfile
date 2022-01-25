# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "baunegaard/win10pro-en"
    config.vm.hostname = "win10vs"
	
	#config.vm.provision "shell", inline: <<-SHELL
	#	config.winrm.username = Read-Host -Prompt 'Input your server name'
	#	config.winrm.password = Read-Host -Prompt 'Input the user name'
	#SHELL

    config.winrm.username = "vagrant"
    config.winrm.password = "vagrant"

    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.manage_guest = true
    config.hostmanager.aliases = %w(
        # Add hostnames to map here
    )

	config.vm.guest = :windows
	config.vm.communicator = :winrm      
	
	config.vm.network "private_network", type: "dhcp" 
	config.vm.network "forwarded_port", host: 33389, guest: 3389, id: "rdp", auto_correct: true

	config.ssh.password = "vagrant"
	config.ssh.username = "vagrant" 
	  
	config.vm.synced_folder '.', '/vagrant', disabled: true
  #config.vm.synced_folder "app/", "/app"
  
    config.vm.provider "parallels" do |prl, override|
        prl.name = "Win10_VS"
        prl.cpus = 4
        prl.memory = 8192
        prl.customize ["set", :id, "--efi-boot", "off"]
        prl.update_guest_tools = true
    end

    config.vm.provider :vmware_desktop do |v, override|
        v.vmx["displayName"] = "Win10_VS"
        v.vmx["numvcpus"] = "4"
        v.vmx["memsize"] = "8192"
        v.vmx["ethernet0.virtualDev"] = "vmxnet3"
        v.vmx["scsi0.virtualDev"] = "lsisas1068"
        v.enable_vmrun_ip_lookup = false
        v.whitelist_verified = true
    end

    config.vm.provider :virtualbox do |v, override|
        v.name = "WIN10_DEV_VS_DELPHIX"
        v.customize ["modifyvm", :id, "--cpus", 4]
		v.customize ["modifyvm", :id, "--vram", "256"]
        v.customize ["modifyvm", :id, "--memory", 8192]
		v.default_nic_type = "82540EM"
    end

    config.vm.provision "shell", path: "configure/pre-windowssettings.ps1"
    config.vm.provision :reload
    config.vm.provision "shell", path: "software/install.ps1"
    config.vm.provision "shell", path: "configure/post-windowssettings.ps1"
    config.vm.provision :reload
    config.vm.provision "shell", path: "configure/install-windowsupdates.ps1"
	
#PC NAME  = WIN10_DEV_VS_DELPHIX
#LOGIN    = WIN10_DEV_VS_DELPHIX\vagrant
#Password = vagrant
#Reactivate Windows(Run CMD As Administrator) => slmgr /rearm 	
end
