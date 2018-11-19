<h1>Setting up NFS on kube nodes so we can provide shared storage to all nodes in the cluster</h1>

Create directory to be shared - in this case using `/opt/data` which is mounted on a USB (`/dev/sda1`)<br/>
Note to self: Make sure USB is mounted in /etc/fstab:<br/>
`/dev/sda1 /opt/data *check options*`

<h2>Setup NFS server on master node</h2>

Install NFS server, NFS common and RPC bind:</br>
`apt-get install nfs-kernel-server nfs-common rpcbind -y`

Add directory to be shared to `/etc/exports`:</br>
`echo "/opt/data 192.168.1.0/24 *(rw,sync,no_root_squash,no_all_squash)"  >> /etc/exports`</br>
The above makes the directory accessible to any device on our 192.168.1.* network</br> (this is not recommended for production environments)

enable rpc-bind and start nfs-server (RPi uses nfs-kernel-server):</br>
`systemctl enable rpc-bind && systemctl start nfs-kernel-server`

Ensure shared directory is being shared via NFS with the following command:</br>
`exportfs`

<h2> Setup NFS clients on remaining nodes</h2>

Install nfs-common</br>
`apt-get install nfs-common`

Create a local directory to mount the remote directory to and mount:</br>
`mkdir -p /mnt/data`</br>
`mount ip.ad.dr.ess:/opt/data /mnt/data`

Add this mount to `/etc/fstab` to ensure it is automatically mounted on reboot:</br>
`echo "ip.ad.dr.ess:/opt/data /mnt/data nfs rw,sync,hard,intr 0 0" >> /etc/fstab`
