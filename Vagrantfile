Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"

    # Custom ssh key
    config.ssh.insert_key = false
    config.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key"]
    config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
    
    # custom VM config
    config.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 4
    end
    
    config.vm.define 'medshakeansible' do |node|
      node.vm.hostname ='msehr.local'
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.become = true
        ansible.become_user = "root"
        ansible.compatibility_mode = "2.0"
      end

    # Configure private network
    config.vm.network "private_network", ip: "55.55.55.5"
    # or public network, for Phonecapture or VPN functionality  
    #config.vm.network "public_network"   
    end
  end