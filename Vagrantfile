hosts = '''
10.10.10.100 prometheus.example.com
10.10.10.100 alertmanager.example.com
10.10.10.101 grafana.example.com
'''

Vagrant.configure('2') do |config|
  config.vm.provider "libvirt" do |lv, config|
    lv.memory = 2048
    lv.cpus = 2
    lv.cpu_mode = "host-passthrough"
    lv.keymap = "pt"
    # replace the default synced_folder with something that works in the base box.
    # NB for some reason, this does not work when placed in the base box Vagrantfile.
    config.vm.synced_folder '.', '/vagrant', type: 'rsync', rsync__exclude: [
      '.vagrant/',
      '.git/',
      '*.box']
  end

  config.vm.provider :virtualbox do |v, override|
    v.linked_clone = true
    v.cpus = 2
    v.memory = 2048
    v.customize ['modifyvm', :id, '--vram', 64]
    v.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
  end

  config.vm.define :prometheus do |config|
    config.vm.box = 'windows-2016-amd64'
    config.vm.hostname = 'prometheus'
    config.vm.network :private_network, ip: '10.10.10.100'
    config.vm.network :private_network, ip: '10.10.10.101'
    config.vm.provision :shell, inline: "echo '#{hosts}' | Out-File -Encoding Ascii -Append c:/Windows/System32/drivers/etc/hosts"
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-common.ps1'
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-certificates.ps1'
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-caddy.ps1'
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-mailhog.ps1'
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-alertmanager.ps1'
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-wmi-exporter.ps1'
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-blackbox-exporter.ps1'
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-prometheus.ps1'
    config.vm.provision :shell, path: 'ps.ps1', args: 'provision-grafana.ps1'
  end
end
