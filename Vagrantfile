smb_user = ENV['VAGRANT_SMB_USER']
smb_pass = ENV['VAGRANT_SMB_PASS']
Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.box_check_update = false
    config.vm.provider "virtualbox" do |v|
      v.memory = 4048
    end

    config.vm.define "node1" do |machine|
      machine.vm.network "private_network", ip: "172.17.177.12"
      machine.vm.synced_folder '.', '/vagrant', disabled: true
    end

    config.vm.define 'manager' do |machine|
      machine.vm.network "private_network", ip: "172.17.177.11"
      machine.vm.synced_folder ".", "/vagrant", type: "smb", smb_username: smb_user, smb_password: smb_pass, mount_options: ["username=#{smb_user}","password=#{smb_pass}"]
      machine.vm.provision :ansible_local do |ansible|
        ansible.playbook       = "provision.yml"
        ansible.verbose        = true
        ansible.install        = true
        ansible.limit          = "all" # or only "nodes" group, etc.
        ansible.inventory_path = "inventory"
      end
    end

end