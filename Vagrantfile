Vagrant.configure("2") do |config| # "2" denotes the API version

    nodes = {
        'database' => { :ip  => '192.168.111.223', :memory => 4096, :cpus => 2 },
        'webserver' => { :ip  => '192.168.111.222', :memory => 4096, :cpus => 2, :http_forward_port => 8000 }
    }

    nodes.each do |node_name, node_opts|
        config.vm.define node_name do |config|
            config.vm.hostname = "#{node_name}"
            config.vm.box = "centos-7-base"
            config.vm.box_url = "https://github.com/holms/vagrant-centos7-box/releases/download/7.1.1503.001/CentOS-7.1.1503-x86_64-netboot.box"

            config.vm.network :private_network, ip: node_opts[:ip]
            if !node_opts[:http_forward_port].nil?
              config.vm.network :forwarded_port, host: node_opts[:http_forward_port], guest: 80
            end


            config.vm.provider "virtualbox" do |v|
              v.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
              v.memory = node_opts[:memory]
              v.cpus = node_opts[:cpus]
            end

            config.vm.provision "shell", inline: "yum install -y python"
            config.vm.provision "ansible" do |ansible|
                ansible.playbook = "ansible/local.yml"
                ansible.sudo = true
                ansible.inventory_path = "ansible/inventory"
            end
        end
    end
end