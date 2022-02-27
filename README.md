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
10. Редактирую nginx.spec `[root@homework06 ~]# vi rpmbuild/SPECS/nginx.spec`
```
%build
./configure %{BASE_CONFIGURE_ARGS} \
    --with-cc-opt="%{WITH_CC_OPT}" \
    --with-ld-opt="%{WITH_LD_OPT}" \
    --with-openssl=/root/openssl-1.1.1m\
    --with-debug
```
11. Собираю пакет
```
[root@homework06 ~]# rpmbuild -bb rpmbuild/SPECS/nginx.spec
```
Получил ошибку
```
checking for OS
 + Linux 3.10.0-1160.59.1.el7.x86_64 x86_64
checking for C compiler ... not found

./configure: error: C compiler cc is not found

ошибка: Неверный код возврата из /var/tmp/rpm-tmp.vl8cYD (%build)


Ошибки сборки пакетов:
    Неверный код возврата из /var/tmp/rpm-tmp.vl8cYD (%build)
```
12. Доставляю gcc
```
[root@homework06 ~]# yum install gcc
...
Установлено:
  gcc.x86_64 0:4.8.5-44.el7

Установлены зависимости:
  cpp.x86_64 0:4.8.5-44.el7                        glibc-devel.x86_64 0:2.17-325.el7_9     glibc-headers.x86_64 0:2.17-325.el7_9
  kernel-headers.x86_64 0:3.10.0-1160.59.1.el7     libmpc.x86_64 0:1.0.1-3.el7             mpfr.x86_64 0:3.1.1-4.el7

Выполнено!
```
13. Запускаю еще раз сборку пакета
```
[root@homework06 ~]# rpmbuild -bb rpmbuild/SPECS/nginx.spec
...
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd nginx-1.20.2
+ /usr/bin/rm -rf /root/rpmbuild/BUILDROOT/nginx-1.20.2-1.el7.ngx.x86_64
+ exit 0
```
На этот раз успешно!
14. Проверяю, что собранные пакеты на месте
```
[root@homework06 ~]# ls -la rpmbuild/RPMS/x86_64/
итого 4104
drwxr-xr-x. 2 root root      98 фев 27 17:17 .
drwxr-xr-x. 3 root root      20 фев 27 17:17 ..
-rw-r--r--. 1 root root 2199212 фев 27 17:17 nginx-1.20.2-1.el7.ngx.x86_64.rpm
-rw-r--r--. 1 root root 2000284 фев 27 17:17 nginx-debuginfo-1.20.2-1.el7.ngx.x86_64.rpm
[root@homework06 ~]#
```
15. Устанавливаю nginx из собранного пакета
```
[root@homework06 ~]# yum localinstall -y rpmbuild/RPMS/x86_64/nginx-1.20.2-1.el7.ngx.x86_64.rpm
Загружены модули: fastestmirror
Проверка rpmbuild/RPMS/x86_64/nginx-1.20.2-1.el7.ngx.x86_64.rpm: 1:nginx-1.20.2-1.el7.ngx.x86_64
rpmbuild/RPMS/x86_64/nginx-1.20.2-1.el7.ngx.x86_64.rpm отмечен для установки
Разрешение зависимостей
--> Проверка сценария
---> Пакет nginx.x86_64 1:1.20.2-1.el7.ngx помечен для установки
--> Проверка зависимостей окончена

Зависимости определены
....
Commercial subscriptions for nginx are available on:
* https://nginx.com/products/

----------------------------------------------------------------------
  Проверка    : 1:nginx-1.20.2-1.el7.ngx.x86_64                                                                                   1/1

Установлено:
  nginx.x86_64 1:1.20.2-1.el7.ngx

Выполнено!
```
16. Запускаю nginx и проверяю его статус
```
[root@homework06 ~]# systemctl start nginx
[root@homework06 ~]# systemctl enable nginx
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
[root@homework06 ~]# systemctl status nginx
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
   Active: active (running) since Вс 2022-02-27 17:22:04 UTC; 23s ago
     Docs: http://nginx.org/en/docs/
 Main PID: 8274 (nginx)
   CGroup: /system.slice/nginx.service
           ├─8274 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
           └─8275 nginx: worker process

фев 27 17:22:04 homework06 systemd[1]: Starting nginx - high performance web server...
фев 27 17:22:04 homework06 systemd[1]: Can't open PID file /var/run/nginx.pid (yet?) after start: No such file or directory
фев 27 17:22:04 homework06 systemd[1]: Started nginx - high performance web server.
```
17. Проверяю при помощи curl работу nginx
```
[root@homework06 ~]# curl http://localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
18. Создаю директорию под репозиторий и копирую в нее свой собранный nginx
```
[root@homework06 ~]# mkdir /usr/share/nginx/html/repo
[root@homework06 ~]# cp rpmbuild/RPMS/x86_64/nginx-1.20.2-1.el7.ngx.x86_64.rpm /usr/share/nginx/html/repo
[root@homework06 ~]# ls -la /usr/share/nginx/html/repo
итого 2148
drwxr-xr-x. 2 root root      47 фев 27 17:26 .
drwxr-xr-x. 3 root root      52 фев 27 17:26 ..
-rw-r--r--. 1 root root 2199212 фев 27 17:26 nginx-1.20.2-1.el7.ngx.x86_64.rpm
[root@homework06 ~]#
```
19. Скачаю в свой репозиторий mc с зависимостями и percona-release
```
[root@homework06 ~]# yum install --downloadonly --downloaddir=/usr/share/nginx/html/repo/ mc
...
Итого за операцию
======================================================================================================================================
Установить  1 пакет (+1 зависимый)

