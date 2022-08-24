machines=[
  {
    :hostname => "medshakeehr",
    :ip => "192.168.1.17",
    :box => "generic/debian11",
    :ram => 512,
    :cpu => 2,

  },
  {
    :hostname => "client",
    :ip => "192.168.1.18",
    :box => "generic/debian11",
    :ram => 512,
    :cpu => 2,
  }
]

Vagrant.configure("2") do |config|
  N = 2
  (1..N).each do |machine_id|
    machines.each do |machine|
      config.vm.define machine[:hostname] do |node|
        node.vm.box = machine[:box]
        node.vm.hostname = machine[:hostname]
        # libvirt can't attach wireless interface to a bridge 
        #https://wiki.libvirt.org/page/Networking#Bridged_networking_.28aka_.22shared_physical_device.22.29
        node.vm.network  "public_network", bridge: "wlp0s20f3"
        node.vm.provider "libvirt" do |lb|
          lb.memory = machine[:ram]
          lb.cpus = machine[:cpu]
        end
        node.vm.provider "virtualbox" do |v|
          v.memory = machine[:ram]
          v.cpus = machine[:cpu]
        end
      end 
    end
    if machine_id == N
      config.vm.provision "ansible" do |ansible|
        ansible.limit = "all"
        ansible.playbook = "playbook.yml"
        inventory_path = "hosts.yml"
        ansible.become = true
        verbose = "-vvv"
      end
    end
  end
end
