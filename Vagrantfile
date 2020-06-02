Vagrant.configure("2") do |config|
    config.vm.box = "generic/ubuntu1804"

    config.vm.define 'ubuntu'

    # configure ssh
    config.ssh.forward_agent = true
    config.ssh.insert_key = false
    config.ssh.private_key_path = ["~/.vagrant.d/insecure_private_key"]

    # Prevent SharedFoldersEnableSymlinksCreate errors
    config.vm.synced_folder ".", "/vagrant", disabled: true

    config.vm.provider :virtualbox do |vb|
        vb.name = 'ubuntu'
        vb.memory = 256
        vb.cpus = 1
        vb.gui = false
        vb.customize ["modifyvm", :id, "--ioapic", "on"] # necessary for 2 or more vcpus
    end
end
