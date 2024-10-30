# Configure the NFS Server

## ติดตั้ง NFS Server
```bash
apt update
apt install nfs-kernel-server
```

## สร้าง Directory สำหรับ Sharefile
```bash
mkdir -p /srv/nfs_share
chmod 755 /srv/nfs_share
```

## ตั้งค่า NFS
เปิดไฟล์ /etc/exports
```bash
nano /etc/exports
```
ตั้งค่า
```bash
/srv/nfs_share [client_IP](rw,sync,no_subtree_check)
```
- `rw` – สิทธิ์ในการ read/write
- `sync` – บังคับ NFS ให้เขียนการเปลี่ยนแปลงลงในดิสก์ก่อนที่จะตอบกลับ
- `no_subtree_check` – เพิ่มความน่าเชื่อถือ
เช่น
```bash
/srv/nfs_share 192.168.1.100(rw,sync,no_subtree_check)
```
Apply การตั้งค่า
```bash
exportfs -a
```

## เปิดใช้งาน NSF Server
```bash
systemctl start nfs-kernel-server
systemctl enable nfs-kernel-server
```

## ตั้งค่า Firewall
```bash
ufw allow from [client_IP] to any port nfs
```

## ตั้งค่า NFS Client
เครื่อง Client ที่ต้องการเชื่อต่อ Sharefile เริม่จากการติดตั้ง nfs-common
```bash
apt install nfs-common
```

สร้าง Directoty สร้าง mount
```bash
mkdir -p /mnt/nfs_share
```

ทำการ Mount 
```bash
mount [client_IP]:/srv/nfs_share /mnt/nfs_share
```

ตั้งค่าให้เครื่องมีทำการ Reboot และยังสามารถ mount ได้อัตโนมัติ
```bash
nano /etc/fstab
```

```bash
[server_IP]:/srv/nfs_share /mnt/nfs_share nfs defaults 0 0
```



