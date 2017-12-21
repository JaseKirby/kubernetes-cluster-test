smb_user = ENV['VAGRANT_SMB_USER']
smb_pass = ENV['VAGRANT_SMB_PASS']
Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.box_check_update = false

    config.vm.define "node-1" do |machine|
      machine.vm.hostname = "node-1"
      machine.vm.network "private_network", ip: "172.17.177.12"
      machine.vm.synced_folder '.', '/vagrant', disabled: true
      machine.vm.provider "virtualbox" do |v|
        v.memory = 2024
      end
    end

    config.vm.define "node-2" do |machine|
      machine.vm.hostname = "node-2"
      machine.vm.network "private_network", ip: "172.17.177.13"
      machine.vm.synced_folder '.', '/vagrant', disabled: true
      machine.vm.provider "virtualbox" do |v|
        v.memory = 2024
      end
    end

    config.vm.define 'master-1' do |machine|
      machine.vm.hostname = "master-1"
      machine.vm.network "private_network", ip: "172.17.177.11"
      machine.vm.synced_folder ".", "/vagrant", type: "smb", smb_username: smb_user, smb_password: smb_pass, mount_options: ["username=#{smb_user}","password=#{smb_pass}"]
      machine.vm.provider "virtualbox" do |v|
        v.memory = 3024
      end
      machine.vm.provision :ansible_local do |ansible|
        ansible.playbook       = "test.yml"
        ansible.verbose        = true
        ansible.install        = true
        ansible.limit          = "all" # or only "nodes" group, etc.
        ansible.inventory_path = "inventory"
      end
    end

end