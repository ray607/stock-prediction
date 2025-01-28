# stock-prediction

# First (software)

linux (ubuntu) + centos-7.iso

open linux create name: centos & memory size 8096 $$ 20GB

setting add 4 CPU & 

詳細設定請參考以下連結

https://courses.openedu.tw/courses/course-v1:5GMB+SP+20020/courseware/3fe2b360d97549d5b54cb00efae59001/411bb71405034252b9ebdbaf484882a9/?activate_block_id=block-v1%3A5GMB%2BSP%2B20020%2Btype%40sequential%2Bblock%40411bb71405034252b9ebdbaf484882a9

https://github.com/xxionhong/network_slice/tree/main/experiment_4

and connect with cmd 
#cmd

```sh
ssh root@xxx.xxx.xxx.xxx
```

```sh
systemctl stop firewalld
```

```sh
systemctl disable firewalld
```

關閉防火牆

```sh
yum install -y centos-release-openstack-queens
```

```sh
yum update -y
```

```sh
yum install -y openstack-packstack
```

```sh
yum install -y vim
```

```sh
packstack --gen-answer-file answer.txt
```

出現Packstack changed given value  to required value /root/.ssh/id_rsa.pub正常

```sh
vim answer.txt
```

進到頁面後，開始編輯五種，首先利用'?名稱'，然後'.i'打字後，按esc跳出

1?DEFAULT_PASSWORD 打上 自己的密碼

2?NTP_SERVERS 打上 clock.stdtime.gov.tw

3?KEYSTONE_ADMIN_PW= 打上 自己的密碼

4?HEAT_IN 把 n 改成 y

5?PROVISION_DEMO 把 y 改成 n

:wq 存檔退出

```sh
packstack --answer-file answer.txt
```

大概要跑20分鐘左右，取決於電腦性能 ，完成帳號為admin，密碼為自己的密碼 登入

# opendaylight

```sh
sudo apt install -y openssh-server
```

```sh
wget https://nexus.opendaylight.org/content/repositories/opendaylight.release/org/opendaylight/integration/karaf/0.7.3/karaf-0.7.3.tar.gz
```
```sh
tar xvfz karaf-0.7.3.tar.gz
```
```sh
 sudo apt-get install openjdk-8-jdk openjdk-8-jre
```
```sh
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
```
```sh
cd karaf-0.7.3
```
```sh
./bin/karaf
```
```sh
feature:install odl-netvirt-openstack odl-dlux-core odl-mdsal-apidocs odl-l2switch-all odl-dluxapps-nodes odl-dluxapps-topology odl-dluxapps-applications odl-restconf-all odl-neutron-service
```
```sh
logout
```
```sh
vim etc/custom.properties
```
```sh
sudo apt install vim -y
```
```sh
vim etc/custom.properties
```
進到頁面後，開始編輯五種，首先利用'?名稱'，然後'.i'打字後，按esc跳出

1?DEFAULT_PASSWORD 打上 自己的密碼

2?NTP_SERVERS 打上 clock.stdtime.gov.tw

3?KEYSTONE_ADMIN_PW= 打上 自己的密碼

4?HEAT_IN 把 n 改成 y

5?PROVISION_DEMO 把 y 改成 n

:wq 存檔退出
```sh
vim etc/opendaylight/datastore/initial/config/netvirt-dhcpservice-config.xml
```
false -> true
```sh
vim etc/opendaylight/datastore/initial/config/netvirt-aclservice-config.xml
```
deny -> allow
```sh
./bin/start
```

//root//

```sh
systemctl stop NetworkManager
```
```sh
systemctl disable NetworkManager
```
```sh
systemctl stop neutron-server
```
```sh
systemctl stop neutron-l3-agent
```
```sh
systemctl stop neutron-openvswitch-agent
```
```sh
systemctl disable neutron-l3-agent neutron-openvswitch-agent
```
```sh
systemctl stop openvswitch
```
```sh
rm -rf /var/log/openvswitch/* /etc/openvswitch/conf.db:
```
```sh
systemctl start openvswitch
```
```sh
yum install python-networking-odl -y
```

```sh
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
```

快捷鍵 gg VG d

DEVICE=enp0s3

TYPE=OVSPort

DEVICETYPE=ovs

OVS_BRIDGE=br-ex

ONBOOT=yes  

```sh
vim /etc/sysconfig/network-scripts/ifcfg-br-ex
```

NBOOT=yes

IPADDR=172.20.10.5   //openstack所在的ip

PREFIX=24

GATEWAY=172.20.10.1

DNS1=8.8.8.8

DEVICE=br-ex

DEVICETYPE=ovs

TYPE=OVSBridge

