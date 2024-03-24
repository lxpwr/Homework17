Настройка маршрутизации

inetRouter

Проверяем статус файрвола, чтобы был неактивен

systemctl status ufw

Создаем файл /etc/iptables_rules.ipv4 для настройки таблиц адресов, как в методичке:

Создаем файл для включения таблиц при перезагрузке

root@inetRouter:~# vi /etc/network/if-pre-up.d/iptables

Даем права на выполнение

chmod +x /etc/network/if-pre-up.d/iptables 

Включаем маршрутизацию:

root@inetRouter:~# echo "net.ipv4.conf.all.forwarding = 1" >> /etc/sysctl.conf

root@inetRouter:~# sysctl -p

Добавляем статические маршруты на все сети лаборатории:

root@inetRouter:~# ip route add 192.168.0.0/24 via 192.168.255.2

root@inetRouter:~# ip route add 192.168.1.0/24 via 192.168.255.2

root@inetRouter:~# ip route add 192.168.2.0/24 via 192.168.255.2

root@inetRouter:~# ip route add 192.168.255.0/24 via 192.168.255.2


centralRouter

Включаем роутинг:
root@centralRouter:~# echo "net.ipv4.conf.all.forwarding = 1" >> /etc/sysctl.conf

root@centralRouter:~# sysctl -p

Удаляем маршрут по умолчанию из dhcp, добавляя в /etc/netplan/00-installer-config.yaml

      dhcp4-overrides:

          use-routes: false

root@centralRouter:~# netplan try

Добавляем статические маршруты, включая дефолтный:

root@centralRouter:~#  ip route add 0.0.0.0/0 via 192.168.255.1

root@centralRouter:~#  ip route add 192.168.2.0/24 via 192.168.255.10

root@centralRouter:~#  ip route add 192.168.1.0/24 via 192.168.255.6


centralServer

Удаляем маршрут по умолчанию из dhcp, добавляя в /etc/netplan/00-installer-config.yaml

      dhcp4-overrides:

          use-routes: false

root@centralServer:~# netplan try

Добавляем статические маршруты, включая дефолтный:

root@centralServer:~# ip route add 0.0.0.0/0 via 192.168.0.1


office1Router

root@office1Router:~# echo "net.ipv4.conf.all.forwarding = 1" >> /etc/sysctl.conf

root@office1Router:~# sysctl -p

Удаляем маршрут по умолчанию из dhcp, добавляя в /etc/netplan/00-installer-config.yaml

      dhcp4-overrides:

          use-routes: false

root@office1Router:~# netplan try

Добавляем статические маршруты, включая дефолтный:

ip route add 0.0.0.0/0 via 192.168.255.9


office1Server

Удаляем маршрут по умолчанию из dhcp, добавляя в /etc/netplan/00-installer-config.yaml

      dhcp4-overrides:

          use-routes: false

root@office1Server:~# netplan try

Добавляем статические маршруты, включая дефолтный:

root@office1Server:~# ip route add 0.0.0.0/0 via 192.168.2.129


office2Router

root@office2Router:~# echo "net.ipv4.conf.all.forwarding = 1" >> /etc/sysctl.conf

root@office2Router:~# sysctl -p

Удаляем маршрут по умолчанию из dhcp, добавляя в /etc/netplan/00-installer-config.yaml

      dhcp4-overrides:

          use-routes: false

root@office2Router:~# netplan try

Добавляем статические маршруты, включая дефолтный:

root@office2Router:~# ip route add 0.0.0.0/0 via 192.168.255.5


root@office2Server

Удаляем маршрут по умолчанию из dhcp, добавляя в /etc/netplan/00-installer-config.yaml

      dhcp4-overrides:

          use-routes: false

root@office2Server:~# netplan try

Добавляем статические маршруты, включая дефолтный:

root@office2Server:~# ip route add 0.0.0.0/0 via 192.168.1.1

По сути, везде, кроме centralRouter, нужно добавить только дефолтные маршртуты

Выполнена автоматизация настроки стенда на Ansible, все файлы прилагаются