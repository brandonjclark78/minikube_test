#
#  Multi machine, essential options Vagrant file example. (documentation removed)
#
# minikube_test
#

# Vagrantfile

# Determine the operating system
if Vagrant::Util::Platform.windows?
  # Windows path
  passwords_path = File.expand_path("C:\Users\brandon\OneDrive\Code\Vagrant\secrets\passwords.rb", __FILE__)
else
  # Linux path
  passwords_path = File.expand_path("/secrets/passwords.rb", __FILE__)
end

# Require the passwords.rb file
require_relative passwords_path



#require_relative 'passwords.rb'

#Include the Module name 'Secrets' in this Vagrant
include Secrets


nodes = {
"controller" => ["generic/rocky9", 2, 16384, 20 ]#,
#"controller" => ["generic/ubuntu2204", 2, 16384, 20 ]#,
#"kube-node1" => ["kalilinux/rolling", 2, 4096, 20 ],
#"kube-node2" => ["kalilinux/rolling", 2, 4096, 20 ]
}

Vagrant.configure(2) do |config|
  nodes.each do | (name, cfg) |
    box, numvcpus, memory, storage = cfg
    #config.timezone.value = :host
    config.vm.define name do |machine|
      machine.vm.box   = box
      machine.vm.hostname = name
      #machine.vm.synced_folder('.vagrant\Vagrantfiles', '/Vagrantfiles', type: 'rsync')

      machine.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname         = Secrets::ESXi_Hostname
        esxi.esxi_username         = Secrets::ESXi_Username
        esxi.esxi_password         = Secrets::ESXi_Password
        esxi.esxi_virtual_network  = "VM_PortGroup"
        esxi.guest_numvcpus        = numvcpus
        esxi.guest_memsize         = memory
        esxi.guest_storage         = storage
		    esxi.guest_disk_type       = 'thin'
        esxi.local_allow_overwrite = 'True'

      end

      #Install Ansible via Shell Provisioner
      #config.vm.provision "shell", path: "https://raw.githubusercontent.com/brandonjclark78/autobuilds/main/install_ansible_deb.sh"
      #Configure System using Ansible
      config.vm.provision "ansible" do |ansible|
        ansible.verbose = "v"
        ansible.playbook = "provisioning/playbook.yml"
      end
    end
  end
end