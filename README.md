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
2. Запускаю вирутальную машину через vagrant, подключаюсь через ssh и захожу под root
```
[tesla@ol85 homework06]$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
...
[tesla@ol85 homework06]$ vagrant ssh
[vagrant@homework06 ~]$ sudo -i
[root@homework06 ~]#
```
3. Устанавливаю на операционную систему обновления и перезагружаюсь
```
[root@homework06 ~]# yum update
...
Transaction Summary
================================================================================
Install    1 Package  (+1 Dependent package)
Upgrade  134 Packages

Total download size: 254 M
...
  yum-utils.noarch 0:1.1.31-54.el7_8
  zlib.x86_64 0:1.2.7-19.el7_9

Complete!
[root@homework06 ~]# reboot
Connection to 127.0.0.1 closed by remote host.
Connection to 127.0.0.1 closed.
[tesla@ol85 homework06]$ vagrant ssh
Last login: Sun Feb 27 09:07:58 2022 from 10.0.2.2
[vagrant@homework06 ~]$ sudo -i
[root@homework06 ~]#
```
4. Устанавливаю необходимые для домашней работы пакеты
```
[root@homework06 ~]# yum install -y redhat-lsb-core wget rpmdevtools rpm-build createrepo yum-utils
Загружены модули: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.truenetwork.ru
 * extras: mirror.truenetwork.ru
 * updates: mirror.yandex.ru
Пакет yum-utils-1.1.31-54.el7_8.noarch уже установлен, и это последняя версия.
...
Итого за операцию
================================================================================
Установить  5 пакетов (+48 зависимых)

Объем загрузки: 17 M
Объем изменений: 51 M
...
  zip.x86_64 0:3.0-11.el7

Выполнено!
```
5. Скачиваю пакет с иходными кодами NGINX 
```
[root@homework06 ~]# wget https://nginx.org/packages/centos/7/SRPMS/nginx-1.20.2-1.el7.ngx.src.rpm
--2022-02-27 09:22:40--  https://nginx.org/packages/centos/7/SRPMS/nginx-1.20.2-1.el7.ngx.src.rpm
Распознаётся nginx.org (nginx.org)... 52.58.199.22, 3.125.197.172, 2a05:d014:edb:5704::6, ...
Подключение к nginx.org (nginx.org)|52.58.199.22|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа... 200 OK
Длина: 1082461 (1,0M) [application/x-redhat-package-manager]
Сохранение в: «nginx-1.20.2-1.el7.ngx.src.rpm»

100%[============================================================================================>] 1 082 461   3,85MB/s   за 0,3s

2022-02-27 09:22:41 (3,85 MB/s) - «nginx-1.20.2-1.el7.ngx.src.rpm» сохранён [1082461/1082461]

[root@homework06 ~]# ls -la
итого 1100
dr-xr-x---.  2 root root     196 фев 27 09:22 .
dr-xr-xr-x. 17 root root     240 апр 30  2020 ..
-rw-------.  1 root root    5570 апр 30  2020 anaconda-ks.cfg
-rw-------.  1 root root      18 фев 27 09:15 .bash_history
-rw-r--r--.  1 root root      18 дек 29  2013 .bash_logout
-rw-r--r--.  1 root root     176 дек 29  2013 .bash_profile
-rw-r--r--.  1 root root     176 дек 29  2013 .bashrc
-rw-r--r--.  1 root root     100 дек 29  2013 .cshrc
-rw-r--r--.  1 root root 1082461 ноя 16 15:20 nginx-1.20.2-1.el7.ngx.src.rpm
-rw-------.  1 root root    5300 апр 30  2020 original-ks.cfg
-rw-r--r--.  1 root root     129 дек 29  2013 .tcshrc
[root@homework06 ~]#

```
6. Устанавлию пакет и проверяю, что в домашней директории создался каталог для сборки
```
[root@homework06 ~]# rpm -i nginx-1.20.2-1.el7.ngx.src.rpm
предупреждение: nginx-1.20.2-1.el7.ngx.src.rpm: Заголовок V4 RSA/SHA1 Signature, key ID 7bd9bf62: NOKEY
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
предупреждение: пользователь builder не существует - используется root
предупреждение: группа builder не существует - используется root
[root@homework06 ~]# ls -la
итого 1100
dr-xr-x---.  3 root root     212 фев 27 09:25 .
dr-xr-xr-x. 17 root root     240 апр 30  2020 ..
-rw-------.  1 root root    5570 апр 30  2020 anaconda-ks.cfg
-rw-------.  1 root root      18 фев 27 09:15 .bash_history
-rw-r--r--.  1 root root      18 дек 29  2013 .bash_logout
-rw-r--r--.  1 root root     176 дек 29  2013 .bash_profile
-rw-r--r--.  1 root root     176 дек 29  2013 .bashrc
-rw-r--r--.  1 root root     100 дек 29  2013 .cshrc
-rw-r--r--.  1 root root 1082461 ноя 16 15:20 nginx-1.20.2-1.el7.ngx.src.rpm
-rw-------.  1 root root    5300 апр 30  2020 original-ks.cfg
drwxr-xr-x.  4 root root      34 фев 27 09:25 rpmbuild
-rw-r--r--.  1 root root     129 дек 29  2013 .tcshrc
```
7. Скачиваю и распаковываю openssl 1.1.1m
```
[root@homework06 ~]# wget https://www.openssl.org/source/openssl-1.1.1m.tar.gz
--2022-02-27 09:50:52--  https://www.openssl.org/source/openssl-1.1.1m.tar.gz
Распознаётся www.openssl.org (www.openssl.org)... 184.51.226.32, 2001:2030:21:1a3::c1e, 2001:2030:21:193::c1e
Подключение к www.openssl.org (www.openssl.org)|184.51.226.32|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа... 200 OK
Длина: 9847315 (9,4M) [application/x-gzip]
Сохранение в: «openssl-1.1.1m.tar.gz»

100%[============================================================================================>] 9 847 315   8,52MB/s   за 1,1s

2022-02-27 09:50:54 (8,52 MB/s) - «openssl-1.1.1m.tar.gz» сохранён [9847315/9847315]

[root@homework06 ~]# tar -xvf openssl-1.1.1m.tar.gz
....
openssl-1.1.1m/util/private.num
openssl-1.1.1m/util/process_docs.pl
openssl-1.1.1m/util/shlib_wrap.sh.in
openssl-1.1.1m/util/su-filter.pl
openssl-1.1.1m/util/unlocal_shlib.com.in
[root@homework06 ~]# ls -la
итого 10724
dr-xr-x---.  4 root root     263 фев 27 09:49 .
dr-xr-xr-x. 17 root root     240 апр 30  2020 ..
-rw-------.  1 root root    5570 апр 30  2020 anaconda-ks.cfg
-rw-------.  1 root root      18 фев 27 09:15 .bash_history
-rw-r--r--.  1 root root      18 дек 29  2013 .bash_logout
-rw-r--r--.  1 root root     176 дек 29  2013 .bash_profile
-rw-r--r--.  1 root root     176 дек 29  2013 .bashrc
-rw-r--r--.  1 root root     100 дек 29  2013 .cshrc
-rw-r--r--.  1 root root 1082461 ноя 16 15:20 nginx-1.20.2-1.el7.ngx.src.rpm
drwxrwxr-x. 18 root root    4096 дек 14 15:45 openssl-1.1.1m
-rw-r--r--.  1 root root 9847315 дек 14 16:28 openssl-1.1.1m.tar.gz
-rw-------.  1 root root    5300 апр 30  2020 original-ks.cfg
drwxr-xr-x.  4 root root      34 фев 27 09:25 rpmbuild
-rw-r--r--.  1 root root     129 дек 29  2013 .tcshrc
```
8. Проверяю зависимости для сборки
```
[root@homework06 ~]# yum-builddep rpmbuild/SPECS/nginx.spec
....

Зависимости определены

======================================================================================================================================
 Package                               Архитектура              Версия                                Репозиторий               Размер
======================================================================================================================================
Установка:
 openssl-devel                         x86_64                   1:1.0.2k-24.el7_9                     updates                   1.5 M
 pcre-devel                            x86_64                   8.32-17.el7                           base                      480 k
 zlib-devel                            x86_64                   1.2.7-19.el7_9                        updates                    50 k
Установка зависимостей:
 keyutils-libs-devel                   x86_64                   1.5.8-3.el7                           base                       37 k
 krb5-devel                            x86_64                   1.15.1-51.el7_9                       updates                   273 k
 libcom_err-devel                      x86_64                   1.42.9-19.el7                         base                       32 k
 libkadm5                              x86_64                   1.15.1-51.el7_9                       updates                   179 k
 libselinux-devel                      x86_64                   2.5-15.el7                            base                      187 k
 libsepol-devel                        x86_64                   2.5-10.el7                            base                       77 k
 libverto-devel                        x86_64                   0.2.5-4.el7                           base                       12 k

Итого за операцию
======================================================================================================================================
Установить  3 пакета (+7 зависимых)
....


Установлено:
  openssl-devel.x86_64 1:1.0.2k-24.el7_9          pcre-devel.x86_64 0:8.32-17.el7          zlib-devel.x86_64 0:1.2.7-19.el7_9

Установлены зависимости:
  keyutils-libs-devel.x86_64 0:1.5.8-3.el7      krb5-devel.x86_64 0:1.15.1-51.el7_9       libcom_err-devel.x86_64 0:1.42.9-19.el7
  libkadm5.x86_64 0:1.15.1-51.el7_9             libselinux-devel.x86_64 0:2.5-15.el7      libsepol-devel.x86_64 0:2.5-10.el7
  libverto-devel.x86_64 0:0.2.5-4.el7

Выполнено!
```
9. В зависимостях есть openssl `openssl-devel.x86_64 1:1.0.2k-24.el7_9 `, но версия постарее.
10. 