BOOTPROTO=static

                     
```sh
systemctl restart network
```
```sh
ovs-vsctl set-manager tcp:172.20.10.7:6640; ovs-vsctl set Open_vSwitch . other_config:local_ip=172.20.10.5
```
```sh
ovs-vsctl set Open_vSwitch . Other_Config:providermappings=extent:br-ex
```
```sh
vim /etc/neutron/plugins/ml2/ml2_conf.ini
```
[ml2_odl]

username = admin

password = admin

url = https://172.20.10.7:8080/controller/nb/v2/neutron

port_binding_controller = pseudo-agentdb-binding

enable_dhcp_service = True

```sh
vim /etc/neutron/neutron.conf
```
```sh
vim /etc/neutron/dhcp_agent.ini
```
```sh
vim /etc/neutron/l3_agent.ini
```
```sh
mysql -e "DROP DATABASE IF EXISTS neutron;";mysql -e "CREATE DATABASE neutron CHARACTER SET utf8;"
```
```sh
/usr/bin/neutron-db-manage --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/plugins/ml2/ml2_conf.ini upgrade heads
```
```sh
/usr/share/openvswitch/scripts/ovs-ctl start
```
```sh
neutron-odl-ovs-hostconfig --datapath_type=system
```
```sh
systemctl restart neutron-server neutron-dhcp-agent
```



#Tracker

//ray//
```sh
ps aux | grep karaf
```
//root//

```sh
mysql -uroot
```
```sh
CREATE DATABASE tacker;
```
```sh
GRANT ALL PRIVILEGES ON tacker.* TO 'tacker'@'localhost' IDENTIFIED BY 'zxcv01251';
```
```sh
GRANT ALL PRIVILEGES ON tacker.* TO 'tacker'@'%' IDENTIFIED BY 'zxcv01251';
```
```sh
flush privileges;
```
```sh
exit;
```
```sh
source /root/keystonerc_admin
```
```sh
openstack user create --domain default --password zxcv01251 tacker
```
```sh
openstack role add --project services --user tacker admin
```
```sh
openstack service create --name tacker --description "Tacker Project" nfv-orchestration
```
```sh
openstack endpoint create --region RegionOne nfv-orchestration public http://172.20.10.5:9890/
```
```sh
openstack endpoint create --region RegionOne nfv-orchestration internal http://172.20.10.5:9890/
```
```sh
openstack endpoint create --region RegionOne nfv-orchestration admin http://172.20.10.5:9890/
```
```sh
yum install python2-tackerclient.noarch
```
```sh
vim /etc/tacker/tacker.conf
```
快捷鍵 gg VV GG dd

[default]

auth_strategy = keystone

policy_file = /etc/tacker/policy.json

debug = True

use_syslog = 192.168.1.104

bind_port = 9890

service_plugins = nfvo,vnfm

state_path = /var/lib/tacker

transport_url = rabbit://openstack:zxcv01251@192.168.1.104

[keystone_authtoken]

www_authenticate_url = http://192.168.1.104:5000

auth_url = http://192.168.1.104:5000

memcached_servers = 192.168.1.104:11211

auth_type = password

project_domain_name = default

project_name = services

username = tacker

password = zxcv01251

token_cache_time = 3600

[database]

connection = mysql+pymysql://tacker:zxcv01251@192.168.1.104/tacker

[nfvo_vim]

vim_drivers = openstack

[tacker]

monitor_driver = ping,http_ping.i

```sh
/usr/bin/tacker-db-manage --config-file /etc/tacker/tacker.conf upgrade head
```

```sh
git clone https://github.com/openstack/tacker-horizon.git
```
```sh
cd tacker-horizon
```
```sh
vim PKG-INFO
```
Metadata-Version: 0.8.0
Name: tacker-horizon
Version: 0.8.0
Summary: Tacker project for OpenStack
Home-page: http://docs.openstack.org/developer/tacker/
Author: OpenStack
Author-email: openstack-dev@lists.openstack.org
License: UNKNOWN
Descriptiom: ======
Platform: UNKNOWN


```sh
python setup.py install
```

```sh
cp tacker_horizon/enabled/_80_nfv.py /usr/share/openstack-dashboard/openstack_dashboard/enabled/
```
```sh
systemctl restart httpd
```
```sh
systemctl restart openstack-tacker-server.service
```
```sh
systemctl enable openstack-tacker-server.service
```
```sh
mkdir -p /etc/tacker/vim/fernet_keys
```
```sh
chown tacker:tacker /etc/tacker/* -R
```
```sh
vim /etc/tacker/config.yaml
```
auth_url: 'http://192.168.1.104:5000/v3'
username: 'admin'
password: 'zxcv01251'
project_name: 'admin'
project_domain_name: 'Default'
user_domain_name: 'Default'
cert_verify: 'True'

```sh
openstack vim register --config-file /etc/tacker/config.yaml --description 'my first vim' --is-default tacker_vim
```

