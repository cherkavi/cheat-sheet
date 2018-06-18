# vagrant
* [download](https://releases.hashicorp.com/vagrant/)
* [reestr](https://www.vagrantup.com/docs/boxes.html)

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
