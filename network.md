<!-- -*- coding: utf-8; -*- -->

Network
=======

    ngrep -d lo | less

Set up
======

Interface
---------

<http://simms-teach.com/howtos/basic-network-config.html>

    ifconfig eth0 192.168.0.3 netmask 255.255.255.0

Gateway
-------

    route add default gw 192.168.0.1

DNS
---

    echo 'nameserver 192.168.0.2' > /etc/resolv.conf

Find out gateway
================

<http://cyberciti.biz/faq/how-to-find-gateway-ip-address>

    route | grep UG

Used ports
==========

    netstat -tnlp

Remote shell
============

    telnet mail.omskportal.ru 25

Telnet
======

    busybox telnet danil.kutkevich.org 22

SSH
===

    ssh -t root@santaslittlehelper "ssh danil@homer"
    scp -P 61022 [-r] foo.tar.gz bar.tar.gz anonymous@kutkevich.org:/home/danil/
    sshfs -p 61022 kutkevich.org:/home/danil/ mnt/kutkevich_org/

Key authentication
------------------

<http://superuser.com/questions/585429/can-i-change-the-filename-of-my-ssh-public-private-key-pair>

    ssh-keygen -f key-file-name
    ssh-copy-id -i ~/.ssh/key-file-name.pub "anonymous@kutkevich.org -p 2000"

Tunneling
---------

### SSH

<http://revsys.com/writings/quicktips/ssh-tunnel.html>

    ssh -f -N -L localhost:2000:homer:22 root@stampy
    ssh -p 2000 danil@localhost

### HTTP

#### Release terminal

    ssh -f -N -L localhost:3001:192.168.0.38:3000 medapp
    curl localhost:3001

#### Not release terminal

    ssh -L localhost:3001:192.168.0.38:3000 \
        -p 9922 danil@medapp2.waveaccess.ru

#### Reverse tunneling

##### SSH

<http://tunnelsup.com/raspberry-pi-phoning-home-using-a-reverse-remote-ssh-tunnel>

On `h4` SailfishOS:

    ssh -N -R localhost:55555:localhost:22 danil@h5.kutkevich.org

On `h5` server:

    ssh -l nemo -p 55555 localhost

Transparent multi-hop SSH agent forwarding
------------------------------------------

<http://sshmenu.sourceforge.net/articles/transparent-mulithop.html>

    ssh-agent
    ssh-add *pem
    ssh -A -t -p 9922 medapp2.waveaccess.ru ssh -A danil@192.168.0.38

Nmap
====

Discover (scanner) hosts and services on a computer network.

    nmap --open 217.197.232.218
    nmap -sP 192.168.0.0/16

Other
=====

    rtorrent -s ./.rtorrent
    host 192.168.132.44 192.168.8.1
    nslookup 172.16.81.4
    whois kutkevich.org
    dig kutkevich.org
    ifconfig -a
    ifconfig eth0.1 hw ether 00:1D:7E:55:19:D9
    dhclient
    ip r
    route del default gw 192.168.28.1 ath0
    route {add|del} [default] [gw 192.168.0.1] -net 172.16.0.0 \
          netmask 255.255.0.0 dev ppp0
    echo "1" > /proc/sys/net/ipv4/ip_forward
    pon darout
    poff darout
    iwlist ath0 scanning
    iwconfig ath0 essid John
    iwconfig ath0 key s:dfvgbh1234567
    killall wpa_supplicant && sleep 5 \
     && wpa_supplicant -i ath0 -c /etc/wpa_supplicant/wpa_supplicant.conf
    wvdial megafon
    curlftpfs -o "user=danil" kutkevich.org mnt/kutkevich_org/
    smbtree [-N] -d 2
    smbclient [-N] -L server
    smbclient -N "\\\\server\\store (e)"
    smbmount "//172.16.84.14/d$" mnt/cdp0002 \
     -o workgroup=darout,username=dkutkevich,iocharset=UTF-8,codepage=windows-1251
    rdesktop -g 99% -k en 192.168.91.5