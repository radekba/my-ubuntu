# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "box-cutter/ubuntu1404-desktop"

  config.ssh.forward_agent = true
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "8192"
    vb.customize ["modifyvm", :id, "--cpus", "4"]
    vb.customize ["modifyvm", :id, "--monitorcount", "2"]
  end

  config.vm.synced_folder "../../git", "/home/vagrant/git", owner: "vagrant", group: "vagrant"

  config.vm.provision "copy-gitconfig", type: "file", source: "~/.gitconfig", destination: ".gitconfig"
  config.vm.provision "copy-gittoken", type: "file", source: "~/.gittoken", destination: ".gittoken"
  config.vm.provision "copy-wallpaper", type: "file", source: "wallpapers/wallpaper.jpg", destination: "tmp/wallpapers/wallpaper.jpg"

  $deployment = "deployment"
  $install = $deployment + "/install"
  $atom = $install + "/atom"
  $atom_packages = $atom + "/packages"
  $uninstall = $deployment + "/uninstall"
  $setup = $deployment + "/setup"
  $launcher = $setup + "/launcher-icons"

  config.vm.provision "uninstall-libre", type: "shell", path: $uninstall + "/uninstall-libre.sh", privileged: true
  config.vm.provision "uninstall-amazon", type: "shell", path: $uninstall + "/uninstall-amazon.sh", privileged: true

  config.vm.provision "install-ansible", type: "shell", path: $install + "/install-ansible.sh", privileged: true
  config.vm.provision "install-nodejs", type: "shell", path: $install + "/install-nodejs.sh", privileged: true
  config.vm.provision "install-docker", type: "shell", path: $install + "/install-docker.sh", privileged: true
  config.vm.provision "install-chrome", type: "shell", path: $install + "/install-chrome.sh", privileged: false
  config.vm.provision "install-atom", type: "shell", path: $atom + "/install-atom.sh", privileged: false
  config.vm.provision "install-atom-eclipse-keybindings", type: "shell", path: $atom_packages + "/install-atom-eclipse-keybindings.sh", privileged: false
  config.vm.provision "install-atom-highlight-selected", type: "shell", path: $atom_packages + "/install-atom-highlight-selected.sh", privileged: false
  config.vm.provision "install-git", type: "shell", path: $install + "/install-git.sh", privileged: true
  config.vm.provision "install-vim", type: "shell", path: $install + "/install-vim.sh", privileged: true
  config.vm.provision "install-xclip", type: "shell", path: $install + "/install-xclip.sh", privileged: true
  config.vm.provision "install-screen", type: "shell", path: $install + "/install-screen.sh", privileged: true
  config.vm.provision "install-nautilus", type: "shell", path: $install + "/install-nautilus.sh", privileged: true
  config.vm.provision "install-fonts", type: "shell", path: $install + "/install-fonts.sh", privileged: false

  config.vm.provision "setup-home", type: "shell", path: $setup + "/setup-home.sh", privileged: false
  config.vm.provision "setup-vim", type: "shell", path: $setup + "/setup-vim.sh", privileged: false
  config.vm.provision "setup-wallpaper", type: "shell", path: $setup + "/setup-wallpaper.sh", privileged: true

  # This does not work, it uses gsetting which does not work over ssh, maybe ansible will be able to do it?
  config.vm.provision "add-chrome-icon", type: "shell", path: $launcher + "/add-chrome-icon.sh", privileged: false
  config.vm.provision "add-atom-icon", type: "shell", path: $launcher + "/add-chrome-icon.sh", privileged: false
  config.vm.provision "remove-firefox-icon", type: "shell", path: $launcher + "/remove-firefox-icon.sh", privileged: false

  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"
end
