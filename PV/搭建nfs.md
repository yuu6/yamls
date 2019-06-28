# 搭建NFS

## 步骤
```shell
#master节点安装nfs
yum -y install nfs-utils

#创建nfs目录
mkdir -p /nfs/data/

#修改权限
chmod -R 777 /nfs/data

#编辑export文件
vim /etc/exports
/nfs/data *(rw,no_root_squash,insecure, sync)

#配置生效
exportfs -r
#查看生效
exportfs

#启动rpcbind、nfs服务
systemctl restart rpcbind && systemctl enable rpcbind
systemctl restart nfs && systemctl enable nfs

#查看 RPC 服务的注册状况
rpcinfo -p localhost

#showmount测试
showmount -e 192.168.206.132

#所有node节点安装客户端
yum -y install nfs-utils
systemctl start nfs && systemctl enable nfs


```


## 创建两个pv

```$xslt
#创建pv卷对应的目录
mkdir -p /nfs/data/pv001
mkdir -p /nfs/data/pv002

#配置exportrs
vim /etc/exports
/nfs/data *(rw,no_root_squash,insecure, sync)
/nfs/data/pv001 *(rw,no_root_squash,insecure, sync)
/nfs/data/pv002 *(rw,no_root_squash,insecure, sync)

#配置生效
exportfs -r
#重启rpcbind、nfs服务
systemctl restart rpcbind && systemctl restart nfs

```