Объем загрузки: 1.8 M
Объем изменений: 5.7 M
Background downloading packages, then exiting:
(1/2): gpm-libs-1.20.7-6.el7.x86_64.rpm                                                                        |  32 kB  00:00:00
(2/2): mc-4.8.7-11.el7.x86_64.rpm                                                                              | 1.7 MB  00:00:00
--------------------------------------------------------------------------------------------------------------------------------------
Общий размер                                                                                          2.7 MB/s | 1.8 MB  00:00:00
exiting because "Download Only" specified
[root@homework06 ~]# wget https://downloads.percona.com/downloads/percona-release/percona-release-1.0-9/redhat/percona-release-1.0-9.noarch.rpm
--2022-02-27 17:30:57--  https://downloads.percona.com/downloads/percona-release/percona-release-1.0-9/redhat/percona-release-1.0-9.noarch.rpm
Распознаётся downloads.percona.com (downloads.percona.com)... 162.220.4.222, 162.220.4.221, 74.121.199.231
Подключение к downloads.percona.com (downloads.percona.com)|162.220.4.222|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа... 200 OK
Длина: 16664 (16K) [application/octet-stream]
Сохранение в: «percona-release-1.0-9.noarch.rpm»

100%[============================================================================================>] 16 664      --.-K/s   за 0,1s

2022-02-27 17:30:58 (125 KB/s) - «percona-release-1.0-9.noarch.rpm» сохранён [16664/16664]
[root@homework06 ~]# cp percona-release-1.0-9.noarch.rpm /usr/share/nginx/html/repo/
[root@homework06 ~]# ls -la /usr/share/nginx/html/repo/
итого 3980
drwxr-xr-x. 2 root root     161 фев 27 17:31 .
drwxr-xr-x. 3 root root      52 фев 27 17:26 ..
-rw-r--r--. 1 root root   33120 авг 22  2019 gpm-libs-1.20.7-6.el7.x86_64.rpm
-rw-r--r--. 1 root root 1815580 ноя 20  2016 mc-4.8.7-11.el7.x86_64.rpm
-rw-r--r--. 1 root root 2199212 фев 27 17:26 nginx-1.20.2-1.el7.ngx.x86_64.rpm
-rw-r--r--. 1 root root   16664 фев 27 17:31 percona-release-1.0-9.noarch.rpm
```
Для меня инетересно то, что yum скачал mc вместе с зависимостями.
20. Создаю репозиторий
```
[root@homework06 ~]# createrepo /usr/share/nginx/html/repo
Spawning worker 0 with 4 pkgs
Workers Finished
Saving Primary metadata
Saving file lists metadata
Saving other metadata
Generating sqlite DBs
Sqlite DBs complete
```
21. Включаю autoindex в настройках nginx, проверяю конфигурацию nginx и перезагружаю ее
```
[root@homework06 ~]# vi /etc/nginx/conf.d/default.conf

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        autoindex on;
    }

