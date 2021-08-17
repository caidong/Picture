---
title: Docker 部署CDH
date: 2021-07-10 23:12:43
tags:
- cdh
- 笔记
categories: 
- cdh
---
### 腾讯云主机硬盘挂载
**[腾讯文档链接](https://cloud.tencent.com/document/product/362/6734)**
1. 查看主机现有磁盘
```sh 
fdisk -l
```
2. 格式化硬盘成指定文件系统格式
```sh
mkfs -t ext4 /dev/vdb
```
3. 新建挂载点
```sh
mkdir /xdfdata
```
4. 新建分区挂载到新挂载点
```sh
mount /dev/vdb /xdfdata
```
5. 查看挂载结果
```sh 
df -TH
```
6. 开启开机自动挂载
```sh
vi /etc/fstab
# 添加如下信息
<设备信息> <挂载点> <文件系统格式> <文件系统安装选项> <文件系统转储频率> <启动时的文件系统检查顺序>
/dev/vdb            /data                 ext4       defaults              0 0
```
7. 检查是否开启成功
```
sudo mount -a
```
###### 一键脚本
```sh 
mkfs -t ext4 /dev/vdb
mkdir /xdfdata
mount /dev/vdb /xdfdata
echo '/dev/vdb            /xdfdata                 ext4       defaults              0 0' >> /etc/fstab
mount -a
```
### docker安装部署
- 安装脚本项目地址(https://github.com/docker/docker-install)

### 大数据集群主机搭建
1. 拉取基础镜像
    ```sh
    sudo docker pull centos:7.6.181
    ```
2. 启动容器
```sh
sudo docker run -d --privileged=true  -ti -v /etc/localtime:/etc/localtime:ro -v /xdfdata/iteach-105-cdh01:/opt --net="host" --name centos-iteach-cdh-105 -h iteach-cdh-105 centos:7.6.1810 /usr/sbin/init
sudo docker run -d --privileged=true  -ti -v /etc/localtime:/etc/localtime:ro -v /xdfdata/iteach-108-cdh01:/opt --net="host" --name centos-iteach-cdh-108 -h iteach-cdh-108 centos:7.6.1810 /usr/sbin/init
sudo docker run -d --privileged=true  -ti -v /etc/localtime:/etc/localtime:ro -v /xdfdata/iteach-100-cdh01:/opt --net="host" --name centos-iteach-cdh-100 -h iteach-cdh-100 centos:7.6.1810 /usr/sbin/init
```
2.1 进入容器
```
sudo docker container exec -it a45267773397 /bin/bash
2.2 设置免密登录
```
- 设置密码
passwd
- 设置免密
ssh-keygen -t rsa #三次回车
ssh-copy-id  -f -i ~/.ssh/id_rsa.pub cdh-master
ssh-copy-id  -f -i ~/.ssh/id_rsa.pub cdh-slave1
ssh-copy-id  -f -i ~/.ssh/id_rsa.pub cdh-slave2
ssh-copy-id  -f -i ~/.ssh/id_rsa.pub cdh-slave3


3. 安装部署cm
    3.1 安装依赖组件
```
yum install -y ntp mod_ssl openssh openssh-server openssh-clients /lib/lsb/init-functions httpd /usr/sbin/ss openssl-devel fuse-libs net-tools MySQL-python libpq.so.5 python-psycopg2 cyrus-sasl-plain openssl cyrus-sasl-gssapi fuse portmap bind-utils psmisc libxslt perl /sbin/service mysql-connector-java unzip zip
```
3.2 更改hosts
```
vi /etc/hosts
172.26.8.100     iteach-cdh-100
172.26.8.105     iteach-cdh-105
172.26.8.108     iteach-cdh-108
```
3.3 ntp 时间同步
**这种方式配置ntp的方式只适合cdh, ambari还有kudu集群对ntp要求更严**
```
yum install  -y ntp
vi /etc/ntp.conf 添加
server 0.cn.pool.ntp.org
server 1.cn.pool.ntp.org
server 2.cn.pool.ntp.org
server 3.cn.pool.ntp.org
systemctl enable ntpd  #开机自启
systemctl start ntpd
```
3.4 配置java环境
```
tar -zxf /docker/cdh-package/jdk-8u172-linux-x64.tar.gz -C /usr/java
export JAVA_HOME=/opt/java/jdk1.8.0_202
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
source ~/.bashrc
/data/cdh/java/jdk1.8.0_202/bin/java
```
3.5 创建用户与文件夹
```
mkdir /opt/cloudera-manager
tar -zxf /opt/CDH6.3.2/cm6.3.1-redhat7.tar.gz -C /opt/cloudera-manager/
mkdir /var/cloudera-scm-server
chown cloudera-scm:cloudera-scm /var/cloudera-scm-server
```
3.6 安装cm 
```
1. 主、从节点
rpm -ivh /opt/cloudera-manager/cm6.3.1/RPMS/x86_64/cloudera-manager-daemons-6.3.1-1466458.el7.x86_64.rpm
rpm -ivh /opt/cloudera-manager/cm6.3.1/RPMS/x86_64/cloudera-manager-agent-6.3.1-1466458.el7.x86_64.rpm

2. 主节点
rpm -ivh /opt/cloudera-manager/cm6.3.1/RPMS/x86_64/cloudera-manager-daemons-6.3.1-1466458.el7.x86_64.rpm
```
3.7 启动服务
```
systemctl start sshd
systemctl start cloudera-scm-server
systemctl start cloudera-scm-agent
```

3.8 其他文档
```
http://centos-iteach-100-cdh01:8080/opt/cloudera/parcels

vi /etc/cloudera-scm-agent/config.ini
server_host=cdh-master

centos-iteach-100-cdh01


cat >> /etc/yum.repos.d/cm.repo << EOF

[CM]
name=cm6
baseurl=http://iteach-cdh-100/cm6/
gpgcheck=0
EOF
页面安装的时候， 如果无法识别parcel库 重新生成.sha文件就可以了
sha1sum CDH-6.1.0-1.cdh6.1.0.p0.770702-el7.parcel| cut -d ' ' -f 1 > CDH-6.1.0-1.cdh6.1.0.p0.770702-el7.parcel.sha
/opt/cloudera/cm/schema/scm_prepare_database.sh mysql -h172.26.8.100  -uroot -piteach --scm-host 172.26.8.100  scmdbn root iteach
```

#### 参考文档
```
https://juejin.cn/post/6844904122731200525#heading-0
https://zhuanlan.zhihu.com/p/366308900

```