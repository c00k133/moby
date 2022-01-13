# -*- mode: ruby -*-
# vi: set ft=ruby nospell :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.vm.define "box" do |box|
    box.vm.hostname = "box"

    # DISABLED. Enable root login.
    # Does not work out of the box, needs to be enabled after setting up box.
    # Unnecessary step, thus commented.
    # - https://stackoverflow.com/questions/25758737/vagrant-login-as-root-by-default
    #box.ssh.username = "root"
    #box.ssh.password = "vagrant"
    #box.ssh.insert_key = "true"

    # The folder this Vagrantfile resides in is mounted to `/vagrant` on the
    # guest by default.
    #box.vm.synced_folder ".", "/vagrant", disabled: true
    #box.vm.synced_folder ".", "/moby-src", disabled: false

    box.vm.provider "virtualbox" do |vb|
      vb.name = "box"
      vb.cpus = 1
      # A minimum of ~2GB required for Docker.
      vb.memory = 2048
    end

    # Might require a `vagrant reload` after creation.
    # `/vagrant` may not be mounted.
    # - https://www.vagrantup.com/docs/provisioning/docker
    box.vm.provision "docker"

    box.vm.provision "shell",
      inline: <<-'EOF'
        # Update and install required packages.
        apt-get --assume-yes update
        apt-get --assume-yes install make golang-go

        # DISABLED. Install Go.
        # The build script for Moby automatically finds $GO_PATH with AUTO_GOPATH=1
        # - https://www.digitalocean.com/community/tutorials/how-to-install-go-on-ubuntu-20-04
        # - https://go.dev/dl/
        #if ! grep --quiet GOPATH /home/vagrant/.profile &>/dev/null; then
        #  curl -OL https://golang.org/dl/go1.17.6.linux-amd64.tar.gz
        #  sudo tar --directory=/usr/local -xvf go1.17.6.linux-amd64.tar.gz

        #  #echo 'export PATH=$PATH:/usr/local/go/bin' >> /home/vagrant/.profile
        #  #echo 'export GOPATH=/home/vagrant/go' >> /home/vagrant/.profile
        #  #echo 'export PATH=$PATH:/usr/local/go/bin' >> /root/.profile
        #  #echo 'export GOPATH=/home/vagrant/go' >> /root/.profile
        #fi

        # Use the 'docker' provisioner above instead.
        # - https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script
        #if ! which docker &>/dev/null; then
        #  curl -fsSL https://get.docker.com -o get-docker.sh
        #  sudo sh get-docker.sh
        #fi
      EOF
  end
end