[root@homework06 ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[root@homework06 ~]# nginx -s reload
```
22. Проверяю
```
[root@homework06 ~]# curl -a http://localhost/repo/
<html>
<head><title>Index of /repo/</title></head>
<body>
<h1>Index of /repo/</h1><hr><pre><a href="../">../</a>
<a href="repodata/">repodata/</a>                                          27-Feb-2022 17:33                   -
<a href="gpm-libs-1.20.7-6.el7.x86_64.rpm">gpm-libs-1.20.7-6.el7.x86_64.rpm</a>                   22-Aug-2019 21:25               33120
<a href="mc-4.8.7-11.el7.x86_64.rpm">mc-4.8.7-11.el7.x86_64.rpm</a>                         20-Nov-2016 19:25             1815580
<a href="nginx-1.20.2-1.el7.ngx.x86_64.rpm">nginx-1.20.2-1.el7.ngx.x86_64.rpm</a>                  27-Feb-2022 17:26             2199212
<a href="percona-release-1.0-9.noarch.rpm">percona-release-1.0-9.noarch.rpm</a>                   27-Feb-2022 17:31               16664
</pre><hr></body>
</html>
```
23. Добавляю свой репозиторий
```
[root@homework06 ~]# yum-config-manager --add-repo http://localhost/repo/
Загружены модули: fastestmirror
adding repo from: http://localhost/repo/

[localhost_repo_]
name=added from: http://localhost/repo/
baseurl=http://localhost/repo/
enabled=1
[root@homework06 ~]# yum repolist enabled
Загружены модули: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.truenetwork.ru
 * extras: mirror.truenetwork.ru
 * updates: mirror.yandex.ru
localhost_repo_                                                                                                | 2.9 kB  00:00:00
localhost_repo_/primary_db                                                                                     | 5.1 kB  00:00:00
Идентификатор репозитория                                 репозиторий                                                        состояние
base/7/x86_64                                             CentOS-7 - Base                                                    10 072
extras/7/x86_64                                           CentOS-7 - Extras                                                     500
localhost_repo_                                           added from: http://localhost/repo/                                      4
updates/7/x86_64                                          CentOS-7 - Updates                                                  3 567
repolist: 14 143
```
Репозиторий назван `localhost_repo_`

24. Далее я отключаю все репозитории, и включаю только свой, локальный
```
[root@homework06 ~]# yum-config-manager --disable "*"
.....
[root@homework06 ~]# yum-config-manager --enable "localhost_repo_"
Загружены модули: fastestmirror
======================================================= repo: localhost_repo_ ========================================================
[localhost_repo_]
....
```
25. Доступные пакеты
```
[root@homework06 ~]# yum list | grep localhost
gpm-libs.x86_64                    1.20.7-6.el7                 localhost_repo_
mc.x86_64                          1:4.8.7-11.el7               localhost_repo_
percona-release.noarch             1.0-9                        localhost_repo_
```
26. Устанавливаю mc и percona-release
```
[root@homework06 ~]# yum install mc
Загружены модули: fastestmirror
Loading mirror speeds from cached hostfile
localhost_repo_
...
ависимости определены

======================================================================================================================================
 Package                      Архитектура                Версия                             Репозиторий                         Размер
======================================================================================================================================
Установка:
 mc                           x86_64                     1:4.8.7-11.el7                     localhost_repo_                     1.7 M
Установка зависимостей:
 gpm-libs                     x86_64                     1.20.7-6.el7                       localhost_repo_                      32 k

Итого за операцию
======================================================================================================================================
Установить  1 пакет (+1 зависимый)
...
Установлено:
  mc.x86_64 1:4.8.7-11.el7

Установлены зависимости:
  gpm-libs.x86_64 0:1.20.7-6.el7

Выполнено!

[root@homework06 ~]# yum install percona-release
Загружены модули: fastestmirror
Loading mirror speeds from cached hostfile
Разрешение зависимостей
--> Проверка сценария
---> Пакет percona-release.noarch 0:1.0-9 помечен для установки
--> Проверка зависимостей окончена

Зависимости определены

======================================================================================================================================
 Package                             Архитектура                Версия                      Репозиторий                         Размер
======================================================================================================================================
Установка:
 percona-release                     noarch                     1.0-9                       localhost_repo_                      16 k

Итого за операцию
======================================================================================================================================
Установить  1 пакет
...
Объем изменений: 18 k
Is this ok [y/d/N]: y
Downloading packages:
предупреждение: /var/cache/yum/x86_64/7/localhost_repo_/packages/percona-release-1.0-9.noarch.rpm: Заголовок V4 DSA/SHA1 Signature, key ID cd2efd2a: NOKEY
Публичный ключ для percona-release-1.0-9.noarch.rpm не установлен
percona-release-1.0-9.noarch.rpm                                                                               |  16 kB  00:00:00


Публичный ключ для percona-release-1.0-9.noarch.rpm не установлен
```
mc установился вместе с зависимостями, а percona-release нет. Необходимо добавить настройку gpgcheck=0 в параметрах репозитория
```
[localhost_repo_]
name=added from: http://localhost/repo/
baseurl=http://localhost/repo/
enabled=1
gpgcheck=0
```
27. Пробую еще раз уставноить percona-release
```
[root@homework06 ~]# yum install percona-release
Загружены модули: fastestmirror
Loading mirror speeds from cached hostfile
localhost_repo_                                                                                                | 2.9 kB  00:00:00
Разрешение зависимостей
--> Проверка сценария
---> Пакет percona-release.noarch 0:1.0-9 помечен для установки
--> Проверка зависимостей окончена

Зависимости определены

======================================================================================================================================
 Package                             Архитектура                Версия                      Репозиторий                         Размер
======================================================================================================================================
Установка:
 percona-release                     noarch                     1.0-9                       localhost_repo_                      16 k
...

  Проверка    : percona-release-1.0-9.noarch                                                                                      1/1

Установлено:
  percona-release.noarch 0:1.0-9

Выполнено!
```
Успех!

28. Далее получаю через личный кабинет zerossl сертификат. Копирую его через scp на виртуальную машину. Конфигурирую nginx соответствующим образом
```
[vagrant@homework06 ~]$ scp tesla@10.0.2.2:~/homework06/certificate01.crt ./
The authenticity of host '10.0.2.2 (10.0.2.2)' can't be established.
ECDSA key fingerprint is SHA256:etBgzo6fwPO4pFwhgCWfS9reMJ+R3l4ioSSRK/VOXGk.
ECDSA key fingerprint is MD5:d6:03:06:98:fa:68:c6:1e:23:6f:15:c8:dc:19:75:61.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.0.2.2' (ECDSA) to the list of known hosts.
tesla@10.0.2.2's password:
certificate01.crt                                                                                                      100% 4765     3.4MB/s   00:00
[vagrant@homework06 ~]$ ls -la
итого 24
drwx------. 3 vagrant vagrant  120 фев 27 18:12 .
drwxr-xr-x. 3 root    root      21 апр 30  2020 ..
-rw-------. 1 vagrant vagrant   15 фев 27 18:08 .bash_history
-rw-r--r--. 1 vagrant vagrant   18 апр  1  2020 .bash_logout
-rw-r--r--. 1 vagrant vagrant  193 апр  1  2020 .bash_profile
-rw-r--r--. 1 vagrant vagrant  231 апр  1  2020 .bashrc
-rw-rw-r--. 1 vagrant vagrant 4765 фев 27 18:12 certificate01.crt
drwx------. 2 vagrant vagrant   48 фев 27 18:12 .ssh
[vagrant@homework06 ~]$ scp tesla@10.0.2.2:~/homework06/private.key ./
tesla@10.0.2.2's password:
private.key                                                                                                            100% 1702     1.3MB/s   00:00
[vagrant@homework06 ~]$ pwd
/home/vagrant
[vagrant@homework06 ~]$ sudo cp /home/vagrant/private.key /etc/ssl/
[vagrant@homework06 ~]$ sudo cp /home/vagrant/certificate01.crt /etc/ssl/
[vagrant@homework06 ~]$ ls -la /etc/ssl
итого 24
drwxr-xr-x.  2 root root   63 фев 27 18:14 .
drwxr-xr-x. 84 root root 8192 фев 27 17:58 ..
-rw-r--r--.  1 root root 4765 фев 27 18:14 certificate01.crt
lrwxrwxrwx.  1 root root   16 фев 27 09:11 certs -> ../pki/tls/certs
-rw-r--r--.  1 root root 1702 фев 27 18:14 private.key
```
Конфиг nginx
```
server {
    listen 80;

    server_name _;

    return 301 https://$host$request_uri;
}


server {
    listen       443 ssl;
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;
    ssl_certificate      /etc/ssl/certificate01.crt;
    ssl_certificate_key  /etc/ssl/private.key;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        autoindex on;
    }
...
```

    
