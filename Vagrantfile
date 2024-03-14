Vagrant.configure("2") do |config|
  # Dynamically set the machine name to the name of the current directory
  current_dir_name = File.basename(Dir.getwd)
  config.vm.define current_dir_name do |docker_vm|
    docker_vm.vm.hostname = "ubuntu"

    docker_vm.vm.provider :docker do |docker, override|
      override.vm.box = nil
      docker.image = "rofrano/vagrant-provider:ubuntu"
      docker.remains_running = true
      docker.has_ssh = true
      docker.privileged = true
      docker.volumes = ["/sys/fs/cgroup:/sys/fs/cgroup:rw"]
      docker.create_args = ["--cgroupns=host"]
    end

    docker_vm.vm.provision "shell", inline: <<-SHELL
      # Add a provisioner to change to a specific subdirectory of /vagrant upon login
      # The subdirectory matches the name of the current directory on the host machine
      echo 'cd /vagrant/#{current_dir_name}' >> /home/vagrant/.bashrc

      # update the packages
      sudo apt-get update
      # cd /vagrant/#{current_dir_name} && [ -x script/bootstrap ] && ./script/bootstrap
    SHELL
  end
end
