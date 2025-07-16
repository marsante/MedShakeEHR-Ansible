machines=[
  {
    :hostname => "medshakeehr",
    :network => "private_network",
    :ip => "192.168.56.11",
    # # For testing VPN
    # :network => "public_network",
    # :ip => "192.168.1.17",
    :box => "generic/debian12",
    :ram => 512,
    :cpu => 2,
  }
  #,
  # # For testing VPN
  # {
  #   :hostname => "client",
  #   :network => "private_network",
  #   :ip => "192.168.56.12",
  #   :box => "debian/bookworm64",
  #   :ram => 512,
  #   :cpu => 2,
  # }
]

Vagrant.configure("2") do |config|

  machines.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
      # libvirt can't attach wireless interface to a bridge 
        #https://wiki.libvirt.org/page/Networking#Bridged_networking_.28aka_.22shared_physical_device.22.29
        
      node.vm.network  machine[:network], :ip => machine[:ip] #, :dev => "virbr0", :mode => "bridge", :type => "bridge"

      node.vm.provider "virtualbox" do |v|
        v.memory = machine[:ram]
        v.cpus = machine[:cpu]
      end

      node.vm.provider "libvirt" do |lb|
        lb.memory = machine[:ram]
        lb.cpus = machine[:cpu]
      end
      
      node.vm.provision "ansible" do |ansible|
        ansible.limit = machine[:hostname]
        ansible.playbook = "playbook.yml"
        ansible.become = true
        ansible.galaxy_role_file = "requirements.yml"
        ansible.raw_arguments = ['-D']
        ansible.groups = {
          "medshakeehr" => ["medshakeehr"],
          "client"  => ["client"]
        }
      end
    end 
  end
end
