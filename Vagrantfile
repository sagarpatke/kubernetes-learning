# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "box-cutter/centos71"

  config.vm.define :master do |master|
    master.vm.network :private_network, ip: '192.168.20.10'

    master.vm.provision :ansible do |ansible|
      ansible.host_vars = {
        'master' => {
          'etcd_bind_address' => '192.168.20.10',
          'etcd_host' => '192.168.20.10',
          'etcd_port' => '2379',
          'flanneld_configure_network' => 'yes',
          'flanneld_network_range' => '172.72.0.0/16',
          'flanneld_key' => '/atomic.io/network',
          'kubernetes_master' => 'yes',
          'kubernetes_minion' => 'no',
          'kube_apiserver_bind_address' => '0.0.0.0',
          'kube_apiserver_host' => '192.168.20.10',
          'kube_apiserver_port' => '8080',
          'kube_apiserver_service_ip_range' => '10.254.0.0/16'
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
          'etcd_host' => '192.168.20.10',
          'etcd_port' => '2379',
          'flanneld_configure_network' => 'no',
          'flanneld_key' => '/atomic.io/network',
          'kubernetes_master' => 'no',
          'kubernetes_minion' => 'yes',
          'kube_apiserver_host' => '192.168.20.10',
          'kubelet_bind_address' => '192.168.20.11',
          'kubelet_host' => '192.168.20.11',
          'kubelet_port' => '10250'
        }
      }
      ansible.playbook = 'minion.yml'
    end
  end
end
