# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "box-cutter/centos71"

  config.vm.define :master do |master|
    master.vm.network :private_network, ip: '192.168.20.10'
    master.vm.provision :ansible do |ansible|
      ansible.host_vars = {
        'master' => {
          'etcd_bind_host' => '0.0.0.0',
          'etcd_bind_port' => '2379',
          'kube_api_bind_address' => '0.0.0.0',
          'kube_api_bind_port' => '8080'
        }
      }
      ansible.playbook = 'master.yml'
    end
  end

  config.vm.define :minion1 do |minion|
    minion.vm.network :private_network, ip: '192.168.20.11'
    minion.vm.provision :ansible do |ansible|
      ansible.host_vars = {
        'minion1' => {
          'flannel_etcd' => 'http://192.168.20.10:2379',
          'kube_master_host' => '192.168.20.10',
          'kube_master_port' => '8080',
          'kubelet_hostname' => '192.168.20.11'
        }
      }
      ansible.playbook = 'minion.yml'
    end
  end
end
