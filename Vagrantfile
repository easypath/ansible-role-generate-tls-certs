# This guide is optimized for Vagrant 1.7 and above.
# Although versions 1.6.x should behave very similarly, it is recommended
# to upgrade instead of disabling the requirement below.
Vagrant.require_version ">= 1.7.0"

Vagrant.configure(2) do |config|

  config.vm.box = "debian/buster64"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/mitchellh/vagrant/issues/5005
  config.ssh.insert_key = false

  config.vm.provider :libvirt do |lv|
    lv.cpus = 1
    lv.memory = 512
  end

  config.vm.define "srv1" do |m|
    m.vm.hostname = "srv1"
    m.vm.network :private_network, ip: "192.168.123.30", libvirt__dhcp_enabled: false
  end
  config.vm.define "srv2" do |m|
    m.vm.hostname = "srv2"
    m.vm.network :private_network, ip: "192.168.123.31", libvirt__dhcp_enabled: false
  end

  config.vm.provision "ansible" do |ansible|
    #ansible.become = true
    ansible.verbose = "v"
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "inventory.yml"
  end
end
