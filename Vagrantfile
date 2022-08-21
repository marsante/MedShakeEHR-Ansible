machines=[
  {
    :hostname => "medshakeehr",
    :ip => "192.168.1.17",
    :box => "generic/debian11",
    :ram => 512,
    :cpu => 2,
    :waddress => "10.9.0.1/32",
    :wendpoint => "medshakeehr",
    :wallowedip => "10.9.0.2"
  },
  {
    :hostname => "client",
    :ip => "192.168.1.18",
    :box => "generic/debian11",
    :ram => 512,
    :cpu => 2,
    :waddress => "10.9.0.2/32",
    :wendpoint => "medshakeehr",
    :wallowedip => "10.9.0.1"
  }
]

Vagrant.configure("2") do |config|
  machines.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
      # libvirt can't attach wireless interface to a bridge 
      #https://wiki.libvirt.org/page/Networking#Bridged_networking_.28aka_.22shared_physical_device.22.29
      node.vm.network  "public_network", bridge: "wlp0s20f3"
      node.hostmanager.enabled = true
      node.hostmanager.manage_host = true
      node.hostmanager.manage_guest = true
      node.vm.provider "libvirt" do |lb|
        lb.memory = machine[:ram]
        lb.cpus = machine[:cpu]
      end
      node.vm.provider "virtualbox" do |v|
        v.memory = machine[:ram]
        v.cpus = machine[:cpu]
      end
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.become = true
        ansible.become_user = "root"
        verbose = "-vvv"
        ansible.extra_vars = {
          wireguard_address: machine[:waddress],
          wireguard_allowed_ips: machine[:wallowedip],
          wireguard_endpoint: machine[:wendpoint],
          wireguard_persistent_keepalive: "30",
          wireguard_interface_restart: true

       }
       end
    end
  end
end
