# 📘 **L2 Linux Administrator Handbook**

*Intermediate Linux System Administration Guide*

---

# **1. Overview**

The **L2 Linux Administrator** role focuses on deeper system management, troubleshooting complex issues, performance tuning, networking, automation, and working with production-grade environments.
This handbook builds on L1 fundamentals and prepares you for real-world support scenarios.

---

# **2. Users, Groups & Permissions (Advanced)**

## **2.1 ACLs (Access Control Lists)**

```bash
# View ACL
getfacl filename

# Set ACL for user
setfacl -m u:john:rwx filename

# Set ACL for group
setfacl -m g:devteam:rx filename

# Remove ACL
setfacl -b filename
```

## **2.2 Special Permissions**

### **SUID**

```bash
chmod u+s file
```

### **SGID**

```bash
chmod g+s dir
```

### **Sticky Bit**

```bash
chmod +t dir
```

---

# **3. Advanced File System Management**

## **3.1 LVM (Logical Volume Manager)**

### Create LVM:

```bash
pvcreate /dev/sdb
vgcreate vg_data /dev/sdb
lvcreate -L 10G -n lv_backup vg_data
mkfs.ext4 /dev/vg_data/lv_backup
mount /dev/vg_data/lv_backup /backup
```

### Extend LVM:

```bash
lvextend -L +5G /dev/vg_data/lv_backup
resize2fs /dev/vg_data/lv_backup
```

### Reduce LVM (careful!):

```bash
umount /backup
e2fsck -f /dev/vg_data/lv_backup
resize2fs /dev/vg_data/lv_backup 8G
lvreduce -L 8G /dev/vg_data/lv_backup
```

---

# **4. Systemd & Service Management (Advanced)**

## **4.1 Analyze boot-related issues**

```bash
systemd-analyze
systemd-analyze blame
journalctl -b
```

## **4.2 Service debugging**

```bash
systemctl status service
journalctl -u service
systemctl restart service
```

## **4.3 Create a systemd service**

`/etc/systemd/system/myapp.service`

```ini
[Unit]
Description=My Application Service

[Service]
ExecStart=/usr/local/bin/myapp
Restart=always
User=appuser

[Install]
WantedBy=multi-user.target
```

Enable:

```bash
systemctl enable --now myapp
```

---

# **5. Networking (Intermediate–Advanced)**

## **5.1 Network Diagnostics**

```bash
ss -tulnp      # Show listening ports
ip a           # Show interfaces
ip r           # Show routes
dig example.com
tcpdump -i eth0 port 80
```

## **5.2 Firewall management**

### Firewalld:

```bash
firewall-cmd --add-port=8080/tcp --permanent
firewall-cmd --reload
```

### UFW:

```bash
ufw allow 22/tcp
ufw status
```

---

# **6. Storage, Disks & Backup Strategies**

## **6.1 Manage mounting**

```bash
lsblk
blkid
mount -a
```

## **6.2 fstab example**

```
UUID=xxxx-xxxx /data ext4 defaults 0 2
```

## **6.3 Backup with rsync**

```bash
rsync -avz /data /backup
```

---

# **7. Package Management & Repositories**

### **Debian/Ubuntu**

```bash
apt update
apt install package
apt remove package
dpkg -l
```

### **RHEL/CentOS/almalinux**

```bash
yum install package
dnf search package
rpm -qa
```

---

# **8. Shell Scripting (Intermediate)**

## **8.1 Functions**

```bash
hello() {
  echo "Hello $1"
}
hello John
```

## **8.2 Script debugging**

```bash
bash -x script.sh
```

## **8.3 Cron automation**

```bash
crontab -e
30 2 * * * /scripts/backup.sh
```

---

# **9. Security Hardening (L2 Level)**

* Disable unused services
* Enforce strong SSH configs
  `/etc/ssh/sshd_config`

```bash
PermitRootLogin no
PasswordAuthentication no
```

* Keep software updated
* Use fail2ban (SSH brute force protection)
* Verify file integrity using `aide` or `tripwire`

---

# **10. Troubleshooting Scenarios**

## **10.1 Disk full**

```bash
df -h
du -sh /*
find / -type f -size +500M
journalctl --vacuum-size=100M
```

## **10.2 High CPU**

```bash
top
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu
```

## **10.3 Application not starting**

Checklist:

* Service status
* Logs
* Permissions
* SELinux
* Port conflicts
* Dependencies

---

# **11. SELinux (Basic L2)**

## Check mode:

```bash
getenforce
```

## Change mode:

```bash
setenforce 0     # permissive
setenforce 1     # enforcing
```

## Logs:

```bash
sealert -a /var/log/audit/audit.log
```

---

# **12. Docker Basics for L2 Admins**

### Check containers:

```bash
docker ps -a
```

### Logs:

```bash
docker logs container_name
```

### Restart container:

```bash
docker restart container_name
```

### Inspect container:

```bash
docker inspect container_name
```

---

# **13. Monitoring & Performance**

### CPU

```bash
mpstat 1
```

### Memory

```bash
free -h
```

### IO

```bash
iotop
```

### Services load

```bash
systemctl list-units --failed
```

---

# **14. Important Logs to Check**

* `/var/log/messages`
* `/var/log/secure`
* `/var/log/dmesg`
* `/var/log/cron`
* `journalctl -xe`

---

# **15. Common L2 Interview/Practical Questions**

* Explain the boot process (BIOS → GRUB → Kernel → init/systemd)
* Diagnose high load average
* Add a disk, partition it, mount it persistently
* Fix a service stuck in “failed” state
* Configure SSH security
* Troubleshoot DNS issues
* Extend LVM on a running server

