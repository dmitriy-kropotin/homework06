# homework06
Для реализации домашнего задания буду использовать схему: repo и nginx на виртуальной машине+проброс портов через Vagrant(80=>8080, 443=>8443) +проброс портов через роутер(8080=>80, 8443=>443). Доменная запись на duckdns.org + zerossl

1. Буду использвать следующий Vagrantfile
```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.hostname = "homework06"
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :forwarded_port, guest: 443, host: 8443
  config.vm.synced_folder ".", "/vagrant", disabled: true
end
```
2. 
