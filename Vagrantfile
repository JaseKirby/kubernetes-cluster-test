smb_user = ENV['VAGRANT_SMB_USER']
smb_pass = ENV['VAGRANT_SMB_PASS']
Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.provider "hyperv" do |h|
        h.enable_virtualization_extensions = true
        h.differencing_disk = true
        h.memory = 2048
    end
    config.vm.box_check_update = false
    config.vm.network "private_network"
    config.vm.synced_folder ".", "/vagrant", type: "smb", smb_username: smb_user, smb_password: smb_pass, mount_options: ["username=#{smb_user}","password=#{smb_pass}"]
    config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "provision.yml"
        ansible.groups = {
            "all" => ["default"],
            "kube-masters" => ["default"],
            "kube-nodes" => ["default"]
        }
    end
  end