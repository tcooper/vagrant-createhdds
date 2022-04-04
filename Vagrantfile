# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  #config.vm.box = "rockylinux/8"
  #config.vm.box_version = "4.0.0"
  # NOTE: The 8.5 box for VirtualBox provider is currently broken
  #config.vm.box_version = "5.0.0"
  config.vm.box = "custom/rocky8u5"
  config.vm.hostname = "createhdds"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = "4"
  end

  config.vm.provision "shell", name: "update", privileged: true, inline: <<-SHELL
    # NOTE: Updating kernel will break vboxsf.
    dnf -y --exclude=kernel*,python3-perf,perf update
    dnf -y install epel-release
    dnf -y install git python3 libvirt-daemon-kvm python3-libvirt python3-libguestfs qemu-kvm virt-install
  SHELL

  config.vm.provision "shell", name: "clone_createhdds", privileged: false, inline: <<-SHELL
    if [[ ! -f /vagrant/factory/hdds/fixed/disk_ks.img ]]; then
      if [[ ! -d /vagrant/createhdds ]]; then
        pushd /vagrant
        git clone https://github.com/rocky-linux/createhdds.git
        popd
      fi
    fi
  SHELL

  config.vm.provision "shell", name: "verify_creathdds", privileged: true, inline: <<-SHELL
    pushd /vagrant
    mkdir -p factory/hdds/fixed && cd factory/hdds/fixed
    /vagrant/createhdds/createhdds.py -t -l info check
    exit 0
  SHELL

  config.vm.provision "shell", name: "run_createhdds", privileged: false, run: "always", inline: <<-SHELL
    echo -e "\nLogin to the createhdds box with the command..."
    echo -e "\n    vagrant ssh"
    echo -e "\nThen run createhdds with the command sequence..."
    echo -e "\n    cd /vagrant/factory/hdds/fixed"
    echo -e "\n    /vagrant/createhdds/createhdds.py -l info ks\n"
  SHELL

end
