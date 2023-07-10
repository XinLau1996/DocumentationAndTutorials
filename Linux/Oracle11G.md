# Centos8 ������ Oracle 11 G �İ�װ���� #
### ע�⿴�ҵı��⣡��������������� Oracle Database 11g �� 2 �� (11.2.0.1.0) �汾 ###
## һ����鱾���Ƿ�װ ##
### 1��ϵͳ���ü�� ###
> ##### �����ڴ治С��1G: �鿴��ʽ: #####
    grep MemTotal /proc/meminfo
> ##### ����Ӳ�̲�С��8G: �鿴��ʽ: #####
    df
> ##### Swap�����ռ䲻С��2G: �鿴��ʽ: #####
    grep SwapTotal /proc/meminfo
![���ü��](image/33.png)
### 2�� �޸� `CentOS` ϵͳ��ʶ �� `Oracle` Ĭ�ϲ�֧�� `CentOS`��  ###
    vim /etc/redhat-release
> �޸�Ϊ `redhat-8` 

### 3���޸��ں˲��� ###
    vim /etc/sysctl.conf
> ׷����������
```
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
kernel.shmall = 2097152
kernel.shmmax = 2147483648
net.ipv4.ip_local_port_range = 9000 65500
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.all.rp_filter = 1
net.core.rmem_default = 262144
net.core.rmem_max= 4194304
net.core.wmem_default= 262144
net.core.wmem_max= 1048576
```
> �޸����,�����µ�����:
```
sysctl -p
```
![�޸� `CentOS` ϵͳ��ʶ �Լ� �ں˲���](image/34.png)
### 4����װ��������� ###
> ��װ epel Դ
```shell
yum install epel-release
```

> ��װ�������
```shell
yum -y install libnsl
```

```shell
yum -y install binutils* compat-libcap1 compat-libstdc++-33 gcc* gcc-c++* glibc* glibc-devel* ksh* libaio* libaio-devel* libgcc* libstdc++* libstdc++-devel* libXi* libXtst* make* sysstat* elfutils* unixODBC* unzip lrzsz
```

## �������� ##
### 1�����ص�ַ ###
    https://www.oracle.com/cn/database/enterprise-edition/downloads/oracle-db11g-linux.html
##### ע����Ҫ��¼ Oracle �ʺţ� �������֮���ϴ� Centos #####
## ������ѹ�����ļ� ##
### 1���л��� `/usr/local/` �ļ�����
```shell
cd /usr/local/
```
> ���������ݿ��ļ�ѹ�����ϴ�����Ŀ¼��
### 2����ѹ ###
```shell
unzip -d /usr/local/ linux.x64_11gR2_database_1of2.zip
unzip -d /usr/local/ linux.x64_11gR2_database_2of2.zip
```
### 3�������û��顢�û� ###
#### ������װ `Oracle` �����û��� ####
```shell
groupadd oinstall
```
#### ���� `DBA` �û���
```shell
groupadd dba
```
#### ���� `oracle` �û������� 'DBA' �û���
```shell
useradd -g dba -m oracle
```
#### ���û� `oracle` ���뵽  `oinstall` �� ####
```shell
usermod -a -G oinstall oracle
```
#### �޸��û� `oracle` ������ ####
```shell
passwd oracle
```
#### �޸�Ŀ¼Ȩ�� ####
```shell
chown -R oracle:oinstall /usr/local/database
chown -R oracle:oinstall /usr/local/oracle
```
#### �޸� `oracle` �û��İ�ȫ�������� ####
```shell
vim /etc/security/limits.conf
```
> �����һ��ǰ׷���������ݣ�
```shell
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
```
#### �޸��û��������� #### **************
```shell
vim /home/oracle/.bashrc
```
> ׷����������
```shell
export PATH
export ORACLE_BASE=/usr/local/oracle
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1
export ORACLE_SID=orcl
export ORACLE_UNQNAME=orcl
export PATH=$ORACLE_HOME/bin:/usr/sbin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export LANG=C0
export NLS_LANG=AMERICAN_AMERICA.AL32UTF8
```
> �޸������������:
```shell
source /home/oracle/.bashrc
```
#### �ر� `selinux` ####
```shell
/etc/selinux/config
```
> �޸���������
```shell
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled      # ********�޸Ĵ���******
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```
> �������Ҫ��������
```shell
reboot
```



### 2������ǽ�˿ڿ��� ###
#### ��� ####
##### 1���鿴����ǽ״̬ #####
	firewall-cmd --state
##### 2���رշ���ǽ #####
	systemctl stop firewalld.service
##### 3����ֹ����ǽ�������� #####
	systemctl disable firewalld.service
##### 4����������ǽ #####
	systemctl start firewalld.service
##### 5��Add ��ӿ��Ŷ˿� #####
	firewall-cmd --permanent --zone=public --add-port=3306/tcp
##### 6��Reload ���¼��� #####
	firewall-cmd --reload
##### 7������Ƿ���Ч ######
	firewall-cmd --zone=public --query-port=3306/tcp
##### 8��ɾ�����Ŷ˿� ######
    firewall-cmd --zone=public --remove-port=8088/tcp --permanent
### 3��`temporary failure in name resolution` ���⣬�����Ƿ�����������������Ч���ر�����������MySQL\MyCat\TomCat�� ###
