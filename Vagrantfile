Vagrant.configure("2") do |config|
    config.vm.box = "debian/bullseye64"
    config.vm.define 'medshakeansible'
    config.vm.hostname = "msehr.local"
    #config.vm.network "private_network", ip: "192.168.56.4"
    config.vm.network "public_network", ip: "192.168.1.54"
    config.vm.synced_folder ".", "/vagrant", type: "rsync"

    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.become = true
      ansible.become_user = "root"
    end

    # # Custom VM config
    # config.vm.provider "virtualbox" do |v|
    #   v.memory = 1024
    #   v.cpus = 4
    # end

    # config.vm.provider "libvirt" do |libvirt|
    #   libvirt.memory = 1024
    #   libvirt.cpus = 4
    # end

    # # Custom ssh key
    config.ssh.insert_key = false
    config.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key"]
    config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"

  end
