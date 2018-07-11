# 资源列表
## 主机
- IN_gd-01_106.75.175.57	10.13.105.240
- OUT_hk-01_128.1.135.156	10.7.116.10

## udpn
- udpn-xxxxxx

# 出入口初始化及加速配置
## 入口
```shell
wget 103.14.35.151:7777/ugaa-scripts-master.zip
yum install telnet unzip zip nc iftop -y
unzip ugaa-scripts-master.zip
yum install nc ipvsadm lrzsz -y
tar -zxvf 2.6.32-431.11.32.el6.ucloud.x86_64.tar.gz
cd 2.6.32-431.11.32.el6.ucloud.x86_64
./install.sh
reboot
cd ugaa-scripts/
ifconfig
sh ugaa_init.sh -r IN -t 10.7.116.10
sh ugaa_init.sh -r IN -t 10.7.116.10 -c
cd ugaa-scripts/
ifconfig
sh ugaa_init.sh -r IN -t 10.7.57.137
sh ugaa_init.sh -r IN -t 10.7.57.137 -c
cd v3
sh path_in.sh --path_id upath-tfctfc --bandwidth 2
sh ugaa_in.sh --path_id=upath-tfctfc --ulb_ip=107.155.48.46 --dns=8.8.8.8 --iplist=52.200.109.142 --tcp_port=
```

## 出口
```shell
wget 103.14.35.151:7777/ugaa-scripts-master.zip
yum install telnet unzip zip nc iftop -y
unzip ugaa-scripts-master.zip
yum install nc ipvsadm lrzsz -y
tar -zxvf 2.6.32-431.11.32.el6.ucloud.x86_64.tar.gz
cd 2.6.32-431.11.32.el6.ucloud.x86_64
./install.sh
reboot
cd ugaa-scripts
ifconfig
sh ugaa_init.sh -r OUT -t 10.13.105.240
sh ugaa_init.sh -r OUT -t 10.13.105.240 -c
cd ugaa-scripts
ifconfig
sh ugaa_init.sh -r OUT -t 10.13.184.65
sh ugaa_init.sh -r OUT -t 10.13.184.65 -c
```

## 测试
telnet 107.155.48.46 80 // ulb ip

