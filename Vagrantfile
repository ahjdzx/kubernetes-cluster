# -*- mode: ruby -*-
# vi: set ft=ruby :

servers = [
    {
        :name => "k8s-master-1",
        :type => "master",
        :box => "ubuntu/xenial64",
        :box_version => "20190822.0.0",
        :eth1 => "192.168.205.10",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "k8s-worker-1",
        :type => "worker",
        :box => "ubuntu/xenial64",
        :box_version => "20190822.0.0",
        :eth1 => "192.168.205.11",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "k8s-worker-2",
        :type => "worker",
        :box => "ubuntu/xenial64",
        :box_version => "20190822.0.0",
        :eth1 => "192.168.205.12",
        :mem => "2048",
        :cpu => "2"
    }
]

Vagrant.configure("2") do |config|

    servers.each do |opts|
        config.vm.define opts[:name] do |config|

            config.vm.box = opts[:box]
            config.vm.box_version = opts[:box_version]
            config.vm.hostname = opts[:name]
            config.vm.network :private_network, ip: opts[:eth1]

            config.vm.provider "virtualbox" do |v|

                v.name = opts[:name]
                v.customize ["modifyvm", :id, "--groups", "/Ahjdzx Development"]
                v.customize ["modifyvm", :id, "--memory", opts[:mem]]
                v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]

            end

            if Vagrant.has_plugin?("vagrant-vbguest")
                config.vbguest.auto_update = false
            end

            if Vagrant.has_plugin?("vagrant-proxyconf")
                # 若安装了plugin，则设置代理信息
                config.proxy.http = "http://192.168.32.209:7777"
                config.proxy.https = "http://192.168.32.209:7777"
                config.proxy.no_proxy = "localhost,127.0.0.1,.example.com,.aliyun.com,192.168.205.10,172.16.0.0/16,10.0.2.15,10.96.0.0/12"
            else
                # 若没有安装plugin，则调用系统命令安装插件，并提示重运行命令
                system('vagrant plugin install vagrant-proxyconf')
                raise("vagrant-proxyconf installed. Run command again.");
            end

            # we cannot use this because we can't install the docker version we want - https://github.com/hashicorp/vagrant/issues/4871
            #config.vm.provision "docker"

            config.vm.provision "shell", path: "box.sh"

            if opts[:type] == "master"
                config.vm.provision "shell", path: "master.sh"
            else
                config.vm.provision "shell", path: "worker.sh"
            end

        end

    end

end
