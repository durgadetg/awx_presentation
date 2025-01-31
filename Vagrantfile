Vagrant.configure("2") do |config|
  servers=[
    {
      :hostname => "centos1.lab.com",
      :box => "centos/7",
      :ip => "192.168.56.105",
      :ssh_port => '2228'
    },
    {
      :hostname => "centos2.lab.com",
      :box => "centos/7",
      :ip => "192.168.56.106",
      :ssh_port => '2229'
    },
    {
      :hostname => "ubuntu1.lab.com",
      :box => "bento/ubuntu-18.04",
      :ip => "192.168.56.107",
      :ssh_port => '2230'
    },
    {
      :hostname => "ubuntu2.lab.com",
      :box => "bento/ubuntu-18.04",
      :ip => "192.168.56.108",
      :ssh_port => '2231'
    },

  ]

  servers.each do |machine|

    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
      node.vm.synced_folder '.', '/vagrant', disabled: true
    
      node.vm.network :private_network, ip: machine[:ip]
      node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"

      node.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--memory", 512]
        v.customize ["modifyvm", :id, "--cpus", 1]
        v.customize ["modifyvm", :id, "--name", machine[:hostname]]
      end
    end
  end

  id_rsa_key_pub = File.read(File.join(Dir.home, ".ssh", "id_rsa.pub"))

  config.vm.provision :shell,
        :inline => "echo 'appending SSH public key to ~vagrant/.ssh/authorized_keys' && echo '#{id_rsa_key_pub }' >> /home/vagrant/.ssh/authorized_keys && chmod 600 /home/vagrant/.ssh/authorized_keys"

  config.ssh.insert_key = false
  config.vbguest.auto_update = false
end
