# vagrant
* [download](https://releases.hashicorp.com/vagrant/)
* [reestr](https://app.vagrantup.com/boxes/search)

## init project
```
vagrant init
```

## check "Vagrantfile"
```
vagrant validate 
```

## start
```
vagrant up
```

## stop machine for a while, save state
```
vagrant suspend
```

## re-start machine after stop
```
vagrant resume
```

## current machine status
```
vagrant status
```


## 'switch off power'
```
vagrant halt
```

## destroy - remove cache from .vagrant.d
```
vagrant destroy
```

## connect to machine 
```
vagrant ssh
```

## default shared folder
```
/vagrant
```


## plugins
```
vagrant plugin install {plugin name}
```

### porxy plugin - vagrant-proxyconf
```
config.proxy.http     = "10.32.2.11:3128"
config.proxy.https    = "10.32.2.11:3128"
config.proxy.no_proxy = "localhost,127.0.0.1"
```

## cookbook

### install ansible
```
  config.vm.provision "shell", inline: <<-SHELL
	sudo apt update
	sudo apt install -y python
	curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
	sudo python get-pip.py
	sudo pip install ansible
	rm get-pip.py
  SHELL
```

### start ansible 
```
config.vm.provision "ansible_local" do |ansible|
  ansible.playbook = "playbook.yml"
  ansible.install_mode = "pip"
  ansible.version = "2.2.1.0"
end
```

### ip address inside network, connection from one box to another, [vm network](https://www.barrykooij.com/connect-mysql-vagrant-machine/)
part of the Vagrantfile
```
  config.vm.network "private_network", type: "dhcp"
```
command inside box
```
ifconfig
```

### configuration of proxy with condition
```
  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = "http://10.32.142.11:3128/"
    config.proxy.https    = "http://10.32.142.11:3128/"
    config.proxy.no_proxy = "localhost,127.0.0.1"
  end
```


## issues

### network adapter
```
A host only network interface you're attempting to configure via DHCP
already has a conflicting host only adapter with DHCP enabled. The
DHCP on this adapter is incompatible with the DHCP settings. Two
host only network interfaces are not allowed to overlap, and each
host only network interface can have only one DHCP server. Please
reconfigure your host only network or remove the virtual machine
using the other host only network.
```
resolution
```
from visual interface of VirtualBox "Global Tools"->"Host Network Manager" remove second virtual network 
```
