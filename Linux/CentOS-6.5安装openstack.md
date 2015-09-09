### CentOS-6.5安装openstack

#### 1. 安装keystone

rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm

Upgrade RDO Juno EL6:
https://wiki.centos.org/Cloud/OpenStack/JunoEL6QuickStart

	rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

	rpm -ivh http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm

	rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

	yum update

	yum clean all

	yum makecache

	yum install centos-release-openstack.noarch

	yum install openstack-utils openstack-keystone python-keystoneclient

安装mysql

	yum install mysql mysql-server

/usr/bin/mysql_secure_installation设置用户名和密码

创建keystone用户

CREATE USER keystone IDENTIFIED BY 'keystone';