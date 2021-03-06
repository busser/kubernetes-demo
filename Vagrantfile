# -*- mode: ruby *-*
# vi: set ft=ruby

if Vagrant::VERSION == '1.8.5'
  ui = Vagrant::UI::Colored.new
  ui.error 'Unsupported Vagrant Version: 1.8.5'
  ui.error 'Version 1.8.5 introduced an SSH key permissions bug, please upgrade to version 1.8.6+'
  ui.error ''
end

Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.ssh.insert_key = false
  config.vm.box_check_update = false

  config.vm.box = "centos/7"
  config.vm.box_version = "=1708.01" # RedHat 7.4

  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "kvm"
    libvirt.uri = "qemu:///system"
  end

  cluster = {
    "master-1" => { :ip => "10.10.10.11", :cpus => 2, :memory => 2048 },
    "worker-1" => { :ip => "10.10.10.21", :cpus => 1, :memory => 4096, :disk => "50G" },
    "worker-2" => { :ip => "10.10.10.22", :cpus => 1, :memory => 4096, :disk => "50G" },
    "worker-3" => { :ip => "10.10.10.23", :cpus => 1, :memory => 4096, :disk => "50G" },
    "client-1" => { :ip => "10.10.10.31", :cpus => 1, :memory => 2048 },
  }

  cluster.each do | hostname, specs |
    config.vm.define hostname do |node|
      node.vm.hostname = hostname
      node.vm.network :private_network, ip: specs[:ip]
      node.vm.provider :libvirt do |libvirt|
        libvirt.cpus   = specs[:cpus]
        libvirt.memory = specs[:memory]
        if specs.key?(:disk)
          libvirt.storage :file, :size => specs[:disk]
        end
      end
    end
  end

end
