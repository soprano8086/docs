### setup nfs server, redhat

```
yum install -y nfs-utils

mkdir /data
chown nfsnobody:nfsnobody /data
chmod -R 777 /data
echo "for testing" > /data/fortest"

echo -n "/data *(rw,sync,no_root_squash,anonuid=1000,anongid=2000)" >> /etc/exports
cat /etc/exports
#exportfs -r

systemctl enable rpcbind
systemctl enable nfs-server
systemctl start rpcbind
systemctl start nfs-server

echo "firewall-cmd --permanent --zone=public --add-service={nfs,mountd,rpc-bind} and reload "
firewall-cmd --permanent --zone=public --add-service={nfs,mountd,rpc-bind}
firewall-cmd --reload

```
### setup nfs client, redhat
```
yum install -y nfs-utils; showmount -e ${NFS_SERVER_IP}
mkdir /data
echo '${NFS_SERVER_IP}:/data /data nfs defaults,_netdev 0 0' >> /etc/fstab
mount -a
ls /data


```
