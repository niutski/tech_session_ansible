Vagrant.configure("2") do |config| # "2" denotes the API version

    nodes = {
        'webserver' => { :ip  => '10.0.0.11', :memory => 1024, :cpus => 1, :role => "web" },
        'loadbalancer' => { :ip  => '10.0.0.12', :memory => 512, :cpus => 1, :role => "loadbalancer" }
    }

    nodes.each do |node_name, node_opts|
        config.vm.define node_name do |node_config|
            node_config.vm.hostname = "#{node_name}"
            node_config.vm.box = "centos-7-base"
            node_config.vm.box_url = "https://github.com/holms/vagrant-centos7-box/releases/download/7.1.1503.001/CentOS-7.1.1503-x86_64-netboot.box"
            node_config.vm.hostname = "#{node_config[:role]}"

            node_config.vm.network :private_network, ip: node_opts[:ip]            

            node_config.vm.provider "virtualbox" do |v|
              v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
              v.memory = node_opts[:memory]
              v.cpus = node_opts[:cpus]
            end
        end
    end
end
